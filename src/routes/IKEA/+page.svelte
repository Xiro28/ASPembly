<script lang="ts">
	import { onMount, onDestroy } from 'svelte';
	import { Engine } from '@babylonjs/core/Engines/engine';
	import { Scene } from '@babylonjs/core/scene';
	import { Vector3 } from '@babylonjs/core/Maths/math.vector';
	import { Color3, Color4 } from '@babylonjs/core/Maths/math.color';
	import { ArcRotateCamera } from '@babylonjs/core/Cameras/arcRotateCamera';
	import { HemisphericLight } from '@babylonjs/core/Lights/hemisphericLight';
	import { DirectionalLight } from '@babylonjs/core/Lights/directionalLight';
	import { ShadowGenerator } from '@babylonjs/core/Lights/Shadows/shadowGenerator';
	import '@babylonjs/core/Lights/Shadows/shadowGeneratorSceneComponent';
	import { MeshBuilder } from '@babylonjs/core/Meshes/meshBuilder';
	import type { Mesh } from '@babylonjs/core/Meshes/mesh';
	import { TransformNode } from '@babylonjs/core/Meshes/transformNode';
	import { StandardMaterial } from '@babylonjs/core/Materials/standardMaterial';
	import '@babylonjs/core/Rendering/edgesRenderer';

	// ── Types ──────────────────────────────────────────────────────────────────
	type Cmd = 'pick' | 'move' | 'align' | 'insert' | 'place' | 'rotate' | 'release';

	interface RobotCommand {
		step: number;
		order: number;
		cmd: Cmd;
		objectId: string;
		from: string;
		to: string;
		description: string;
		repIndex: number;
		repCount: number;
	}

	interface ParsedProgram {
		steps: number[];
		repetitions: Record<number, number>;
		commands: RobotCommand[];
	}

	interface SceneObj {
		id: string;
		root: TransformNode;
		meshes: Mesh[];
		currentPos: Vector3;
		mat: StandardMaterial;
	}

	// ── Default ASP input ─────────────────────────────────────────────────────
	const DEFAULT_ASP = `% ÄPPLARÖ Bench assembly

step(1).
repetition(1,6).
robot_command(1,1,pick,dowel,table,none,pick_up_one_of_the_6_wooden_dowels).
robot_command(1,2,move,dowel,table,frame_hole,move_the_dowel_towards_the_pre_drilled_hole_on_the_frame).
robot_command(1,3,align,dowel,none,frame_hole,align_the_dowel_with_the_hole).
robot_command(1,4,insert,dowel,none,frame_hole,insert_the_dowel_into_the_hole).
robot_command(1,5,release,dowel,none,none,release_the_dowel_now_seated_in_the_frame).

step(2).
repetition(2,2).
robot_command(2,1,pick,side_frame,table,none,pick_up_the_side_frame).
robot_command(2,2,move,side_frame,table,side_of_table_top,move_the_side_frame_towards_its_end_of_the_seat_panel).
robot_command(2,3,align,side_frame,none,side_of_table_top,align_the_frame_dowels_with_the_seat_holes).
robot_command(2,4,insert,side_frame,none,side_of_table_top,push_the_side_frame_onto_the_seat_panel_dowels).
robot_command(2,5,release,side_frame,none,none,release_the_side_frame).

step(3).
repetition(3,4).
robot_command(3,1,pick,screw_long,table,none,pick_up_one_of_the_4_long_hex_bolts).
robot_command(3,2,move,screw_long,table,screw_hole,move_the_bolt_to_the_corner_hole_on_the_frame).
robot_command(3,3,align,screw_long,none,screw_hole,align_the_bolt_with_the_hole).
robot_command(3,4,insert,screw_long,none,screw_hole,insert_the_bolt_into_the_hole).
robot_command(3,5,pick,allen_key,table,none,pick_up_the_allen_key).
robot_command(3,6,move,allen_key,table,screw_head,move_the_allen_key_to_the_bolt_head).
robot_command(3,7,align,allen_key,none,screw_head,align_the_allen_key_with_the_bolt_head).
robot_command(3,8,rotate,allen_key,none,none,rotate_the_allen_key_to_tighten_the_bolt).
robot_command(3,9,release,allen_key,none,none,release_the_allen_key).

step(4).
repetition(4,2).
robot_command(4,1,pick,screw_medium,table,none,pick_up_one_of_the_2_medium_screws).
robot_command(4,2,move,screw_medium,table,screw_hole,move_the_screw_to_the_frame_hole).
robot_command(4,3,align,screw_medium,none,screw_hole,align_the_screw_with_the_hole).
robot_command(4,4,insert,screw_medium,none,screw_hole,insert_the_screw_into_the_frame).
robot_command(4,5,pick,allen_key,table,none,pick_up_the_allen_key).
robot_command(4,6,move,allen_key,table,screw_head,bring_the_allen_key_to_the_screw_head).
robot_command(4,7,align,allen_key,none,screw_head,align_the_allen_key_with_the_screw_head).
robot_command(4,8,rotate,allen_key,none,none,rotate_to_tighten).
robot_command(4,9,release,allen_key,none,none,release_the_allen_key).

step(5).
repetition(5,4).
robot_command(5,1,pick,screw,table,none,pick_up_one_of_the_4_wood_screws).
robot_command(5,2,move,screw,table,screw_hole,move_the_screw_to_the_hole_on_the_frame).
robot_command(5,3,align,screw,none,screw_hole,align_the_screw_with_the_hole).
robot_command(5,4,insert,screw,none,screw_hole,insert_the_screw).
robot_command(5,5,rotate,screw,none,none,rotate_the_screw_to_tighten_it).
robot_command(5,6,release,screw,none,none,release_the_screw_after_tightening).
`;

	// ── ASP Parser ─────────────────────────────────────────────────────────────
	const SUPPORTED_CMDS: Cmd[] = ['pick', 'move', 'align', 'insert', 'place', 'rotate', 'release'];

	function stripComment(line: string) {
		let inQ = false, r = '';
		for (const c of line) {
			if (c === '"') { inQ = !inQ; r += c; continue; }
			if (c === '%' && !inQ) break;
			r += c;
		}
		return r.trim();
	}

	function splitArgs(s: string) {
		const r: string[] = []; let cur = '', inQ = false;
		for (const c of s) {
			if (c === '"') { inQ = !inQ; cur += c; continue; }
			if (c === ',' && !inQ) { r.push(cur.trim()); cur = ''; continue; }
			cur += c;
		}
		if (cur.trim()) r.push(cur.trim());
		return r;
	}

	function stripQ(v: string) {
		const t = v.trim();
		return t.startsWith('"') && t.endsWith('"') ? t.slice(1, -1) : t;
	}

	function humanize(v: string) {
		const s = stripQ(v).replace(/_/g, ' ').replace(/\s+/g, ' ').trim();
		return s ? s.charAt(0).toUpperCase() + s.slice(1) : '';
	}

	function parseProgram(input: string): ParsedProgram {
		const steps = new Set<number>();
		const repetitions: Record<number, number> = {};
		const raw: Omit<RobotCommand, 'repIndex' | 'repCount'>[] = [];

		for (const rawLine of input.split('\n')) {
			const line = stripComment(rawLine);
			if (!line) continue;

			const stepM = line.match(/^step\s*\((\d+)\)\s*\.?$/);
			if (stepM) { steps.add(+stepM[1]); continue; }

			const repM = line.match(/^repetition\s*\((\d+)\s*,\s*(\d+)\)\s*\.?$/);
			if (repM) { repetitions[+repM[1]] = +repM[2]; steps.add(+repM[1]); continue; }

			const rcM = line.match(/^robot_command\s*\((.*)\)\s*\.?$/);
			if (rcM) {
				const a = splitArgs(rcM[1]);
				if (a.length !== 7) throw new Error(`robot_command needs 7 args: ${line}`);
				const cmd = a[2].trim() as Cmd;
				if (!SUPPORTED_CMDS.includes(cmd)) throw new Error(`Unknown cmd: ${cmd}`);
				raw.push({
					step: +a[0], order: +a[1], cmd,
					objectId: a[3].trim(), from: a[4].trim(),
					to: a[5].trim(), description: humanize(a[6])
				});
				steps.add(+a[0]);
				continue;
			}
			// ignore unknown facts silently
		}

		if (raw.length === 0) throw new Error('No robot_command found.');

		// Expand repetitions
		const expanded: RobotCommand[] = [];
		const sorted = [...raw].sort((a, b) => a.step - b.step || a.order - b.order);
		for (const c of sorted) {
			const repCount = repetitions[c.step] ?? 1;
			for (let ri = 1; ri <= repCount; ri++) {
				const oid = repCount > 1 ? `${c.objectId}_${ri}` : c.objectId;
				expanded.push({ ...c, objectId: oid, repIndex: ri, repCount });
			}
		}

		return {
			steps: [...steps].sort((a, b) => a - b),
			repetitions,
			commands: expanded.sort((a, b) => a.step - b.step || a.repIndex - b.repIndex || a.order - b.order)
		};
	}

	// ── State ─────────────────────────────────────────────────────────────────
	let canvas: HTMLCanvasElement;
	let engine: Engine | null = null;
	let scene: Scene | null = null;
	let shadowGen: ShadowGenerator | null = null;

	let aspText = $state(DEFAULT_ASP);
	let parseError = $state('');
	let program: ParsedProgram = $state(parseProgram(DEFAULT_ASP));

	// Playback
	let cmdIndex = $state(-1);           // -1 = before start
	let isPlaying = $state(false);
	let statusText = $state('Ready — press Play or step through');

	// Scene objects registry
	const sceneObjects = new Map<string, SceneObj>();
	let gripperNode: TransformNode | null = null;
	let gripperL: Mesh | null = null;
	let gripperR: Mesh | null = null;
	let heldId: string | null = null;

	// Derived
	const flatCmds = $derived(program.commands);
	const totalCmds = $derived(flatCmds.length);

	// Group commands by step for the sidebar
	const stepGroups = $derived(() => {
		const groups: { step: number; repCount: number; cmds: RobotCommand[] }[] = [];
		const seen = new Map<number, typeof groups[0]>();
		for (const c of flatCmds) {
			const key = c.step * 1000 + c.repIndex;
			if (!seen.has(key)) {
				const g = { step: c.step, repCount: c.repCount, cmds: [] as RobotCommand[] };
				groups.push(g); seen.set(key, g);
			}
			seen.get(key)!.cmds.push(c);
		}
		return groups;
	});

	// ── Material helpers ───────────────────────────────────────────────────────
	const MAT_WOOD    = '#b8824a';   // warm brown (wood)
	const MAT_METAL   = '#909098';   // neutral steel grey
	const MAT_TOOL    = '#3a3a5c';   // dark tool colour
	const MAT_FLOOR   = '#d4c9b0';   // beige floor
	const MAT_ACTIVE  = '#ff8c00';   // orange  — currently active
	const MAT_DONE    = '#3a8cff';   // blue    — assembled/done

	function hexToColor3(hex: string): Color3 {
		const r = parseInt(hex.slice(1, 3), 16) / 255;
		const g = parseInt(hex.slice(3, 5), 16) / 255;
		const b = parseInt(hex.slice(5, 7), 16) / 255;
		return new Color3(r, g, b);
	}

	function makeMat(name: string, hex: string): StandardMaterial {
		const m = new StandardMaterial(name, scene!);
		m.diffuseColor  = hexToColor3(hex);
		m.specularColor = new Color3(0.15, 0.15, 0.15);
		m.emissiveColor = Color3.Black();
		m.ambientColor  = new Color3(0.1, 0.1, 0.1);
		return m;
	}

	// ── Part type inference ────────────────────────────────────────────────────
	type PartKind =
		| 'screw' | 'screw_medium' | 'screw_long'
		| 'dowel' | 'panel' | 'side_frame' | 'leg'
		| 'washer' | 'barrel_nut' | 'nut' | 'allen_key';

	function inferKind(id: string): PartKind {
		// Strip repetition suffix (_1, _2, …) then match on canonical name
		const n = id.replace(/_\d+$/, '').toLowerCase();
		if (n === 'screw_long'  || n.includes('screw_long'))   return 'screw_long';
		if (n === 'screw_medium'|| n.includes('screw_medium')) return 'screw_medium';
		if (n === 'screw'       || n.includes('screw'))        return 'screw';
		if (n === 'dowel'       || n.includes('dowel'))        return 'dowel';
		if (n === 'panel'       || n.includes('panel'))        return 'panel';
		if (n === 'side_frame'  || n.includes('side_frame') || n.includes('frame')) return 'side_frame';
		if (n === 'leg'         || n.includes('leg'))          return 'leg';
		if (n === 'barrel_nut'  || n.includes('barrel'))       return 'barrel_nut';
		if (n === 'washer'      || n.includes('washer'))       return 'washer';
		if (n === 'nut'         || n.includes('nut'))          return 'nut';
		if (n === 'allen_key'   || n.includes('allen'))        return 'allen_key';
		return 'screw';
	}

	// ── Bench geometry constants ───────────────────────────────────────────────
	// Assembled bench: 3.0 long (X) × 1.1 wide (Z) × 0.45 tall (Y)
	// Frames sit at y=0 with legs 0.45 tall → seat panel centre at y = 0.45 + 0.025
	const SEAT_Y    = 0.45;    // leg height = seat bottom height
	const BENCH_HALF = 1.40;   // half bench length — frames sit just inside the seat ends
	const FRAME_X_L  = -BENCH_HALF;
	const FRAME_X_R  =  BENCH_HALF;

	// Pre-computed "final" positions for every target location.
	// Screws/dowels spread across 4 corners + extras with repetition index baked in.
	function targetPos(location: string, repIndex: number, step: number): Vector3 {
		switch (location) {
			// Frames snap to floor level at each end; rep1→left, rep2→right
			case 'side_of_table_top':
			case 'other_side_of_table_top':
				return repIndex % 2 === 1
					? new Vector3(FRAME_X_L, 0, 0)
					: new Vector3(FRAME_X_R, 0, 0);
			case 'frame_hole': {
				// 6 dowel holes: 3 on each frame, spread along Z
				const zOffsets  = [-0.35, 0, 0.35];
				const frameX    = repIndex <= 3 ? FRAME_X_L : FRAME_X_R;
				const zOff      = zOffsets[(repIndex - 1) % 3];
				// root.y = SEAT_Y - dowel_height so dowel top is flush with seat surface
				return new Vector3(frameX, SEAT_Y - 0.18, zOff);
			}
			case 'screw_hole': {
				// 4 corner bolts (step4), 2 centre bolts (step5), 4 stretcher (step6)
				if (step === 4) {
					const corners = [
						new Vector3(FRAME_X_L, 0.32, -0.45),
						new Vector3(FRAME_X_L, 0.32,  0.45),
						new Vector3(FRAME_X_R, 0.32, -0.45),
						new Vector3(FRAME_X_R, 0.32,  0.45),
					];
					return corners[(repIndex - 1) % 4];
				}
				if (step === 5) {
					const mid = [
						new Vector3(FRAME_X_L, 0.18, 0),
						new Vector3(FRAME_X_R, 0.18, 0),
					];
					return mid[(repIndex - 1) % 2];
				}
				// step 6
				const outer = [
					new Vector3(FRAME_X_L, 0.08, -0.38),
					new Vector3(FRAME_X_L, 0.08,  0.38),
					new Vector3(FRAME_X_R, 0.08, -0.38),
					new Vector3(FRAME_X_R, 0.08,  0.38),
				];
				return outer[(repIndex - 1) % 4];
			}
			case 'screw_head': {
				// Allen key follows the screw placed in the same step/repetition
				return targetPos('screw_hole', repIndex, step).add(new Vector3(0, 0.08, 0));
			}
			default:
				return new Vector3(0, 0.5, 0); // generic safe fallback
		}
	}

	// Initial (pre-assembly) positions for each part kind
	function initialPos(kind: PartKind, index: number): Vector3 {
		switch (kind) {
			case 'dowel': {
				const col = index % 6, row = Math.floor(index / 6);
				return new Vector3(-3.0 + col * 0.28, 0.22, -1.8 + row * 0.4);
			}
			case 'side_frame':
				// Staged to the front sides, clearly in camera view
				return new Vector3(index === 0 ? -2.2 : 2.2, 0, 1.6);
			case 'panel':
			case 'leg': {
				const col = index % 3;
				return new Vector3(-2.0 + col * 0.5, 0.1, 1.8);
			}
			case 'screw_long':
			case 'screw_medium':
			case 'screw': {
				const col = index % 5, row = Math.floor(index / 5);
				return new Vector3(2.2 + col * 0.28, 0.22, -1.4 + row * 0.35);
			}
			case 'barrel_nut':
			case 'washer':
			case 'nut': {
				const col = index % 6;
				return new Vector3(2.2 + col * 0.22, 0.18, 0.4);
			}
			case 'allen_key':
				return new Vector3(3.2, 0.22, 0.8);
			default:
				return new Vector3(3.0, 0.22, index * 0.4);
		}
	}

	// ── Build 3D mesh for a given part kind ────────────────────────────────────
	function buildPartMesh(id: string, kind: PartKind, pos: Vector3): SceneObj {
		const root = new TransformNode(id, scene!);
		root.position = pos.clone();
		const meshes: Mesh[] = [];

		let mat: StandardMaterial;

		switch (kind) {
			case 'dowel': {
				mat = makeMat(`mat_${id}`, MAT_WOOD);
				const cyl = MeshBuilder.CreateCylinder(`${id}_cyl`, {
					height: 0.18, diameter: 0.045, tessellation: 10
				}, scene!);
				cyl.material = mat;
				cyl.parent = root;
				cyl.position.y = 0.09;
				shadowGen?.addShadowCaster(cyl);
				meshes.push(cyl);
				break;
			}
			case 'screw_long':
			case 'screw_medium':
			case 'screw': {
				mat = makeMat(`mat_${id}`, MAT_METAL);
				const heights = { screw_long: 0.22, screw_medium: 0.15, screw: 0.10 } as Record<string,number>;
				const diams   = { screw_long: 0.030, screw_medium: 0.026, screw: 0.020 } as Record<string,number>;
				const h = heights[kind] ?? 0.10, d = diams[kind] ?? 0.020;
				const shaft = MeshBuilder.CreateCylinder(`${id}_shaft`, { height: h, diameter: d, tessellation: 8 }, scene!);
				shaft.material = mat;
				shaft.parent = root;
				shaft.position.y = h / 2;
				const head = MeshBuilder.CreateCylinder(`${id}_head`, { height: d * 0.8, diameter: d * 2.2, tessellation: 8 }, scene!);
				head.material = mat;
				head.parent = root;
				head.position.y = h + 0.03;
				shadowGen?.addShadowCaster(shaft);
				shadowGen?.addShadowCaster(head);
				meshes.push(shaft, head);
				break;
			}
			case 'allen_key': {
				mat = makeMat(`mat_${id}`, MAT_TOOL);
				// L-shaped: long arm + short arm
				const longArm = MeshBuilder.CreateBox(`${id}_long`, { width: 0.22, height: 0.022, depth: 0.022 }, scene!);
				longArm.material = mat;
				longArm.parent = root;
				longArm.position = new Vector3(0.11, 0.011, 0);
				const shortArm = MeshBuilder.CreateBox(`${id}_short`, { width: 0.022, height: 0.10, depth: 0.022 }, scene!);
				shortArm.material = mat;
				shortArm.parent = root;
				shortArm.position = new Vector3(0, 0.05, 0);
				shadowGen?.addShadowCaster(longArm);
				shadowGen?.addShadowCaster(shortArm);
				meshes.push(longArm, shortArm);
				break;
			}
			case 'side_frame': {
				mat = makeMat(`mat_${id}`, MAT_WOOD);
				// Posts span the bench WIDTH (Z axis), so rotate root 90° around Y.
				// In local space: posts are at z = ±SPREAD; after rotation they align to world Z.
				root.rotation.y = Math.PI / 2;

				// Leg height matches seat height so legs reach exactly under the seat
				const PW = 0.085, PH = SEAT_Y, PD = 0.085;
				// Half-distance between the two legs (bench is 1.1 wide, legs are at ±0.50)
				const SPREAD = 0.50;
				// Note: after rotation.y = π/2, local X → world Z, local Z → world -X
				const postL = MeshBuilder.CreateBox(`${id}_pL`, { width: PW, height: PH, depth: PD }, scene!);
				postL.material = mat; postL.parent = root;
				postL.position = new Vector3(-SPREAD, PH / 2, 0);
				const postR = MeshBuilder.CreateBox(`${id}_pR`, { width: PW, height: PH, depth: PD }, scene!);
				postR.material = mat; postR.parent = root;
				postR.position = new Vector3(SPREAD, PH / 2, 0);
				// Bottom stretcher — connects the two legs near the floor
				const stretcher = MeshBuilder.CreateBox(`${id}_str`, { width: SPREAD * 2 + PW, height: PW, depth: PD }, scene!);
				stretcher.material = mat; stretcher.parent = root;
				stretcher.position = new Vector3(0, 0.10, 0);
				// Upper cross-rail just below where the seat sits
				const topRail = MeshBuilder.CreateBox(`${id}_top`, { width: SPREAD * 2 + PW, height: PW, depth: PD }, scene!);
				topRail.material = mat; topRail.parent = root;
				topRail.position = new Vector3(0, PH - PW * 0.5, 0);
				for (const m of [postL, postR, stretcher, topRail]) {
					shadowGen?.addShadowCaster(m);
					meshes.push(m);
				}
				break;
			}
			case 'panel': {
				mat = makeMat(`mat_${id}`, MAT_WOOD);
				const p = MeshBuilder.CreateBox(`${id}_panel`, { width: 1.2, height: 0.05, depth: 0.8 }, scene!);
				p.material = mat; p.parent = root;
				shadowGen?.addShadowCaster(p);
				meshes.push(p);
				break;
			}
			case 'leg': {
				mat = makeMat(`mat_${id}`, MAT_WOOD);
				const l = MeshBuilder.CreateCylinder(`${id}_leg`, { height: 0.7, diameter: 0.09, tessellation: 8 }, scene!);
				l.material = mat; l.parent = root; l.position.y = 0.35;
				shadowGen?.addShadowCaster(l);
				meshes.push(l);
				break;
			}
			case 'barrel_nut': {
				mat = makeMat(`mat_${id}`, MAT_METAL);
				const bn = MeshBuilder.CreateCylinder(`${id}_bn`, { height: 0.14, diameter: 0.14, tessellation: 12 }, scene!);
				bn.material = mat; bn.parent = root; bn.position.y = 0.07;
				bn.rotation.z = Math.PI / 2;
				shadowGen?.addShadowCaster(bn);
				meshes.push(bn);
				break;
			}
			case 'washer': {
				mat = makeMat(`mat_${id}`, MAT_METAL);
				const ws = MeshBuilder.CreateTorus(`${id}_ws`, { diameter: 0.12, thickness: 0.025, tessellation: 14 }, scene!);
				ws.material = mat; ws.parent = root; ws.position.y = 0.015;
				shadowGen?.addShadowCaster(ws);
				meshes.push(ws);
				break;
			}
			case 'nut': {
				mat = makeMat(`mat_${id}`, MAT_METAL);
				const nt = MeshBuilder.CreateCylinder(`${id}_nt`, { height: 0.06, diameter: 0.1, tessellation: 6 }, scene!);
				nt.material = mat; nt.parent = root; nt.position.y = 0.03;
				shadowGen?.addShadowCaster(nt);
				meshes.push(nt);
				break;
			}
			default: {
				mat = makeMat(`mat_${id}`, MAT_METAL);
				const b = MeshBuilder.CreateBox(`${id}_box`, { size: 0.1 }, scene!);
				b.material = mat; b.parent = root;
				shadowGen?.addShadowCaster(b);
				meshes.push(b);
			}
		}

		return { id, root, meshes, currentPos: pos.clone(), mat };
	}

	// ── Build the static bench seat panel & floor ──────────────────────────────
	function buildEnvironment() {
		// Floor
		const floor = MeshBuilder.CreateBox('floor', { width: 14, height: 0.06, depth: 8 }, scene!);
		floor.position.y = -0.03;
		const floorMat = makeMat('floorMat', MAT_FLOOR);
		floor.material = floorMat;
		floor.receiveShadows = true;

		// Seat slat panel — bottom face sits exactly on top of the side frames (y = SEAT_Y)
		const seatMat = makeMat('seatMat', MAT_WOOD);
		const SLAT_H    = 0.045;
		const seatNode  = new TransformNode('seat_panel', scene!);
		// slatNode bottom = SEAT_Y → centre = SEAT_Y + SLAT_H/2
		seatNode.position = new Vector3(0, SEAT_Y + SLAT_H / 2, 0);

		// Slats run along the LENGTH (X). 10 slats across the 3.0 unit span.
		const SLAT_COUNT = 10;
		const BENCH_LEN  = 3.0;
		const BENCH_W    = 1.10;
		const GAP        = 0.012;
		const SLAT_W     = (BENCH_LEN - GAP * (SLAT_COUNT + 1)) / SLAT_COUNT;
		for (let i = 0; i < SLAT_COUNT; i++) {
			const slat = MeshBuilder.CreateBox(`slat_${i}`, { width: SLAT_W, height: SLAT_H, depth: BENCH_W }, scene!);
			slat.material = seatMat;
			slat.parent = seatNode;
			slat.position.x = -BENCH_LEN / 2 + GAP + SLAT_W / 2 + i * (SLAT_W + GAP);
			shadowGen?.addShadowCaster(slat);
			slat.receiveShadows = true;
		}
		// Two longitudinal support rails under the slats (the internal frame of the seat panel)
		const railMat = makeMat('seatRailMat', '#9e6e3a');
		for (const zOff of [-0.38, 0.38]) {
			const rail = MeshBuilder.CreateBox(`seat_rail_${zOff}`, { width: BENCH_LEN, height: 0.08, depth: 0.07 }, scene!);
			rail.material = railMat;
			rail.parent = seatNode;
			rail.position = new Vector3(0, -SLAT_H / 2 - 0.04, zOff);
			shadowGen?.addShadowCaster(rail);
		}
	}

	// ── Build gripper ──────────────────────────────────────────────────────────
	function buildGripper() {
		gripperNode = new TransformNode('gripper', scene!);
		gripperNode.position = new Vector3(0, 1.8, 0);
		const gripMat = makeMat('gripMat', '#445566');
		gripMat.alpha = 0.7;
		gripperL = MeshBuilder.CreateBox('gripL', { width: 0.06, height: 0.18, depth: 0.06 }, scene!);
		gripperL.material = gripMat; gripperL.parent = gripperNode;
		gripperL.position.x = -0.1;
		gripperR = MeshBuilder.CreateBox('gripR', { width: 0.06, height: 0.18, depth: 0.06 }, scene!);
		gripperR.material = gripMat; gripperR.parent = gripperNode;
		gripperR.position.x = 0.1;
		const palm = MeshBuilder.CreateBox('gripPalm', { width: 0.28, height: 0.06, depth: 0.06 }, scene!);
		palm.material = gripMat; palm.parent = gripperNode;
		palm.position.y = 0.12;
	}

	// ── Initialise all part objects from the parsed commands ───────────────────
	function populateScene() {
		// Clear previous — dispose meshes AND materials to avoid stale state
		for (const obj of sceneObjects.values()) {
			obj.root.getChildMeshes().forEach(m => { m.material?.dispose(); m.dispose(); });
			obj.root.dispose();
			obj.mat.dispose();
		}
		sceneObjects.clear();

		// Count occurrences of each base kind for initial-position indexing
		const kindCount: Record<string, number> = {};
		const seenIds = new Set<string>();

		for (const c of program.commands) {
			if (seenIds.has(c.objectId)) continue;
			seenIds.add(c.objectId);

			const kind = inferKind(c.objectId);
			const idx = kindCount[kind] ?? 0;
			kindCount[kind] = idx + 1;

			const pos = initialPos(kind, idx);
			const obj = buildPartMesh(c.objectId, kind, pos);
			sceneObjects.set(c.objectId, obj);
		}
	}

	// ── Animation helpers ──────────────────────────────────────────────────────
	function lerp(a: Vector3, b: Vector3, t: number) {
		return new Vector3(a.x + (b.x - a.x) * t, a.y + (b.y - a.y) * t, a.z + (b.z - a.z) * t);
	}

	function ease(t: number) { return t < 0.5 ? 2 * t * t : 1 - Math.pow(-2 * t + 2, 2) / 2; }

	function sleep(ms: number) { return new Promise<void>(r => setTimeout(r, ms)); }

	async function animateMove(node: TransformNode, from: Vector3, to: Vector3, durationMs: number) {
		const steps = Math.max(1, Math.round(durationMs / 16));
		for (let i = 0; i <= steps; i++) {
			if (!isPlaying && i > 0) return;
			const t = ease(i / steps);
			node.position = lerp(from, to, t);
			await sleep(16);
		}
		node.position = to.clone();
	}

	async function animateRotate(node: TransformNode, turns: number, durationMs: number) {
		const steps = Math.max(1, Math.round(durationMs / 16));
		const startY = node.rotation.y;
		const endY   = startY + turns * Math.PI * 2;
		for (let i = 0; i <= steps; i++) {
			if (!isPlaying && i > 0) return;
			const t = ease(i / steps);
			node.rotation.y = startY + (endY - startY) * t;
			await sleep(16);
		}
	}

	function highlightObj(obj: SceneObj, active: boolean, done = false) {
		if (!obj) return;
		const kind = inferKind(obj.id);
		const metalKinds: PartKind[] = ['screw','screw_medium','screw_long','allen_key','washer','barrel_nut','nut'];
		const naturalHex = metalKinds.includes(kind) ? MAT_METAL : MAT_WOOD;
		if (active) {
			obj.mat.diffuseColor  = hexToColor3(MAT_ACTIVE);
			obj.mat.emissiveColor = hexToColor3(MAT_ACTIVE).scale(0.35);
		} else if (done) {
			obj.mat.diffuseColor  = hexToColor3(MAT_DONE);
			obj.mat.emissiveColor = hexToColor3(MAT_DONE).scale(0.25);
		} else {
			obj.mat.diffuseColor  = hexToColor3(naturalHex);
			obj.mat.emissiveColor = Color3.Black();
		}
	}

	// ── Execute one robot command ──────────────────────────────────────────────
	async function executeCommand(c: RobotCommand) {
		const obj = sceneObjects.get(c.objectId);
		if (!obj) return;

		// Update gripper world position (approximate) ─────────────────────────
		const moveGripper = async (targetWorld: Vector3) => {
			if (!gripperNode) return;
			const from = gripperNode.position.clone();
			// Rise first, then move, then descend
			const rise    = new Vector3(from.x, Math.max(from.y, targetWorld.y + 0.5), from.z);
			const over    = new Vector3(targetWorld.x, rise.y, targetWorld.z);
			const descent = targetWorld.clone();
			await animateMove(gripperNode, from, rise,    180);
			await animateMove(gripperNode, rise, over,    280);
			await animateMove(gripperNode, over, descent, 180);
		};

		switch (c.cmd) {
			case 'pick': {
				highlightObj(obj, true);
				await moveGripper(obj.currentPos.add(new Vector3(0, 0.12, 0)));
				// Close gripper
				if (gripperL) gripperL.position.x = -0.055;
				if (gripperR) gripperR.position.x =  0.055;
				heldId = c.objectId;
				break;
			}
			case 'move':
			case 'align': {
				if (!obj) break;
				const dest = (c.to !== 'none' && c.to)
					? targetPos(c.to, c.repIndex, c.step).add(new Vector3(0, 0.15, 0))
					: obj.currentPos.clone();
				await moveGripper(dest.add(new Vector3(0, 0.12, 0)));
				const from = obj.currentPos.clone();
				await animateMove(obj.root, from, dest, 600);
				obj.currentPos = dest.clone();
				break;
			}
			case 'insert':
			case 'place': {
				if (!obj) break;
				const dest = (c.to !== 'none' && c.to)
					? targetPos(c.to, c.repIndex, c.step)
					: obj.currentPos.clone();
				const from = obj.currentPos.clone();
				await animateMove(obj.root, from, dest, 500);
				obj.currentPos = dest.clone();
				highlightObj(obj, false, true);
				// Snap gripper up
				if (gripperNode) {
					const up = gripperNode.position.add(new Vector3(0, 0.4, 0));
					await animateMove(gripperNode, gripperNode.position.clone(), up, 250);
				}
				break;
			}
			case 'rotate': {
				if (!obj) break;
				await animateRotate(obj.root, 3, 700);
				break;
			}
			case 'release': {
				if (!obj) break;
				if (gripperL) gripperL.position.x = -0.1;
				if (gripperR) gripperR.position.x =  0.1;
				heldId = null;
				// Tools (allen_key) return to the table rest area; parts stay done
				const kind = inferKind(obj.id);
				if (kind === 'allen_key') {
					const toolRest = initialPos('allen_key', 0);
					await animateMove(obj.root, obj.currentPos.clone(), toolRest, 500);
					obj.currentPos = toolRest.clone();
					highlightObj(obj, false, false);
				} else {
					highlightObj(obj, false, true);
				}
				// Retract gripper upward
				if (gripperNode) {
					const retract = gripperNode.position.add(new Vector3(0, 0.8, 0));
					await animateMove(gripperNode, gripperNode.position.clone(), retract, 350);
				}
				break;
			}
		}
	}

	// ── Playback control ───────────────────────────────────────────────────────
	async function playFrom(startIdx: number) {
		isPlaying = true;
		for (let i = startIdx; i < totalCmds && isPlaying; i++) {
			cmdIndex = i;
			const c = flatCmds[i];
			statusText = `Step ${c.step} · ${c.description}`;
			await executeCommand(c);
			await sleep(120);
		}
		if (isPlaying) {
			isPlaying = false;
			statusText = 'Assembly complete';
		}
	}

	function handlePlay() {
		if (isPlaying) { isPlaying = false; statusText = 'Paused'; return; }
		const start = cmdIndex < totalCmds - 1 ? cmdIndex + 1 : 0;
		if (start === 0) resetScene();
		playFrom(start);
	}

	async function handleStep(dir: number) {
		if (isPlaying) return;
		const next = cmdIndex + dir;
		if (next < 0 || next >= totalCmds) return;
		if (dir < 0) {
			// Step back: reset and replay up to next
			resetScene();
			isPlaying = true;
			for (let i = 0; i <= next && isPlaying; i++) {
				cmdIndex = i;
				await executeCommand(flatCmds[i]);
			}
			isPlaying = false;
		} else {
			cmdIndex = next;
			const c = flatCmds[next];
			statusText = `Step ${c.step} · ${c.description}`;
			isPlaying = true;
			await executeCommand(c);
			isPlaying = false;
		}
	}

	function handleReset() {
		isPlaying = false;
		resetScene();
	}

	function resetScene() {
		cmdIndex = -1;
		statusText = 'Ready — press Play or step through';
		heldId = null;
		if (gripperL) gripperL.position.x = -0.1;
		if (gripperR) gripperR.position.x =  0.1;
		if (gripperNode) gripperNode.position = new Vector3(0, 1.8, 0);
		populateScene();
	}

	function handleJumpToStep(stepNum: number) {
		if (isPlaying) return;
		const idx = flatCmds.findIndex(c => c.step === stepNum && c.repIndex === 1 && c.order === 1);
		if (idx < 0) return;
		resetScene();
		isPlaying = true;
		(async () => {
			for (let i = 0; i < idx && isPlaying; i++) {
				cmdIndex = i;
				await executeCommand(flatCmds[i]);
			}
			isPlaying = false;
			cmdIndex = idx - 1;
			statusText = `Ready to start Step ${stepNum}`;
		})();
	}

	// ── Apply edits to ASP input ───────────────────────────────────────────────
	function applyAsp() {
		try {
			program = parseProgram(aspText);
			parseError = '';
			if (scene) populateScene();
			handleReset();
		} catch (e: any) {
			parseError = e.message ?? 'Parse error';
		}
	}

	// ── Babylon setup ──────────────────────────────────────────────────────────
	onMount(() => {
		engine = new Engine(canvas, true, { adaptToDeviceRatio: true });
		scene  = new Scene(engine);
		scene.clearColor = new Color4(0.12, 0.12, 0.16, 1);

		// Camera
		const camera = new ArcRotateCamera('cam', -Math.PI / 4, Math.PI / 3.5, 9, new Vector3(0, 0.5, 0), scene);
		camera.attachControl(canvas, true);
		camera.lowerRadiusLimit = 3;
		camera.upperRadiusLimit = 18;
		camera.wheelPrecision = 40;

		// Lights
		const ambient = new HemisphericLight('amb', new Vector3(0, 1, 0), scene);
		ambient.intensity = 0.55;
		ambient.groundColor = new Color3(0.2, 0.18, 0.15);
		const sun = new DirectionalLight('sun', new Vector3(-1, -2, -1), scene);
		sun.intensity = 1.1;
		sun.position = new Vector3(5, 8, 5);
		shadowGen = new ShadowGenerator(1024, sun);
		shadowGen.useBlurExponentialShadowMap = true;
		shadowGen.blurKernel = 16;

		buildEnvironment();
		buildGripper();
		populateScene();

		engine.runRenderLoop(() => scene?.render());
		window.addEventListener('resize', () => engine?.resize());
	});

	onDestroy(() => {
		engine?.dispose();
	});

	// ── Sidebar helpers ────────────────────────────────────────────────────────
	function currentStep(): number {
		if (cmdIndex < 0) return -1;
		return flatCmds[cmdIndex]?.step ?? -1;
	}

	function currentRepIndex(): number {
		return flatCmds[cmdIndex]?.repIndex ?? 0;
	}

	const CMD_ICONS: Record<Cmd, string> = {
		pick:    '🤏',
		move:    '→',
		align:   '⊕',
		insert:  '↓',
		place:   '↓',
		rotate:  '↺',
		release: '✓',
	};
</script>

<!-- ── Layout ─────────────────────────────────────────────────────────────── -->
<div class="app">

	<!-- Left panel: step list -->
	<aside class="sidebar">
		<h2 class="sidebar-title">Assembly Steps</h2>
		<div class="step-list">
			{#each program.steps as step}
				{@const repCount = program.repetitions[step] ?? 1}
				{@const isCurrent = currentStep() === step}
				{@const isDone = currentStep() > step || (currentStep() === step && cmdIndex >= 0 && flatCmds[cmdIndex]?.order === flatCmds.filter(c => c.step === step).at(-1)?.order && flatCmds[cmdIndex]?.repIndex === repCount)}
				<button
					class="step-btn"
					class:active={isCurrent}
					class:done={isDone && !isCurrent}
					onclick={() => handleJumpToStep(step)}
				>
					<span class="step-num">Step {step}</span>
					{#if repCount > 1}
						<span class="step-rep">×{repCount}</span>
					{/if}
					<span class="step-indicator">{isDone ? '✓' : isCurrent ? '▶' : '○'}</span>
				</button>
			{/each}
		</div>

		<!-- Command detail -->
		{#if cmdIndex >= 0 && cmdIndex < totalCmds}
			{@const c = flatCmds[cmdIndex]}
			<div class="cmd-detail">
				<div class="cmd-step-label">Step {c.step}{c.repCount > 1 ? ` (rep ${c.repIndex}/${c.repCount})` : ''}</div>
				<div class="cmd-action">
					<span class="cmd-icon">{CMD_ICONS[c.cmd]}</span>
					<span class="cmd-name">{c.cmd.toUpperCase()}</span>
				</div>
				<div class="cmd-object">{c.objectId.replace(/_\d+$/, '').replace(/_/g, ' ')}</div>
				{#if c.from !== 'none'}<div class="cmd-loc">From: <em>{c.from}</em></div>{/if}
				{#if c.to !== 'none'}<div class="cmd-loc">To: <em>{c.to}</em></div>{/if}
				<div class="cmd-desc">{c.description}</div>
			</div>
		{/if}
	</aside>

	<!-- 3D viewport -->
	<main class="viewport">
		<canvas bind:this={canvas} class="canvas3d"></canvas>

		<!-- Status bar -->
		<div class="status-bar">
			<span class="status-text">{statusText}</span>
			<span class="status-prog">{cmdIndex + 1} / {totalCmds}</span>
		</div>

		<!-- Playback controls -->
		<div class="controls">
			<button class="ctrl-btn" onclick={handleReset} title="Reset" disabled={isPlaying}>⏮</button>
			<button class="ctrl-btn" onclick={() => handleStep(-1)} title="Back" disabled={isPlaying}>◀</button>
			<button class="ctrl-btn play-btn" onclick={handlePlay}>
				{isPlaying ? '⏸' : '▶'}
			</button>
			<button class="ctrl-btn" onclick={() => handleStep(1)} title="Next" disabled={isPlaying}>▶</button>
		</div>
	</main>

	<!-- Right panel: ASP editor -->
	<aside class="editor-panel">
		<h2 class="sidebar-title">ASP Input</h2>
		<textarea class="asp-editor" bind:value={aspText} spellcheck="false"></textarea>
		{#if parseError}<div class="parse-error">{parseError}</div>{/if}
		<button class="apply-btn" onclick={applyAsp}>Apply</button>
	</aside>
</div>

<!-- ── Styles ─────────────────────────────────────────────────────────────── -->
<style>
	:global(body, html) {
		margin: 0; padding: 0;
		background: #0e0e12;
		color: #e8e4dc;
		font-family: 'Inter', system-ui, sans-serif;
		height: 100%;
	}

	.app {
		display: grid;
		grid-template-columns: 200px 1fr 260px;
		height: 100vh;
		overflow: hidden;
		gap: 0;
	}

	/* ── Sidebar ─────────────────────────────────────────────────── */
	.sidebar {
		background: #16161e;
		border-right: 1px solid #2a2a3a;
		display: flex;
		flex-direction: column;
		overflow: hidden;
		padding: 0;
	}

	.sidebar-title {
		font-size: 0.72rem;
		font-weight: 600;
		letter-spacing: 0.1em;
		text-transform: uppercase;
		color: #7070a0;
		padding: 14px 14px 8px;
		margin: 0;
		border-bottom: 1px solid #222230;
	}

	.step-list {
		flex: 0 0 auto;
		padding: 8px 8px 0;
		display: flex;
		flex-direction: column;
		gap: 4px;
	}

	.step-btn {
		display: flex;
		align-items: center;
		gap: 6px;
		background: #1e1e2a;
		border: 1px solid #2a2a3c;
		border-radius: 6px;
		color: #a0a0c0;
		cursor: pointer;
		font-size: 0.8rem;
		padding: 7px 10px;
		text-align: left;
		transition: all 0.15s;
		width: 100%;
	}

	.step-btn:hover { background: #252535; border-color: #4040608a; color: #d0d0f0; }
	.step-btn.active { background: #1a2a4a; border-color: #4466cc; color: #88aaff; }
	.step-btn.done   { background: #1a2a1e; border-color: #336644; color: #66cc88; }

	.step-num { flex: 1; font-weight: 600; }
	.step-rep { font-size: 0.7rem; color: #606080; }
	.step-indicator { margin-left: auto; font-size: 0.75rem; }

	.cmd-detail {
		margin: 12px 8px 8px;
		padding: 10px 12px;
		background: #1a1a26;
		border-radius: 8px;
		border: 1px solid #2a2a40;
		font-size: 0.78rem;
		line-height: 1.5;
	}

	.cmd-step-label { color: #6070a0; font-size: 0.7rem; margin-bottom: 4px; }
	.cmd-action     { display: flex; align-items: center; gap: 6px; margin-bottom: 4px; }
	.cmd-icon       { font-size: 1rem; }
	.cmd-name       { font-weight: 700; color: #88aaff; letter-spacing: 0.05em; }
	.cmd-object     { color: #c8c0a8; font-weight: 500; margin-bottom: 2px; }
	.cmd-loc        { color: #808088; font-size: 0.72rem; }
	.cmd-loc em     { color: #a09858; font-style: normal; }
	.cmd-desc       { color: #9898b0; margin-top: 6px; font-size: 0.73rem; line-height: 1.4; }

	/* ── Viewport ─────────────────────────────────────────────────── */
	.viewport {
		position: relative;
		background: #0e0e12;
		display: flex;
		flex-direction: column;
	}

	.canvas3d {
		flex: 1;
		width: 100%;
		height: 100%;
		display: block;
		outline: none;
	}

	.status-bar {
		position: absolute;
		top: 10px; left: 50%;
		transform: translateX(-50%);
		background: rgba(10, 10, 20, 0.82);
		border: 1px solid #2a2a44;
		border-radius: 20px;
		padding: 5px 16px;
		display: flex;
		gap: 16px;
		align-items: center;
		font-size: 0.78rem;
		backdrop-filter: blur(6px);
		pointer-events: none;
		max-width: 80%;
	}

	.status-text { color: #c8c0d0; flex: 1; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
	.status-prog { color: #606080; font-variant-numeric: tabular-nums; white-space: nowrap; }

	.controls {
		position: absolute;
		bottom: 18px; left: 50%;
		transform: translateX(-50%);
		display: flex;
		gap: 8px;
		background: rgba(12, 12, 22, 0.88);
		border: 1px solid #2a2a44;
		border-radius: 32px;
		padding: 8px 16px;
		backdrop-filter: blur(8px);
	}

	.ctrl-btn {
		background: #1e1e30;
		border: 1px solid #333350;
		border-radius: 50%;
		color: #c0c0e0;
		cursor: pointer;
		font-size: 0.95rem;
		height: 38px; width: 38px;
		transition: all 0.12s;
		display: flex; align-items: center; justify-content: center;
	}

	.ctrl-btn:hover:not(:disabled) { background: #282840; border-color: #5555aa; color: #e0e0ff; }
	.ctrl-btn:disabled { opacity: 0.35; cursor: not-allowed; }
	.play-btn { width: 52px; height: 52px; font-size: 1.2rem; background: #2030608a; border-color: #4466cc; }
	.play-btn:hover { background: #2a3a78 !important; }

	/* ── Editor panel ─────────────────────────────────────────────── */
	.editor-panel {
		background: #14141c;
		border-left: 1px solid #2a2a3a;
		display: flex;
		flex-direction: column;
		overflow: hidden;
	}

	.asp-editor {
		flex: 1;
		background: #0e0e16;
		border: none;
		color: #a8d8a8;
		font-family: 'JetBrains Mono', 'Fira Code', monospace;
		font-size: 0.66rem;
		line-height: 1.5;
		outline: none;
		overflow-y: auto;
		padding: 10px 12px;
		resize: none;
		tab-size: 2;
	}

	.parse-error {
		background: #3a1515;
		border-top: 1px solid #aa3333;
		color: #ff8888;
		font-size: 0.7rem;
		padding: 6px 12px;
	}

	.apply-btn {
		background: #1e3050;
		border: none;
		border-top: 1px solid #2a3a60;
		color: #88aaff;
		cursor: pointer;
		font-size: 0.8rem;
		font-weight: 600;
		letter-spacing: 0.06em;
		padding: 10px;
		text-transform: uppercase;
		transition: background 0.12s;
		width: 100%;
	}

	.apply-btn:hover { background: #253860; }
</style>
