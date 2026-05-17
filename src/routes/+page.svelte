<script lang="ts">
  import { asset } from '$app/paths';

  const logoUrl = asset('/aspembly-logo-removebg-preview.png');

  const heroPlaceholderUrl = asset('/assembly_manual_example.png');
  const inputPlaceholderUrl = asset('/table_final.png');
  const interpretationPlaceholderUrl = asset('/table_simulated.png');

  const aspOutput = `% ÄPPLARÖ Bench assembly

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

  const pipeline = [
    {
      step: '01',
      title: 'Visual Manual Input',
      text: 'ASPembly starts from image-based assembly manuals: pages, diagrams, arrows, part identifiers and implicit manipulation cues.',
      icon: '◩'
    },
    {
      step: '02',
      title: 'VLM Extraction Oracle',
      text: 'A Vision-Language Model reads the page and proposes candidate actions, objects, source locations, targets and descriptions.',
      icon: '◉'
    },
    {
      step: '03',
      title: 'vLGX Control Layer',
      text: 'The extraction is not one-shot. Logic guards decide which predicates are admissible, skipped, delayed or inferred.',
      icon: '◆'
    },
    {
      step: '04',
      title: 'ASP Symbolic Trace',
      text: 'The final output is a compact symbolic command trace suitable for inspection, validation and downstream robotic reasoning.',
      icon: '▣'
    }
  ];

  const features = [
    'Vision-to-symbolic assembly interpretation',
    'ASP-guided extraction instead of passive post-checking',
    'Guard-based control over the evolving symbolic state',
    'Compact robot_command/7 action representation',
    'Readable ASP facts for inspection and debugging',
    'Future-ready path toward robotic execution'
  ];

  const actions = ['pick', 'move', 'align', 'insert', 'rotate', 'release'];

  const assets = [
    {
      name: 'seat slat panel',
      kind: 'seat-panel',
      detail: '10 slats + 2 rails'
    },
    {
      name: 'side frame',
      kind: 'side-frame',
      detail: '2 posts + 2 rails'
    },
    {
      name: 'wooden dowel',
      kind: 'dowel',
      detail: 'wood cylinder'
    },
    {
      name: 'long hex bolt',
      kind: 'screw-long',
      detail: 'h 0.22 · d 0.030'
    },
    {
      name: 'medium screw',
      kind: 'screw-medium',
      detail: 'h 0.15 · d 0.026'
    },
    {
      name: 'wood screw',
      kind: 'screw-short',
      detail: 'h 0.10 · d 0.020'
    },
    {
      name: 'allen key',
      kind: 'allen-key',
      detail: 'L-shaped tool'
    },
    {
      name: 'robot gripper',
      kind: 'gripper',
      detail: 'palm + 2 fingers'
    }
  ];
</script>

<svelte:head>
  <title>ASPembly | Neuro-Symbolic Assembly Pipeline</title>
  <meta
    name="description"
    content="ASPembly transforms visual assembly manuals into symbolic ASP traces for robotic assembly reasoning."
  />
</svelte:head>

<main class="page">
  <section class="hero">
    <div class="hero-bg"></div>

    <div class="asset-cloud asset-cloud-one">
      <span class="asset-piece bolt"></span>
      <span class="asset-piece board-piece"></span>
      <span class="asset-piece dowel"></span>
      <span class="asset-piece screw-piece"></span>
    </div>

    <div class="asset-cloud asset-cloud-two">
      <span class="asset-piece board-piece small"></span>
      <span class="asset-piece screw-piece"></span>
      <span class="asset-piece dowel small"></span>
    </div>

    <div class="hero-content">
      <div class="eyebrow">
        <span class="dot"></span>
        Neuro-Symbolic Robotic Assembly
      </div>

      <img class="hero-logo" src={logoUrl} alt="ASPembly logo" />

      <h1>
        From visual manuals to
        <span>symbolic assembly traces.</span>
      </h1>

      <p class="hero-subtitle">
        ASPembly is a visual neuro-symbolic pipeline that turns image-based
        assembly instructions into structured ASP facts, combining Vision-Language
        Models with Answer Set Programming as an active reasoning and control layer.
      </p>

      <div class="hero-actions">
        <a href="#pipeline" class="btn primary">Explore pipeline</a>
        <a href="#demo" class="btn secondary">View symbolic output</a>
      </div>

      <div class="hero-metrics">
        <div>
          <strong>VLM</strong>
          <span>visual extraction</span>
        </div>
        <div>
          <strong>ASP</strong>
          <span>logic control</span>
        </div>
        <div>
          <strong>vLGX</strong>
          <span>guided pipeline</span>
        </div>
      </div>
    </div>

    <div class="hero-card">
      <div class="window-bar">
        <span></span>
        <span></span>
        <span></span>
      </div>

      <div class="manual-preview">
        <img class="manual-preview-image" src={heroPlaceholderUrl} alt="Manual page placeholder" />
      </div>

      <div class="trace-mini">
        <span>robot_command</span>
        <span>pick → move → align → insert → rotate → release</span>
      </div>
    </div>
  </section>

  <section id="pipeline" class="section">
    <div class="section-heading">
      <p>Pipeline</p>
      <h2>Not just extraction. Controlled symbolic construction.</h2>
      <span>
        ASPembly does not ask a model to hallucinate a final plan. It builds the
        symbolic trace incrementally, while ASP controls what is admissible at each step.
      </span>
    </div>

    <div class="pipeline-grid">
      {#each pipeline as item}
        <article class="pipeline-card">
          <div class="pipeline-top">
            <span class="pipeline-icon">{item.icon}</span>
            <span class="pipeline-step">{item.step}</span>
          </div>
          <h3>{item.title}</h3>
          <p>{item.text}</p>
        </article>
      {/each}
    </div>
  </section>

  <section class="split-section">
    <div class="logic-panel">
      <p class="kicker">Why ASPembly matters</p>
      <h2>ASP becomes an active process-control layer.</h2>
      <p>
        Instead of validating the output only after generation, ASPembly uses logic
        during extraction. Guard conditions over the current symbolic state determine
        whether the visual oracle should be invoked, skipped or constrained.
      </p>

      <div class="feature-list">
        {#each features as feature}
          <div class="feature-item">
            <span>✓</span>
            <p>{feature}</p>
          </div>
        {/each}
      </div>
    </div>

    <div class="control-card">
      <div class="control-orbit">
        <div class="orbit-center">ASP</div>
        <span class="orbit-node n1">validate</span>
        <span class="orbit-node n2">derive</span>
        <span class="orbit-node n3">control</span>
        <span class="orbit-node n4">constrain</span>
      </div>

      <div>
        <p class="kicker">Active symbolic control</p>
        <h3>Reasoning is part of the extraction loop.</h3>
        <p>
          The VLM proposes visual candidates, but ASP regulates the construction of
          the trace. This keeps the symbolic state coherent before it becomes the
          basis for inspection, visualization or robotic reasoning.
        </p>
      </div>
    </div>
  </section>

  <section class="vocabulary-section">
    <div class="section-heading">
      <p>Closed Vocabulary</p>
      <h2>Finite actions. Finite assets. Controlled interpretation.</h2>
      <span>
        ASPembly can operate over a constrained assembly vocabulary: a finite set of
        symbolic actions and a finite library of physical assets used to interpret,
        validate and visualize the extracted trace.
      </span>
    </div>

    <div class="vocabulary-grid">
      <article class="vocab-card">
        <div class="vocab-header">
          <span>Action vocabulary</span>
          <strong>{actions.length} primitives</strong>
        </div>

        <div class="chip-grid">
          {#each actions as action}
            <span class="chip action-chip">{action}</span>
          {/each}
        </div>
      </article>

      <article class="vocab-card assets-card">
        <div class="vocab-header">
          <span>Physical asset library</span>
          <strong>{assets.length} assets</strong>
        </div>

        <div class="asset-board">
          {#each assets as item}
            <div class="asset-token">
              <div class={`physical-asset ${item.kind}`} aria-hidden="true">
                {#if item.kind === 'seat-panel'}
                  {#each Array(10) as _, i}
                    <span class="seat-slat" style={`--i:${i}`}></span>
                  {/each}
                  <span class="seat-rail rail-a"></span>
                  <span class="seat-rail rail-b"></span>
                {:else if item.kind === 'side-frame'}
                  <span class="frame-post post-a"></span>
                  <span class="frame-post post-b"></span>
                  <span class="frame-rail frame-top"></span>
                  <span class="frame-rail frame-bottom"></span>
                {:else if item.kind === 'dowel'}
                  <span class="dowel-body"></span>
                {:else if item.kind === 'screw-long'}
                  <span class="screw-head long-head"></span>
                  <span class="screw-shaft long-shaft"></span>
                {:else if item.kind === 'screw-medium'}
                  <span class="screw-head medium-head"></span>
                  <span class="screw-shaft medium-shaft"></span>
                {:else if item.kind === 'screw-short'}
                  <span class="screw-head short-head"></span>
                  <span class="screw-shaft short-shaft"></span>
                {:else if item.kind === 'allen-key'}
                  <span class="allen-long"></span>
                  <span class="allen-short"></span>
                {:else if item.kind === 'gripper'}
                  <span class="gripper-palm"></span>
                  <span class="gripper-finger finger-left"></span>
                  <span class="gripper-finger finger-right"></span>
                {/if}
              </div>

              <div>
                <p>{item.name}</p>
                <small>{item.detail}</small>
              </div>
            </div>
          {/each}
        </div>
      </article>
    </div>
  </section>

  <section id="demo" class="demo-section">
    <div class="section-heading">
      <p>Demonstration</p>
      <h2>One visual instruction, three synchronized views.</h2>
      <span>
        The landing page can later replace these placeholders with real screenshots:
        manual page, symbolic ASP output and 3D visual interpretation.
      </span>
    </div>

    <div class="demo-grid">
      <article class="demo-card visual">
        <div class="demo-card-header">
          <span>Input</span>
          <strong>Manual page</strong>
        </div>

        <div class="placeholder-manual">
          <img class="placeholder-image" src={inputPlaceholderUrl} alt="Manual page" />
        </div>
      </article>

      <article class="demo-card code">
        <div class="demo-card-header">
          <span>Output</span>
          <strong>ASP trace</strong>
        </div>

        <pre>{aspOutput}</pre>
      </article>

      <article class="demo-card render">
        <div class="demo-card-header">
          <span>Interpretation</span>
          <strong>3D view placeholder</strong>
        </div>

        <div class="render-box">
          <img class="placeholder-image" src={interpretationPlaceholderUrl} alt="3D view placeholder" />
        </div>
      </article>
    </div>
  </section>

  <section class="architecture-section">
    <div class="arch-card">
      <h2>ASPembly architecture</h2>

      <div class="arch-flow">
        <div>Visual Page</div>
        <span>→</span>
        <div>VLM Candidates</div>
        <span>→</span>
        <div>vLGX Guards</div>
        <span>→</span>
        <div>ASP Facts</div>
        <span>→</span>
        <div>Symbolic Trace</div>
      </div>

      <p>
        The key idea is separation of responsibilities: the VLM observes and proposes,
        while ASP validates, derives, constrains and controls.
      </p>
    </div>
  </section>
</main>

<style>
  :global(body) {
    margin: 0;
    background: #050816;
    color: #f8fafc;
    font-family:
      Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI",
      sans-serif;
  }

  :global(*) {
    box-sizing: border-box;
  }

  .page {
    min-height: 100vh;
    overflow-x: hidden;
    background:
      radial-gradient(circle at top left, rgba(68, 56, 202, 0.35), transparent 34rem),
      radial-gradient(circle at 80% 10%, rgba(14, 165, 233, 0.18), transparent 28rem),
      linear-gradient(180deg, #050816 0%, #0f172a 45%, #020617 100%);
  }

  .hero {
    position: relative;
    display: grid;
    grid-template-columns: minmax(0, 1.05fr) minmax(320px, 0.95fr);
    gap: 4rem;
    align-items: center;
    max-width: 1220px;
    margin: 0 auto;
    padding: 6rem 1.5rem 5rem;
  }

  .hero-bg {
    position: absolute;
    inset: 5rem 1rem auto auto;
    width: 34rem;
    height: 34rem;
    background: linear-gradient(135deg, rgba(59, 130, 246, 0.24), rgba(168, 85, 247, 0.14));
    filter: blur(70px);
    border-radius: 999px;
    pointer-events: none;
  }

  .asset-cloud {
    position: absolute;
    pointer-events: none;
    opacity: 0.35;
    filter: blur(0.1px);
  }

  .asset-cloud-one {
    top: 8%;
    right: 3%;
    width: 260px;
    height: 220px;
  }

  .asset-cloud-two {
    left: -3%;
    bottom: 4%;
    width: 250px;
    height: 220px;
    opacity: 0.22;
  }

  .asset-piece {
    position: absolute;
    display: block;
    border: 1px solid rgba(148, 163, 184, 0.24);
    box-shadow: 0 24px 60px rgba(0, 0, 0, 0.28);
  }

  .bolt {
    width: 34px;
    height: 34px;
    border-radius: 999px;
    top: 12%;
    left: 18%;
    background:
      radial-gradient(circle, #cbd5e1 0 35%, #64748b 36% 100%);
  }

  .board-piece {
    width: 120px;
    height: 44px;
    border-radius: 0.8rem;
    top: 40%;
    left: 28%;
    background: linear-gradient(135deg, #d6b083, #8b5e34);
    transform: rotate(-18deg);
  }

  .board-piece.small {
    width: 95px;
    height: 34px;
    top: 30%;
    left: 12%;
    transform: rotate(22deg);
  }

  .dowel {
    width: 22px;
    height: 110px;
    border-radius: 999px;
    top: 8%;
    right: 25%;
    background: linear-gradient(90deg, #b45309, #fde68a, #92400e);
    transform: rotate(28deg);
  }

  .dowel.small {
    height: 82px;
    top: 45%;
    left: 55%;
    transform: rotate(-34deg);
  }

  .screw-piece {
    width: 16px;
    height: 96px;
    border-radius: 999px;
    right: 10%;
    bottom: 6%;
    background: linear-gradient(90deg, #475569, #e2e8f0, #334155);
    transform: rotate(-38deg);
  }

  .hero-content,
  .hero-card {
    position: relative;
    z-index: 1;
  }

  .eyebrow {
    display: inline-flex;
    align-items: center;
    gap: 0.65rem;
    padding: 0.55rem 0.85rem;
    border: 1px solid rgba(148, 163, 184, 0.22);
    border-radius: 999px;
    color: #c7d2fe;
    background: rgba(15, 23, 42, 0.72);
    backdrop-filter: blur(18px);
    font-size: 0.9rem;
    margin-bottom: 1.4rem;
  }

  .hero-logo {
    display: block;
    width: min(100%, 620px);
    height: auto;
    margin: 0 0 1.6rem;
    object-fit: contain;
    filter: drop-shadow(0 24px 55px rgba(14, 165, 233, 0.24));
  }

  .dot {
    width: 0.55rem;
    height: 0.55rem;
    border-radius: 999px;
    background: #22c55e;
    box-shadow: 0 0 24px #22c55e;
  }

  h1 {
    margin: 0;
    max-width: 850px;
    font-size: clamp(3rem, 8vw, 6.7rem);
    line-height: 0.92;
    letter-spacing: -0.075em;
  }

  h1 span {
    display: block;
    background: linear-gradient(90deg, #60a5fa, #a78bfa, #22d3ee);
    -webkit-background-clip: text;
    background-clip: text;
    color: transparent;
  }

  .hero-subtitle {
    max-width: 680px;
    margin: 1.6rem 0 0;
    color: #cbd5e1;
    font-size: clamp(1.05rem, 2vw, 1.25rem);
    line-height: 1.75;
  }

  .hero-actions {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
    margin-top: 2rem;
  }

  .btn {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    min-height: 3.15rem;
    padding: 0 1.35rem;
    border-radius: 1rem;
    text-decoration: none;
    font-weight: 750;
    transition:
      transform 0.2s ease,
      border-color 0.2s ease,
      background 0.2s ease;
  }

  .btn:hover {
    transform: translateY(-2px);
  }

  .primary {
    color: #020617;
    background: linear-gradient(135deg, #67e8f9, #a78bfa);
  }

  .secondary {
    color: #e2e8f0;
    border: 1px solid rgba(148, 163, 184, 0.25);
    background: rgba(15, 23, 42, 0.72);
  }

  .hero-metrics {
    display: grid;
    grid-template-columns: repeat(3, minmax(0, 1fr));
    gap: 1rem;
    margin-top: 2.2rem;
    max-width: 620px;
  }

  .hero-metrics div {
    padding: 1rem;
    border: 1px solid rgba(148, 163, 184, 0.18);
    border-radius: 1.2rem;
    background: rgba(15, 23, 42, 0.6);
  }

  .hero-metrics strong {
    display: block;
    font-size: 1.25rem;
  }

  .hero-metrics span {
    display: block;
    margin-top: 0.3rem;
    color: #94a3b8;
    font-size: 0.9rem;
  }

  .hero-card {
    min-height: 560px;
    border: 1px solid rgba(148, 163, 184, 0.22);
    border-radius: 2rem;
    background:
      linear-gradient(180deg, rgba(15, 23, 42, 0.86), rgba(15, 23, 42, 0.58)),
      radial-gradient(circle at 50% 20%, rgba(59, 130, 246, 0.3), transparent 18rem);
    box-shadow: 0 30px 100px rgba(0, 0, 0, 0.45);
    backdrop-filter: blur(20px);
    padding: 1rem;
  }

  .window-bar {
    height: 2.7rem;
    display: flex;
    align-items: center;
    gap: 0.45rem;
    padding: 0 0.6rem;
  }

  .window-bar span {
    width: 0.75rem;
    height: 0.75rem;
    border-radius: 999px;
    background: rgba(148, 163, 184, 0.55);
  }

  .manual-preview {
    height: 410px;
    border-radius: 1.5rem;
    background: linear-gradient(135deg, rgba(248, 250, 252, 0.95), rgba(226, 232, 240, 0.85));
    padding: 1.2rem;
    color: #0f172a;
    overflow: hidden;
  }

  .manual-preview-image {
    display: block;
    width: 100%;
    height: 100%;
    border-radius: 1rem;
    object-fit: contain;
    background: #f8fafc;
  }

  .trace-mini {
    display: grid;
    gap: 0.4rem;
    margin-top: 1rem;
    padding: 1rem;
    border-radius: 1.2rem;
    background: rgba(2, 6, 23, 0.72);
    border: 1px solid rgba(148, 163, 184, 0.18);
    font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace;
  }

  .trace-mini span:first-child {
    color: #67e8f9;
    font-size: 0.9rem;
  }

  .trace-mini span:last-child {
    color: #e2e8f0;
  }

  .section,
  .split-section,
  .vocabulary-section,
  .demo-section,
  .architecture-section {
    max-width: 1220px;
    margin: 0 auto;
    padding: 5rem 1.5rem;
  }

  .section-heading {
    max-width: 760px;
    margin-bottom: 2rem;
  }

  .section-heading p,
  .kicker {
    margin: 0 0 0.6rem;
    color: #67e8f9;
    font-weight: 850;
    text-transform: uppercase;
    letter-spacing: 0.14em;
    font-size: 0.78rem;
  }

  .section-heading h2,
  .logic-panel h2,
  .arch-card h2 {
    margin: 0;
    color: #f8fafc;
    font-size: clamp(2rem, 5vw, 3.6rem);
    line-height: 1;
    letter-spacing: -0.055em;
  }

  .section-heading span,
  .logic-panel > p,
  .arch-card > p {
    display: block;
    margin-top: 1rem;
    color: #cbd5e1;
    font-size: 1.08rem;
    line-height: 1.75;
  }

  .pipeline-grid {
    display: grid;
    grid-template-columns: repeat(4, minmax(0, 1fr));
    gap: 1rem;
  }

  .pipeline-card {
    min-height: 290px;
    padding: 1.25rem;
    border-radius: 1.5rem;
    border: 1px solid rgba(148, 163, 184, 0.18);
    background: rgba(15, 23, 42, 0.72);
    transition:
      transform 0.2s ease,
      background 0.2s ease,
      border-color 0.2s ease;
  }

  .pipeline-card:hover {
    transform: translateY(-5px);
    border-color: rgba(103, 232, 249, 0.38);
    background: rgba(15, 23, 42, 0.95);
  }

  .pipeline-top {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 2.5rem;
  }

  .pipeline-icon {
    display: grid;
    place-items: center;
    width: 3rem;
    height: 3rem;
    border-radius: 1rem;
    color: #020617;
    background: linear-gradient(135deg, #67e8f9, #a78bfa);
    font-size: 1.25rem;
  }

  .pipeline-step {
    color: #64748b;
    font-weight: 900;
    font-size: 1.25rem;
  }

  .pipeline-card h3 {
    margin: 0 0 0.75rem;
    font-size: 1.25rem;
  }

  .pipeline-card p {
    margin: 0;
    color: #94a3b8;
    line-height: 1.65;
  }

  .split-section {
    display: grid;
    grid-template-columns: minmax(0, 1fr) minmax(320px, 0.8fr);
    gap: 1.5rem;
    align-items: stretch;
  }

  .logic-panel,
  .control-card,
  .vocab-card,
  .arch-card {
    border: 1px solid rgba(148, 163, 184, 0.18);
    border-radius: 2rem;
    background:
      linear-gradient(180deg, rgba(15, 23, 42, 0.82), rgba(15, 23, 42, 0.58)),
      radial-gradient(circle at top left, rgba(59, 130, 246, 0.12), transparent 22rem);
    padding: clamp(1.25rem, 4vw, 2rem);
  }

  .feature-list {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 0.8rem;
    margin-top: 2rem;
  }

  .feature-item {
    display: flex;
    gap: 0.75rem;
    align-items: flex-start;
    padding: 0.9rem;
    border-radius: 1rem;
    background: rgba(2, 6, 23, 0.42);
    border: 1px solid rgba(148, 163, 184, 0.12);
  }

  .feature-item span {
    color: #22c55e;
    font-weight: 900;
  }

  .feature-item p {
    margin: 0;
    color: #cbd5e1;
    line-height: 1.45;
  }

  .control-card {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    overflow: hidden;
    position: relative;
    min-height: 440px;
  }

  .control-card h3 {
    margin: 0;
    color: #f8fafc;
    font-size: clamp(1.7rem, 4vw, 2.55rem);
    line-height: 1.02;
    letter-spacing: -0.05em;
  }

  .control-card p:last-child {
    margin: 1rem 0 0;
    color: #cbd5e1;
    line-height: 1.7;
  }

  .control-orbit {
    position: relative;
    height: 220px;
    margin-bottom: 1.5rem;
    border-radius: 1.5rem;
    background:
      radial-gradient(circle at center, rgba(103, 232, 249, 0.2), transparent 7rem),
      rgba(2, 6, 23, 0.38);
    border: 1px solid rgba(148, 163, 184, 0.14);
  }

  .control-orbit::before {
    content: "";
    position: absolute;
    inset: 38px;
    border: 1px dashed rgba(103, 232, 249, 0.35);
    border-radius: 999px;
  }

  .orbit-center {
    position: absolute;
    left: 50%;
    top: 50%;
    display: grid;
    place-items: center;
    width: 78px;
    height: 78px;
    border-radius: 50%;
    color: #020617;
    background: linear-gradient(135deg, #67e8f9, #a78bfa);
    font-weight: 950;
    transform: translate(-50%, -50%);
    box-shadow: 0 0 45px rgba(103, 232, 249, 0.4);
  }

  .orbit-node {
    position: absolute;
    padding: 0.45rem 0.65rem;
    border-radius: 999px;
    color: #dbeafe;
    background: rgba(15, 23, 42, 0.92);
    border: 1px solid rgba(148, 163, 184, 0.22);
    font-size: 0.78rem;
    font-weight: 800;
  }

  .n1 {
    left: 50%;
    top: 20px;
    transform: translateX(-50%);
  }

  .n2 {
    right: 20px;
    top: 50%;
    transform: translateY(-50%);
  }

  .n3 {
    left: 50%;
    bottom: 20px;
    transform: translateX(-50%);
  }

  .n4 {
    left: 20px;
    top: 50%;
    transform: translateY(-50%);
  }

  .vocabulary-grid {
    display: grid;
    grid-template-columns: 0.85fr 1.15fr;
    gap: 1rem;
  }

  .vocab-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 1rem;
    margin-bottom: 1.25rem;
  }

  .vocab-header span {
    color: #94a3b8;
    font-size: 0.8rem;
    text-transform: uppercase;
    letter-spacing: 0.12em;
    font-weight: 850;
  }

  .vocab-header strong {
    padding: 0.35rem 0.65rem;
    border-radius: 999px;
    color: #052e16;
    background: #86efac;
    font-size: 0.8rem;
  }

  .chip-grid {
    display: flex;
    flex-wrap: wrap;
    gap: 0.75rem;
  }

  .chip {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    padding: 0.75rem 1rem;
    border-radius: 999px;
    font-weight: 850;
    border: 1px solid rgba(148, 163, 184, 0.2);
  }

  .action-chip {
    color: #020617;
    background: linear-gradient(135deg, #67e8f9, #a78bfa);
  }

  .asset-board {
    display: grid;
    grid-template-columns: repeat(4, minmax(0, 1fr));
    gap: 0.8rem;
  }

  .asset-token {
    min-height: 158px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    padding: 0.85rem;
    border-radius: 1.2rem;
    background:
      radial-gradient(circle at top right, rgba(103, 232, 249, 0.16), transparent 5rem),
      rgba(2, 6, 23, 0.5);
    border: 1px solid rgba(148, 163, 184, 0.14);
    overflow: hidden;
  }

  .asset-token p {
    margin: 0;
    color: #e2e8f0;
    font-weight: 750;
    line-height: 1.25;
  }

  .asset-token small {
    display: block;
    margin-top: 0.25rem;
    color: #94a3b8;
    font-size: 0.72rem;
    line-height: 1.2;
  }

  .physical-asset {
    position: relative;
    width: 100%;
    height: 82px;
    border-radius: 1rem;
    background:
      radial-gradient(circle at 50% 18%, rgba(255, 255, 255, 0.08), transparent 4.5rem),
      rgba(15, 23, 42, 0.58);
    border: 1px solid rgba(148, 163, 184, 0.1);
    overflow: hidden;
  }

  .physical-asset span {
    position: absolute;
    display: block;
  }

  .seat-panel {
    perspective: 420px;
  }

  .seat-slat {
    top: 19px;
    left: calc(8px + var(--i) * 10.5px);
    width: 8px;
    height: 48px;
    border-radius: 0.18rem;
    background: linear-gradient(135deg, #d6b083, #b8824a 45%, #8b5e34);
    box-shadow: 0 10px 18px rgba(0, 0, 0, 0.28);
    transform: skewY(-9deg);
  }

  .seat-rail {
    left: 8px;
    width: 104px;
    height: 7px;
    border-radius: 999px;
    background: linear-gradient(90deg, #7c4f28, #9e6e3a, #7c4f28);
    opacity: 0.92;
  }

  .rail-a {
    top: 29px;
  }

  .rail-b {
    bottom: 17px;
  }

  .side-frame .frame-post {
    top: 14px;
    width: 10px;
    height: 52px;
    border-radius: 0.25rem;
    background: linear-gradient(135deg, #d6b083, #b8824a 45%, #8b5e34);
    box-shadow: 0 10px 18px rgba(0, 0, 0, 0.3);
  }

  .side-frame .post-a {
    left: 28px;
  }

  .side-frame .post-b {
    right: 28px;
  }

  .side-frame .frame-rail {
    left: 28px;
    right: 28px;
    height: 10px;
    border-radius: 0.25rem;
    background: linear-gradient(135deg, #d6b083, #b8824a 45%, #8b5e34);
    box-shadow: 0 10px 18px rgba(0, 0, 0, 0.3);
  }

  .side-frame .frame-top {
    top: 15px;
  }

  .side-frame .frame-bottom {
    bottom: 15px;
  }

  .dowel-body {
    left: 50%;
    top: 16px;
    width: 18px;
    height: 48px;
    border-radius: 999px;
    background: linear-gradient(90deg, #8b5e34, #fde68a 48%, #92400e);
    box-shadow: 0 14px 28px rgba(0, 0, 0, 0.32);
    transform: translateX(-50%) rotate(24deg);
  }

  .screw-head {
    left: 50%;
    border-radius: 999px;
    background: linear-gradient(90deg, #475569, #f8fafc 45%, #334155);
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.34);
    transform: translateX(-50%) rotate(-16deg);
  }

  .screw-shaft {
    left: 50%;
    border-radius: 999px;
    background: linear-gradient(90deg, #475569, #e2e8f0 45%, #334155);
    box-shadow: 0 14px 28px rgba(0, 0, 0, 0.32);
    transform: translateX(-50%) rotate(-16deg);
  }

  .long-head {
    top: 10px;
    width: 31px;
    height: 12px;
  }

  .long-shaft {
    top: 20px;
    width: 9px;
    height: 51px;
  }

  .medium-head {
    top: 17px;
    width: 27px;
    height: 11px;
  }

  .medium-shaft {
    top: 27px;
    width: 8px;
    height: 39px;
  }

  .short-head {
    top: 25px;
    width: 23px;
    height: 10px;
  }

  .short-shaft {
    top: 34px;
    width: 6px;
    height: 28px;
  }

  .allen-long {
    left: 34px;
    top: 46px;
    width: 72px;
    height: 8px;
    border-radius: 999px;
    background: linear-gradient(90deg, #1f2937, #64748b, #111827);
    transform: rotate(-8deg);
    box-shadow: 0 14px 28px rgba(0, 0, 0, 0.32);
  }

  .allen-short {
    left: 31px;
    top: 18px;
    width: 8px;
    height: 36px;
    border-radius: 999px;
    background: linear-gradient(180deg, #64748b, #111827);
    transform: rotate(-8deg);
  }

  .gripper-palm {
    left: 35px;
    top: 20px;
    width: 58px;
    height: 14px;
    border-radius: 0.35rem;
    background: linear-gradient(135deg, #64748b, #445566, #1e293b);
    opacity: 0.78;
    box-shadow: 0 14px 28px rgba(0, 0, 0, 0.3);
  }

  .gripper-finger {
    top: 35px;
    width: 12px;
    height: 34px;
    border-radius: 0.35rem;
    background: linear-gradient(180deg, #64748b, #1e293b);
    opacity: 0.78;
  }

  .finger-left {
    left: 43px;
    transform: rotate(7deg);
  }

  .finger-right {
    right: 43px;
    transform: rotate(-7deg);
  }

  pre {
    white-space: pre-wrap;
    overflow: auto;
    margin: 0;
    padding: 1rem;
    border-radius: 1rem;
    color: #bfdbfe;
    background: #020617;
    border: 1px solid rgba(148, 163, 184, 0.15);
    font-size: 0.86rem;
    line-height: 1.6;
    font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace;
  }

  .demo-grid {
    display: grid;
    grid-template-columns: minmax(260px, 0.9fr) minmax(420px, 1.25fr) minmax(260px, 0.9fr);
    gap: 1rem;
    align-items: stretch;
  }

  .demo-card {
    min-height: 520px;
    min-width: 0;
    padding: 1rem;
    border-radius: 1.5rem;
    border: 1px solid rgba(148, 163, 184, 0.18);
    background: rgba(15, 23, 42, 0.72);
  }

  .demo-card.visual,
  .demo-card.render {
    min-width: 260px;
  }

  .demo-card.code {
    min-width: 0;
  }

  .demo-card-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 1rem;
    margin-bottom: 1rem;
  }

  .demo-card-header span {
    color: #94a3b8;
    font-size: 0.85rem;
    text-transform: uppercase;
    letter-spacing: 0.12em;
    font-weight: 850;
  }

  .demo-card-header strong {
    color: #e2e8f0;
  }

  .placeholder-manual,
  .render-box {
    height: 440px;
    border-radius: 1.2rem;
    background: #e2e8f0;
    overflow: hidden;
  }

  .placeholder-image {
    display: block;
    width: 100%;
    height: 100%;
    object-fit: contain;
    background: #f8fafc;
  }

  .code pre {
    height: 440px;
    max-height: 440px;
    min-width: 0;
    overflow: auto;
  }

  .render-box {
    position: relative;
    background:
      radial-gradient(circle at 50% 15%, rgba(96, 165, 250, 0.34), transparent 10rem),
      linear-gradient(180deg, #111827, #020617);
  }

  .render-box .placeholder-image {
    background: transparent;
  }

  .arch-card {
    text-align: center;
  }

  .arch-flow {
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    justify-content: center;
    gap: 0.75rem;
    margin: 2rem 0;
  }

  .arch-flow div {
    padding: 0.95rem 1rem;
    border-radius: 1rem;
    background: rgba(2, 6, 23, 0.58);
    border: 1px solid rgba(148, 163, 184, 0.18);
    color: #e2e8f0;
    font-weight: 800;
  }

  .arch-flow span {
    color: #67e8f9;
    font-weight: 900;
  }

  @media (max-width: 1050px) {
    .hero,
    .split-section,
    .demo-grid,
    .vocabulary-grid {
      grid-template-columns: 1fr;
    }

    .pipeline-grid {
      grid-template-columns: repeat(2, minmax(0, 1fr));
    }

    .asset-board {
      grid-template-columns: repeat(2, minmax(0, 1fr));
    }

    .hero-card {
      min-height: auto;
    }
  }

  @media (max-width: 720px) {
    .hero {
      padding-top: 3.5rem;
      gap: 2rem;
    }

    .hero-logo {
      width: min(100%, 520px);
      margin-bottom: 1.25rem;
    }

    .hero-metrics,
    .feature-list,
    .pipeline-grid,
    .asset-board {
      grid-template-columns: 1fr;
    }

    .hero-actions {
      flex-direction: column;
    }

    .btn {
      width: 100%;
    }

    .manual-preview {
      height: 360px;
    }

    .demo-card {
      min-height: auto;
    }

    .placeholder-manual,
    .render-box,
    .code pre {
      height: 360px;
    }

    .arch-flow {
      flex-direction: column;
    }

    .arch-flow span {
      transform: rotate(90deg);
    }
  }
</style>