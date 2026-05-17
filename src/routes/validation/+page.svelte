<script lang="ts">
  import { asset } from '$app/paths';

  type ResultItem = {
    model: string;
    key: string;
    source: string;
    output: string;
  };

  type ValidationCase = {
    id: string;
    title: string;
    description: string;
    pdf: string;
    results: ResultItem[];
  };

  type Metrics = {
    assemblyYes: number;
    assemblyNo: number;
    steps: number;
    repetitions: number;
    commands: number;
    incomplete: number;
    lines: number;
    actions: string[];
    objects: string[];
  };

  const validationCases: ValidationCase[] = [
  {
    "id": "0",
    "title": "PDF 0",
    "description": "ÄPPLARÖ bench / multi-step extraction",
    "pdf": "/validation/0.pdf",
    "results": [
      {
        "model": "Gemma 4 31B",
        "key": "gemma4_31b",
        "source": "result_gemma4_31b_0.txt",
        "output": "assembly_page(n).\nassembly_page(n).\nassembly_page(y).\nstep(1).\nrepetition(1,6).\norders_incomplete(1,4).\nrobot_command(1,1,pick,dowel,table,none,pick_up_one_of_the_6_dowelspart_128832).\nrobot_command(1,2,move,dowel,table,hole,move_the_dowel_to_the_pre_drilled_hole_on_the_frame).\nrobot_command(1,3,align,dowel,none,none,align_the_dowel_with_the_hole).\nrobot_command(1,5,release,dowel,none,none,release_the_dowel_in_the_hole).\nassembly_page(y).\nstep(2).\nstep(3).\norders_incomplete(3,2).\nrobot_command(2,1,pick,side_frame,table,none,pick_up_the_side_frame).\nrobot_command(2,2,move,side_frame,table,main_frame,move_the_side_frame_to_the_main_frame).\nrobot_command(2,3,align,side_frame,none,none,align_the_side_frame_with_the_main_frame).\nrobot_command(2,4,insert,side_frame,none,main_frame,insert_the_side_frame_into_the_main_frame).\nrobot_command(2,5,release,side_frame,none,none,release_the_side_frame_after_it_is_inserted).\nrobot_command(3,1,pick,side_frame,table,none,pick_up_the_second_side_frame).\nrobot_command(3,3,align,side_frame,none,none,align_the_side_frame_with_the_main_frame).\nrobot_command(3,4,insert,side_frame,none,main_frame,insert_the_side_frame_into_the_main_frame).\nrobot_command(3,5,release,side_frame,none,none,release_the_side_frame_after_it_is_inserted).\nassembly_page(y).\nstep(4).\nrepetition(4,4).\nrobot_command(4,1,pick,screw_long,table,none,pick_up_one_of_the_4_long_screwspart_113152).\nrobot_command(4,2,move,screw_long,table,hole,move_the_long_screw_to_the_pre_drilled_hole_on_the_frame).\nrobot_command(4,3,align,screw_long,none,none,align_the_long_screw_with_the_hole).\nrobot_command(4,4,insert,screw_long,none,hole,insert_the_long_screw_into_the_hole).\nrobot_command(4,5,pick,tool,table,none,pick_up_the_allen_keypart_100001).\nrobot_command(4,6,move,tool,table,screw_long,move_the_allen_key_to_the_head_of_the_long_screw).\nrobot_command(4,7,align,tool,none,none,descriptionalign_the_allen_key_with_the_screw_head).\nrobot_command(4,8,rotate,tool,none,none,rotate_the_allen_key_to_tighten_the_long_screw).\nrobot_command(4,9,release,tool,none,none,release_the_allen_key).\nrobot_command(4,10,release,screw_long,none,none,release_the_long_screw).\nassembly_page(y).\nstep(5).\nrepetition(5,2).\nrobot_command(5,1,pick,screw_medium,table,none,pick_up_one_of_the_medium_screws_part_106581).\nrobot_command(5,2,move,screw_medium,table,hole,move_the_medium_screw_to_the_pre_drilled_hole_on_the_frame).\nrobot_command(5,3,align,screw_medium,none,none,align_the_medium_screw_with_the_hole).\nrobot_command(5,4,insert,screw_medium,none,hole,insert_the_medium_screw_into_the_hole).\nrobot_command(5,5,pick,tool,table,none,pick_up_the_allen_key_part_100001).\nrobot_command(5,6,move,tool,table,screw_medium,move_the_allen_key_to_the_head_of_the_medium_screw).\nrobot_command(5,7,align,tool,none,none,align_the_allen_key_with_the_screw_head).\nrobot_command(5,8,rotate,tool,none,none,rotate_the_allen_key_to_tighten_the_medium_screw).\nrobot_command(5,9,release,tool,none,none,release_the_allen_key).\nrobot_command(5,10,release,screw_medium,none,none,release_the_medium_screw).\nassembly_page(y).\nstep(6).\nrepetition(6,4).\nrobot_command(6,1,pick,screw_long,table,none,pick_up_one_of_the_4_screws_part_10046802).\nrobot_command(6,2,move,screw_long,table,hole,move_the_screw_to_the_pre_drilled_hole_on_the_frame).\nrobot_command(6,3,align,screw_long,none,none,align_the_screw_with_the_hole).\nrobot_command(6,4,insert,screw_long,none,hole,insert_the_screw_into_the_hole).\nrobot_command(6,5,pick,tool,table,none,pick_up_the_allen_key).\nrobot_command(6,6,move,tool,table,screw_long,move_the_allen_key_to_the_head_of_the_screw).\nrobot_command(6,7,align,tool,none,none,align_the_allen_key_with_the_screw_head).\nrobot_command(6,8,rotate,tool,none,none,rotate_the_allen_key_to_tighten_the_screw).\nrobot_command(6,9,release,tool,none,none,release_the_allen_key).\nrobot_command(6,10,release,screw_long,none,none,release_the_screw).\nassembly_page(n)."
      },
      {
        "model": "Gemma 4 E4B",
        "key": "gemma4_e4b",
        "source": "result_gemma4_e4b_0.txt",
        "output": "assembly_page(n).\nassembly_page(n).\nassembly_page(y).\nstep(1).\nrepetition(1,6).\nrobot_command(1,1,pick,dowel,table,none,pick_up_one_dowel_from_the_parts_inventory).\nrobot_command(1,2,move,dowel,table,none,move_the_dowel_to_the_designated_hole_on_the_panel).\nrobot_command(1,3,insert,dowel,table,hole_1,insert_the_dowel_into_the_first_hole_on_the_panel).\nrobot_command(1,4,release,dowel,table,none,release_the_dowel_from_the_panel).\nrobot_command(1,5,pick,dowel,table,none,pick_up_the_second_dowel_from_the_parts_inventory).\nrobot_command(1,6,move,dowel,table,none,move_the_dowel_to_the_second_designated_hole_on_the_panel).\nrobot_command(1,7,insert,dowel,table,hole_2,insert_the_dowel_into_the_second_hole_on_the_panel).\nrobot_command(1,8,release,dowel,table,none,release_the_dowel_from_the_panel).\nassembly_page(y).\nstep(2).\nstep(3).\nrepetition(2,6).\ninconsistent_repetition(3).\nmissing_commands(3).\nrobot_command(2,1,pick,dowel,table,none,pick_up_one_dowel_from_the_parts_inventory).\nrobot_command(2,2,move,dowel,table,none,move_the_dowel_to_the_designated_hole_on_the_panel).\nrobot_command(2,3,insert,dowel,table,hole_1,insert_the_dowel_into_the_first_hole_on_the_panel).\nrobot_command(2,4,release,dowel,table,none,release_the_dowel_from_the_panel).\nrobot_command(2,5,pick,dowel,table,none,pick_up_the_second_dowel_from_the_parts_inventory).\nrobot_command(2,6,move,dowel,table,none,move_the_dowel_to_the_second_designated_hole_on_the_panel).\nrobot_command(2,7,insert,dowel,table,hole_2,insert_the_dowel_into_the_second_hole_on_the_panel).\nrobot_command(2,8,release,dowel,table,none,release_the_dowel_from_the_panel).\nassembly_page(y).\nstep(4).\nrepetition(4,6).\nrobot_command(4,1,pick,dowel,table,none,pick_up_one_dowel_from_the_parts_inventory).\nrobot_command(4,2,move,dowel,table,none,move_the_dowel_to_the_designated_hole_on_the_panel).\nrobot_command(4,3,insert,dowel,table,hole_1,insert_the_dowel_into_the_first_hole_on_the_panel).\nrobot_command(4,4,release,dowel,table,none,release_the_dowel_from_the_panel).\nrobot_command(4,5,pick,dowel,table,none,pick_up_the_second_dowel_from_the_parts_inventory).\nrobot_command(4,6,move,dowel,table,none,move_the_dowel_to_the_second_designated_hole_on_the_panel).\nrobot_command(4,7,insert,dowel,table,hole_2,insert_the_dowel_into_the_second_hole_on_the_panel).\nrobot_command(4,8,release,dowel,table,none,release_the_dowel_from_the_panel).\nassembly_page(y).\nstep(5).\nrepetition(5,6).\nrobot_command(5,1,pick,dowel,table,none,pick_up_one_dowel_from_the_parts_inventory).\nrobot_command(5,2,move,dowel,table,none,move_the_dowel_to_the_designated_hole_on_the_panel).\nrobot_command(5,3,insert,dowel,table,hole_1,insert_the_dowel_into_the_first_hole_on_the_panel).\nrobot_command(5,4,release,dowel,table,none,release_the_dowel_from_the_panel).\nrobot_command(5,5,pick,dowel,table,none,pick_up_the_second_dowel_from_the_parts_inventory).\nrobot_command(5,6,move,dowel,table,none,move_the_dowel_to_the_second_designated_hole_on_the_panel).\nrobot_command(5,7,insert,dowel,table,hole_2,insert_the_dowel_into_the_second_hole_on_the_panel).\nrobot_command(5,8,release,dowel,table,none,release_the_dowel_from_the_panel).\nassembly_page(y).\nstep(6).\nrepetition(6,4).\nrobot_command(6,1,pick,dowel,table,none,pick_up_one_dowel_from_the_parts_inventory).\nrobot_command(6,2,move,dowel,table,none,move_the_dowel_to_the_designated_hole_on_the_panel).\nrobot_command(6,3,insert,dowel,table,hole_1,insert_the_dowel_into_the_first_hole_on_the_panel).\nrobot_command(6,4,release,dowel,table,none,release_the_dowel_from_the_panel).\nrobot_command(6,5,pick,dowel,table,none,pick_up_the_second_dowel_from_the_parts_inventory).\nrobot_command(6,6,move,dowel,table,none,move_the_dowel_to_the_second_designated_hole_on_the_panel).\nrobot_command(6,7,insert,dowel,table,hole_2,insert_the_dowel_into_the_second_hole_on_the_panel).\nrobot_command(6,8,release,dowel,table,none,release_the_dowel_from_the_panel).\nassembly_page(y).\nstep(1).\nstep(2).\nstep(3).\nstep(4).\nstep(5).\nstep(6).\nrepetition(1,6).\nrepetition(2,6).\nrepetition(4,6).\nrepetition(5,6).\nrepetition(6,4).\ninconsistent_repetition(3).\nrobot_command(1,1,pick,dowel,table,none,pick_up_one_dowel_from_the_parts_inventory).\nrobot_command(1,2,move,dowel,table,none,move_the_dowel_to_the_designated_hole_on_the_panel).\nrobot_command(1,3,insert,dowel,table,hole_1,insert_the_dowel_into_the_first_hole_on_the_panel).\nrobot_command(1,4,release,dowel,table,none,release_the_dowel_from_the_panel).\nrobot_command(1,5,pick,dowel,table,none,pick_up_the_second_dowel_from_the_parts_inventory).\nrobot_command(1,6,move,dowel,table,none,move_the_dowel_to_the_second_designated_hole_on_the_panel).\nrobot_command(1,7,insert,dowel,table,hole_2,insert_the_dowel_into_the_second_hole_on_the_panel).\nrobot_command(1,8,release,dowel,table,none,release_the_dowel_from_the_panel).\nrobot_command(2,1,pick,dowel,table,none,pick_up_one_dowel_from_the_parts_inventory).\nrobot_command(2,2,move,dowel,table,none,move_the_dowel_to_the_designated_hole_on_the_panel).\nrobot_command(2,3,insert,dowel,table,hole_1,insert_the_dowel_into_the_first_hole_on_the_panel).\nrobot_command(2,4,release,dowel,table,none,release_the_dowel_from_the_panel).\nrobot_command(2,5,pick,dowel,table,none,pick_up_the_second_dowel_from_the_parts_inventory).\nrobot_command(2,6,move,dowel,table,none,move_the_dowel_to_the_second_designated_hole_on_the_panel).\nrobot_command(2,7,insert,dowel,table,hole_2,insert_the_dowel_into_the_second_hole_on_the_panel).\nrobot_command(2,8,release,dowel,table,none,release_the_dowel_from_the_panel).\nrobot_command(3,1,pick,dowel,table,none,pick_up_one_dowel_from_the_parts_inventory).\nrobot_command(3,2,move,dowel,table,none,move_the_dowel_to_the_designated_hole_on_the_panel).\nrobot_command(3,3,insert,dowel,table,hole_1,insert_the_dowel_into_the_first_hole_on_the_panel).\nrobot_command(3,4,release,dowel,table,none,release_the_dowel_from_the_panel).\nrobot_command(3,5,pick,dowel,table,none,pick_up_the_second_dowel_from_the_parts_inventory).\nrobot_command(3,6,move,dowel,table,none,move_the_dowel_to_the_second_designated_hole_on_the_panel).\nrobot_command(3,7,insert,dowel,table,hole_2,insert_the_dowel_into_the_second_hole_on_the_panel).\nrobot_command(3,8,release,dowel,table,none,release_the_dowel_from_the_panel).\nrobot_command(4,1,pick,dowel,table,none,pick_up_one_dowel_from_the_parts_inventory).\nrobot_command(4,2,move,dowel,table,none,move_the_dowel_to_the_designated_hole_on_the_panel).\nrobot_command(4,3,insert,dowel,table,hole_1,insert_the_dowel_into_the_first_hole_on_the_panel).\nrobot_command(4,4,release,dowel,table,none,release_the_dowel_from_the_panel).\nrobot_command(4,5,pick,dowel,table,none,pick_up_the_second_dowel_from_the_parts_inventory).\nrobot_command(4,6,move,dowel,table,none,move_the_dowel_to_the_second_designated_hole_on_the_panel).\nrobot_command(4,7,insert,dowel,table,hole_2,insert_the_dowel_into_the_second_hole_on_the_panel).\nrobot_command(4,8,release,dowel,table,none,release_the_dowel_from_the_panel).\nrobot_command(5,1,pick,dowel,table,none,pick_up_one_dowel_from_the_parts_inventory).\nrobot_command(5,2,move,dowel,table,none,move_the_dowel_to_the_designated_hole_on_the_panel).\nrobot_command(5,3,insert,dowel,table,hole_1,insert_the_dowel_into_the_first_hole_on_the_panel).\nrobot_command(5,4,release,dowel,table,none,release_the_dowel_from_the_panel).\nrobot_command(5,5,pick,dowel,table,none,pick_up_the_second_dowel_from_the_parts_inventory).\nrobot_command(5,6,move,dowel,table,none,move_the_dowel_to_the_second_designated_hole_on_the_panel).\nrobot_command(5,7,insert,dowel,table,hole_2,insert_the_dowel_into_the_second_hole_on_the_panel).\nrobot_command(5,8,release,dowel,table,none,release_the_dowel_from_the_panel).\nrobot_command(6,1,pick,dowel,table,none,pick_up_one_dowel_from_the_parts_inventory).\nrobot_command(6,2,move,dowel,table,none,move_the_dowel_to_the_designated_hole_on_the_panel).\nrobot_command(6,3,insert,dowel,table,hole_1,insert_the_dowel_into_the_first_hole_on_the_panel).\nrobot_command(6,4,release,dowel,table,none,release_the_dowel_from_the_panel).\nrobot_command(6,5,pick,dowel,table,none,pick_up_the_second_dowel_from_the_parts_inventory).\nrobot_command(6,6,move,dowel,table,none,move_the_dowel_to_the_second_designated_hole_on_the_panel).\nrobot_command(6,7,insert,dowel,table,hole_2,insert_the_dowel_into_the_second_hole_on_the_panel).\nrobot_command(6,8,release,dowel,table,none,release_the_dowel_from_the_panel)."
      }
    ]
  },
  {
    "id": "1",
    "title": "PDF 1",
    "description": "Assembly sequence with extended symbolic trace",
    "pdf": "/validation/1.pdf",
    "results": [
      {
        "model": "Gemma 4 31B",
        "key": "gemma4_31b",
        "source": "result_gemma4_31b_1.txt",
        "output": "assembly_page(n).\nassembly_page(n).\nassembly_page(n).\nassembly_page(y).\nstep(1).\nrepetition(1,4).\nrobot_command(1,1,pick,dowel,table,none,pick_up_one_of_the_4x_dowelspart_113004).\nrobot_command(1,2,move,dowel,table,hole,move_the_dowel_to_the_hole_on_the_side_of_the_panel).\nrobot_command(1,3,align,dowel,none,none,align_the_dowel_with_the_hole_on_the_panel).\nrobot_command(1,4,insert,dowel,none,hole,insert_the_dowel_into_the_hole_on_the_panelrepeating_this_for_4x_dowels).\nrobot_command(1,5,release,dowel,none,none,release_the_dowel_after_it_is_fully_inserted).\nassembly_page(y).\nstep(2).\nstep(3).\nrepetition(2,4).\nrepetition(3,4).\norders_incomplete(3,4).\nrobot_command(2,1,pick,dowel,table,none,pick_up_one_of_the_4x_dowels).\nrobot_command(2,2,move,dowel,table,hole,move_the_dowel_to_the_hole_on_the_side_of_the_panel).\nrobot_command(2,3,align,dowel,none,none,align_the_dowel_with_the_hole_on_the_panel).\nrobot_command(2,4,insert,dowel,none,hole,insert_the_dowel_into_the_hole_on_the_panel_repeating_this_for_4x_dowels).\nrobot_command(2,5,release,dowel,none,none,release_the_dowel_after_it_is_fully_inserted).\nrobot_command(3,1,pick,dowel,table,none,pick_up_one_of_the_4x_dowels_part_128995).\nrobot_command(3,2,move,dowel,table,hole,move_the_dowel_to_the_hole_on_the_panel).\nrobot_command(3,3,align,dowel,none,none,align_the_dowel_with_the_hole_on_the_panel).\nrobot_command(3,5,release,dowel,none,none,release_the_dowel_after_it_is_fully_inserted).\nassembly_page(y).\nstep(4).\nrepetition(4,4).\nrobot_command(4,1,pick,dowel,table,none,pick_up_one_of_the_4x_dowels).\nrobot_command(4,2,move,dowel,table,hole,move_the_dowel_to_the_hole_on_the_side_of_the_panel).\nrobot_command(4,3,align,dowel,none,none,align_the_dowel_with_the_hole_on_the_panel).\nrobot_command(4,4,insert,dowel,none,hole,insert_the_dowel_into_the_hole_on_the_panel_repeating_this_for_4x_dowels).\nrobot_command(4,5,release,dowel,none,none,release_the_dowel_after_it_is_fully_inserted).\nassembly_page(y).\nstep(5).\nrepetition(5,12).\nrobot_command(5,1,pick,screw_long,table,none,pick_up_one_of_the_12x_screws_part_117794).\nrobot_command(5,2,move,screw_long,table,hole,move_the_screw_to_the_hole_on_the_panel).\nrobot_command(5,3,align,screw_long,none,none,align_the_screw_with_the_hole_on_the_panel).\nrobot_command(5,4,insert,screw_long,none,hole,insert_the_screw_into_the_hole_on_the_panelrepeating_this_for_12x_screws).\nrobot_command(5,5,rotate,screw_long,none,none,rotate_the_screw_using_the_allen_key_part_100001_to_tighten_it).\nrobot_command(5,6,release,screw_long,none,none,release_the_screw_after_it_is_fully_tightened).\nassembly_page(y).\nstep(6).\nrepetition(6,4).\nrobot_command(6,1,pick,dowel,table,none,pick_up_one_of_the_4x_dowels_part_128832).\nrobot_command(6,2,move,dowel,table,hole,move_the_dowel_to_the_hole_on_the_side_of_the_panel).\nrobot_command(6,3,align,dowel,none,none,align_the_dowel_with_the_hole_on_the_panel).\nrobot_command(6,4,insert,dowel,none,hole,insert_the_dowel_into_the_hole_on_the_panel_repeating_this_for_4x_dowels).\nrobot_command(6,5,release,dowel,none,none,release_the_dowel_after_it_is_fully_inserted).\nassembly_page(y).\nstep(7).\nrepetition(7,2).\nrobot_command(7,1,pick,dowel,table,none,pick_up_one_of_the_2x_dowels).\nrobot_command(7,2,move,dowel,table,hole,move_the_dowel_to_the_hole_on_the_side_of_the_panel).\nrobot_command(7,3,align,dowel,none,none,align_the_dowel_with_the_hole_on_the_panel).\nrobot_command(7,4,insert,dowel,none,hole,insert_the_dowel_into_the_hole_on_the_panel_repeating_this_for_2x_dowels).\nrobot_command(7,5,release,dowel,none,none,release_the_dowel_after_it_is_fully_inserted).\nassembly_page(y).\nstep(8).\nrepetition(8,2).\nrobot_command(8,1,pick,screw_long,table,none,pick_up_one_of_the_2x_screws_part_113134).\nrobot_command(8,2,move,screw_long,table,hole,move_the_screw_to_the_hole_on_the_panel).\nrobot_command(8,3,align,screw_long,none,none,align_the_screw_with_the_hole_on_the_panel).\nrobot_command(8,4,insert,screw_long,none,hole,insert_the_screw_into_the_hole_on_the_panel_repeating_this_for_2x_screws).\nrobot_command(8,5,rotate,screw_long,none,none,rotate_the_screw_using_the_allen_key_part_100001_to_tighten_it).\nrobot_command(8,6,release,screw_long,none,none,release_the_screw_after_it_is_fully_tightened).\nassembly_page(y).\nstep(9).\nrepetition(9,3).\nrobot_command(9,1,pick,screw_long,table,none,pick_up_one_of_the_3x_screws_part_117794).\nrobot_command(9,2,move,screw_long,table,hole,move_the_screw_to_the_hole_on_the_panel).\nrobot_command(9,3,align,screw_long,none,none,align_the_screw_with_the_hole_on_the_panel).\nrobot_command(9,4,insert,screw_long,none,hole,insert_the_screw_into_the_hole_on_the_panel_repeating_this_for_3x_screws).\nrobot_command(9,5,rotate,screw_long,none,none,rotate_the_screw_using_the_allen_key_part_100001_to_tighten_it).\nrobot_command(9,6,release,screw_long,none,none,release_the_screw_after_it_is_fully_tightened).\nassembly_page(y).\nstep(10).\nrepetition(10,2).\nrobot_command(10,1,pick,side_frame,table,none,pick_up_the_side_frame).\nrobot_command(10,2,move,side_frame,table,bed_frame,move_the_side_frame_towards_the_bed_frame_as_indicated_by_the_arrow).\nrobot_command(10,3,align,side_frame,none,none,align_the_side_frame_with_the_bed_frame).\nrobot_command(10,4,insert,side_frame,none,bed_frame,insert_the_side_frame_into_the_bed_frame_repeating_this_for_2x_connections_as_shown_in_the_magnified_view).\nrobot_command(10,5,release,side_frame,none,none,release_the_side_frame_after_it_is_fully_inserted).\nassembly_page(y).\nstep(11).\nrepetition(11,2).\nrobot_command(11,1,pick,dowel,table,none,pick_up_one_of_the_2x_dowels_part_113153).\nrobot_command(11,2,move,dowel,table,hole,move_the_dowel_to_the_hole_on_the_side_of_the_frame).\nrobot_command(11,3,align,dowel,none,none,align_the_dowel_with_the_hole_on_the_frame).\nrobot_command(11,4,insert,dowel,none,hole,insert_the_dowel_into_the_hole_on_the_frame_repeating_this_for_2x_dowels).\nrobot_command(11,5,release,dowel,none,none,release_the_dowel_after_it_is_fully_inserted).\nrobot_command(11,6,pick,screw_long,table,none,pick_up_one_of_the_2x_screws_part_113134).\nrobot_command(11,7,move,screw_long,table,hole,move_the_screw_to_the_hole_on_the_frame).\nrobot_command(11,8,align,screw_long,none,none,align_the_screw_with_the_hole_on_the_frame).\nrobot_command(11,9,insert,screw_long,none,hole,insert_the_screw_into_the_hole_on_the_frame_repeating_this_for_2x_screws).\nrobot_command(11,10,rotate,screw_long,none,none,rotate_the_screw_using_the_allen_key_part_100001_to_tighten_it).\nrobot_command(11,11,release,screw_long,none,none,release_the_screw_after_it_is_fully_tightened).\nassembly_page(y).\nstep(12).\nrepetition(12,3).\nrobot_command(12,1,pick,screw_long,table,none,pick_up_one_of_the_3x_screws_part_117794).\nrobot_command(12,2,move,screw_long,table,hole,move_the_screw_to_the_hole_on_the_frame).\nrobot_command(12,3,align,screw_long,none,none,align_the_screw_with_the_hole_on_the_frame).\nrobot_command(12,4,insert,screw_long,none,hole,insert_the_screw_into_the_hole_on_the_frame_repeating_this_for_3x_screws).\nrobot_command(12,5,rotate,screw_long,none,none,rotate_the_screw_using_the_allen_key_part_100001_to_tighten_it).\nrobot_command(12,6,release,screw_long,none,none,release_the_screw_after_it_is_fully_tightened).\nassembly_page(y).\nstep(13).\nrepetition(13,2).\nrobot_command(13,1,pick,screw_long,table,none,pick_up_one_of_the_2x_screws_part_105089).\nrobot_command(13,2,move,screw_long,table,hole,move_the_screw_to_the_hole_on_the_frame).\nrobot_command(13,3,align,screw_long,none,none,align_the_screw_with_the_hole_on_the_frame).\nrobot_command(13,4,insert,screw_long,none,hole,insert_the_screw_into_the_hole_on_the_frame_repeating_this_for_2x_screws).\nrobot_command(13,5,rotate,screw_long,none,none,rotate_the_screw_using_the_allen_key_part_100001_to_tighten_it).\nrobot_command(13,6,release,screw_long,none,none,release_the_screw_after_it_is_fully_tightened).\nassembly_page(y).\nstep(14).\nrepetition(14,2).\nrobot_command(14,1,pick,screw_long,table,none,pick_up_one_of_the_2x_screws_part_105089).\nrobot_command(14,2,move,screw_long,table,hole,move_the_screw_to_the_hole_on_the_frame).\nrobot_command(14,3,align,screw_long,none,none,align_the_screw_with_the_hole_on_the_frame).\nrobot_command(14,4,insert,screw_long,none,hole,insert_the_screw_into_the_hole_on_the_frame_repeating_this_for_2x_screws).\nrobot_command(14,5,rotate,screw_long,none,none,rotate_the_screw_using_the_allen_key_part_100001_to_tighten_it).\nrobot_command(14,6,rotate,screw_long,none,none,rotate_the_screw_using_the_screwdriver_part_113107_to_tighten_it).\nrobot_command(14,7,release,screw_long,none,none,release_the_screw_after_it_is_fully_tightened).\nassembly_page(y).\nstep(15).\nrepetition(15,1).\nrobot_command(15,1,pick,screw_long,table,none,pick_up_the_screw_part_10089487).\nrobot_command(15,2,move,screw_long,table,hole,move_the_screw_to_the_hole_on_the_frame).\nrobot_command(15,3,align,screw_long,none,none,align_the_screw_with_the_hole_on_the_frame).\nrobot_command(15,4,insert,screw_long,none,hole,insert_the_screw_into_the_hole_on_the_frame).\nrobot_command(15,5,rotate,screw_long,none,none,rotate_the_screw_using_the_allen_key_part_100001_to_tighten_it).\nrobot_command(15,6,release,screw_long,none,none,release_the_screw_after_it_is_fully_tightened).\nassembly_page(y).\nstep(16).\nrepetition(16,1).\nrobot_command(16,1,pick,screw_long,table,none,pick_up_the_screw_part_10089487).\nrobot_command(16,2,move,screw_long,table,hole,move_the_screw_to_the_hole_on_the_frame).\nrobot_command(16,3,align,screw_long,none,none,align_the_screw_with_the_hole_on_the_frame).\nrobot_command(16,4,insert,screw_long,none,hole,insert_the_screw_into_the_hole_on_the_frame).\nrobot_command(16,5,rotate,screw_long,none,none,rotate_the_screw_using_the_allen_key_part_100001_to_tighten_it).\nrobot_command(16,6,release,screw_long,none,none,release_the_screw_after_it_is_fully_tightened).\nassembly_page(y).\nstep(17).\nrobot_command(17,1,move,side_frame,table,bed_frame,move_the_side_frame_towards_the_bed_frame_as_indicated_by_the_arrow).\nrobot_command(17,2,align,side_frame,none,none,align_the_side_frame_with_the_bed_frame).\nrobot_command(17,3,insert,side_frame,none,bed_frame,insert_the_side_frame_into_the_bed_frame).\nrobot_command(17,4,release,side_frame,none,none,release_the_side_frame_after_it_is_fully_inserted).\nassembly_page(n)."
      },
      {
        "model": "Gemma 4 E4B",
        "key": "gemma4_e4b",
        "source": "result_gemma4_e4b_1.txt",
        "output": "assembly_page(n).\nassembly_page(n).\nassembly_page(n).\nassembly_page(y).\nstep(1).\nrepetition(1,4).\nrobot_command(1,1,pick,dowel,none,none,pick_up_one_dowel113004).\nrobot_command(1,2,move,dowel,none,none,move_the_dowel_to_the_side_panel_hole).\nrobot_command(1,3,insert,dowel,none,side_panel_hole,insert_the_dowel_into_the_side_panel_hole).\nrobot_command(1,4,release,dowel,none,none,release_the_dowel).\nassembly_page(y).\nstep(2).\nstep(3).\nrepetition(2,4).\ninconsistent_repetition(3).\nrobot_command(2,1,pick,dowel,none,none,pick_up_one_dowel).\nrobot_command(2,2,move,dowel,none,none,move_the_dowel_to_the_side_panel_hole).\nrobot_command(2,3,insert,dowel,none,side_panel_hole,insert_the_dowel_into_the_side_panel_hole).\nrobot_command(2,4,release,dowel,none,none,release_the_dowel).\nrobot_command(3,1,pick,dowel,none,none,pick_up_one_dowel).\nrobot_command(3,2,move,dowel,none,none,move_the_dowel_to_the_side_panel_hole).\nrobot_command(3,3,insert,dowel,none,side_panel_hole,insert_the_dowel_into_the_side_panel_hole).\nrobot_command(3,4,release,dowel,none,none,release_the_dowel).\nassembly_page(y).\nstep(4).\nrepetition(4,4).\nrobot_command(4,1,pick,dowel,none,none,pick_up_one_dowel).\nrobot_command(4,2,move,dowel,none,none,move_the_dowel_to_the_side_panel_hole).\nrobot_command(4,3,insert,dowel,none,side_panel_hole,insert_the_dowel_into_the_side_panel_hole).\nrobot_command(4,4,release,dowel,none,none,release_the_dowel).\nassembly_page(y).\nstep(5).\nrepetition(5,12).\nrobot_command(5,1,pick,dowel,none,none,pick_up_one_dowel).\nrobot_command(5,2,move,dowel,none,none,move_the_dowel_to_the_side_panel_hole).\nrobot_command(5,3,insert,dowel,none,side_panel_hole,insert_the_dowel_into_the_side_panel_hole).\nrobot_command(5,4,release,dowel,none,none,release_the_dowel).\nassembly_page(y).\n\n\nassembly_page(y).\nstep(7).\nrepetition(7,2).\nrobot_command(7,1,pick,dowel,none,none,pick_up_one_dowel).\nrobot_command(7,2,move,dowel,none,none,move_the_dowel_to_the_side_panel_hole).\nrobot_command(7,3,insert,dowel,none,side_panel_hole,insert_the_dowel_into_the_side_panel_hole).\nrobot_command(7,4,release,dowel,none,none,release_the_dowel).\nassembly_page(y).\n\n\nassembly_page(y).\nstep(9).\nrepetition(9,3).\nrobot_command(9,1,pick,dowel,none,none,pick_up_one_dowel).\nrobot_command(9,2,move,dowel,none,none,move_the_dowel_to_the_side_panel_hole).\nrobot_command(9,3,insert,dowel,none,side_panel_hole,insert_the_dowel_into_the_side_panel_hole).\nrobot_command(9,4,release,dowel,none,none,release_the_dowel).\nassembly_page(y).\nstep(10).\nrepetition(10,2).\nrobot_command(10,1,pick,dowel,none,none,pick_up_one_dowel).\nrobot_command(10,2,move,dowel,none,none,move_the_dowel_to_the_side_panel_hole).\nrobot_command(10,3,insert,dowel,none,side_panel_hole,insert_the_dowel_into_the_side_panel_hole).\nrobot_command(10,4,release,dowel,none,none,release_the_dowel).\nassembly_page(y).\n\n\nassembly_page(y).\n\n\nassembly_page(y).\n\nassembly_page(y).\n\n\nassembly_page(y).\n\n\nassembly_page(y).\n\nassembly_page(y).\n\nassembly_page(y).\n\n"
      }
    ]
  },
  {
    "id": "3",
    "title": "PDF 3",
    "description": "Compact table/leg assembly case",
    "pdf": "/validation/3.pdf",
    "results": [
      {
        "model": "Gemma 4 31B",
        "key": "gemma4_31b",
        "source": "result_gemma4_31b_3.txt",
        "output": "assembly_page(n).\nassembly_page(n).\nassembly_page(y).\nstep(1).\nrepetition(1,4).\nrobot_command(1,1,pick,leg,none,none,pick_up_one_of_the_four_legs).\nrobot_command(1,2,move,leg,none,table_top,move_the_leg_to_the_corner_of_the_table_top).\nrobot_command(1,3,align,leg,none,none,align_the_leg_with_the_hole_in_the_table_top).\nrobot_command(1,4,insert,leg,none,table_top_hole,insert_the_leg_into_the_hole_of_the_table_toprepeating_this_for_all_4_legs).\nrobot_command(1,5,rotate,leg,none,none,rotate_the_leg_to_lock_it_in_place).\nrobot_command(1,6,release,leg,none,none,release_the_leg).\nassembly_page(y).\nstep(2).\nrepetition(2,4).\nrobot_command(2,1,pick,leg,none,none,pick_up_one_of_the_four_legs).\nrobot_command(2,2,move,leg,none,table_top,move_the_leg_to_the_corner_of_the_table_top).\nrobot_command(2,3,align,leg,none,none,align_the_leg_with_the_hole_in_the_table_top).\nrobot_command(2,4,insert,leg,none,table_top_hole,insert_the_leg_into_the_hole_of_the_table_top_repeating_this_for_all_4_legs).\nrobot_command(2,5,rotate,leg,none,none,rotate_the_leg_to_lock_it_in_place).\nrobot_command(2,6,release,leg,none,none,release_the_leg)."
      },
      {
        "model": "Gemma 4 E4B",
        "key": "gemma4_e4b",
        "source": "result_gemma4_e4b_3.txt",
        "output": "assembly_page(n).\nassembly_page(n).\nassembly_page(y).\nstep(1).\nrepetition(1,4).\nrobot_command(1,1,pick,dowel,none,none,pick_up_the_first_dowel).\nrobot_command(1,2,move,dowel,none,hole_1,move_the_dowel_to_the_first_hole).\nrobot_command(1,3,insert,dowel,hole_1,none,insert_the_dowel_into_the_first_hole).\nrobot_command(1,4,release,dowel,hole_1,none,release_the_dowel_in_the_first_hole).\nrobot_command(1,5,pick,dowel,none,none,pick_up_the_second_dowel).\nrobot_command(1,6,move,dowel,none,hole_2,move_the_dowel_to_the_second_hole).\nrobot_command(1,7,insert,dowel,hole_2,none,insert_the_dowel_into_the_second_hole).\nrobot_command(1,8,release,dowel,hole_2,none,release_the_dowel_in_the_second_hole).\nrobot_command(1,9,pick,dowel,none,none,pick_up_the_third_dowel).\nrobot_command(1,10,move,dowel,none,hole_3,move_the_dowel_to_the_third_hole).\nrobot_command(1,11,insert,dowel,hole_3,none,insert_the_dowel_into_the_third_hole).\nrobot_command(1,12,release,dowel,hole_3,none,release_the_dowel_in_the_third_hole).\nassembly_page(y).\nstep(2).\nrepetition(2,4).\nrobot_command(2,1,pick,dowel,none,none,pick_up_the_first_dowel).\nrobot_command(2,2,move,dowel,none,hole_1,move_the_dowel_to_the_first_hole).\nrobot_command(2,3,insert,dowel,none,hole_1,insert_the_dowel_into_the_first_hole).\nrobot_command(2,4,release,dowel,none,none,release_the_dowel_in_the_first_hole).\nrobot_command(2,5,pick,dowel,none,none,pick_up_the_second_dowel).\nrobot_command(2,6,move,dowel,none,hole_2,move_the_dowel_to_the_second_hole).\nrobot_command(2,7,insert,dowel,none,hole_2,insert_the_dowel_into_the_second_hole).\nrobot_command(2,8,release,dowel,none,none,release_the_dowel_in_the_second_hole).\nrobot_command(2,9,pick,dowel,none,none,pick_up_the_third_dowel).\nrobot_command(2,10,move,dowel,none,hole_3,move_the_dowel_to_the_third_hole).\nrobot_command(2,11,insert,dowel,none,hole_3,insert_the_dowel_into_the_third_hole).\nrobot_command(2,12,release,dowel,none,none,release_the_dowel_in_the_third_hole)."
      }
    ]
  }
];

  let selectedCaseId = $state(validationCases[0].id);
  let selectedModelKey = $state(validationCases[0].results[0].key);

  const selectedCase = $derived(
    validationCases.find((item) => item.id === selectedCaseId) ?? validationCases[0]
  );

  const selectedResult = $derived(
    selectedCase.results.find((item) => item.key === selectedModelKey) ?? selectedCase.results[0]
  );

  const selectedPdfUrl = $derived(asset(selectedCase.pdf));

  const metricsByResult = $derived(
    Object.fromEntries(
      validationCases.flatMap((testCase) =>
        testCase.results.map((result) => [
          `${testCase.id}:${result.key}`,
          computeMetrics(result.output)
        ])
      )
    ) as Record<string, Metrics>
  );

  const selectedMetrics = $derived(metricsByResult[`${selectedCase.id}:${selectedResult.key}`]);

  const modelSummaries = $derived(
    selectedCase.results.map((result) => ({
      ...result,
      metrics: metricsByResult[`${selectedCase.id}:${result.key}`]
    }))
  );

  const globalStats = $derived({
    cases: validationCases.length,
    models: new Set(validationCases.flatMap((item) => item.results.map((result) => result.key))).size,
    outputs: validationCases.reduce((acc, item) => acc + item.results.length, 0),
    commands: validationCases.reduce(
      (acc, item) =>
        acc +
        item.results.reduce(
          (innerAcc, result) => innerAcc + metricsByResult[`${item.id}:${result.key}`].commands,
          0
        ),
      0
    )
  });

  function computeMetrics(output: string): Metrics {
    const lines = output
      .split('\n')
      .map((line) => line.trim())
      .filter(Boolean);

    const commands = lines.filter((line) => line.startsWith('robot_command('));
    const actionSet = new Set<string>();
    const objectSet = new Set<string>();

    for (const command of commands) {
      const match = command.match(/^robot_command\(\d+,\d+,([^,]+),([^,]+),/);
      if (match) {
        actionSet.add(match[1]);
        objectSet.add(match[2]);
      }
    }

    return {
      assemblyYes: lines.filter((line) => line === 'assembly_page(y).').length,
      assemblyNo: lines.filter((line) => line === 'assembly_page(n).').length,
      steps: lines.filter((line) => /^step\(\d+\)\.$/.test(line)).length,
      repetitions: lines.filter((line) => line.startsWith('repetition(')).length,
      commands: commands.length,
      incomplete: lines.filter((line) => line.startsWith('orders_incomplete(')).length,
      lines: lines.length,
      actions: Array.from(actionSet).sort(),
      objects: Array.from(objectSet).sort()
    };
  }

  function selectCase(id: string) {
    selectedCaseId = id;
    const nextCase = validationCases.find((item) => item.id === id) ?? validationCases[0];
    selectedModelKey = nextCase.results[0].key;
  }
</script>

<svelte:head>
  <title>ASPembly | Validation</title>
  <meta
    name="description"
    content="Validation page for comparing ASPembly symbolic outputs across PDF cases and model runs."
  />
</svelte:head>

<main class="page">
  <section class="hero">
    <div class="hero-bg"></div>

    <div class="hero-content">
      <div class="eyebrow">
        <span class="dot"></span>
        Validation dashboard
      </div>

      <h1>
        Model outputs over
        <span>assembly manuals.</span>
      </h1>

      <p class="hero-subtitle">
        Confronto diretto tra pagine PDF, output ASP generati dai modelli e indicatori
        strutturali estratti automaticamente dai fatti prodotti.
      </p>
    </div>

    <div class="summary-card">
      <div class="window-bar">
        <span></span>
        <span></span>
        <span></span>
      </div>

      <div class="summary-grid">
        <div>
          <strong>{globalStats.cases}</strong>
          <span>PDF</span>
        </div>
        <div>
          <strong>{globalStats.models}</strong>
          <span>models</span>
        </div>
        <div>
          <strong>{globalStats.outputs}</strong>
          <span>outputs</span>
        </div>
        <div>
          <strong>{globalStats.commands}</strong>
          <span>commands</span>
        </div>
      </div>

      <div class="trace-mini">
        <span>validation target</span>
        <span>assembly_page · step · repetition · orders_incomplete · robot_command</span>
      </div>
    </div>
  </section>

  <section class="section">
    <div class="section-heading">
      <p>Test cases</p>
      <h2>Seleziona il PDF da ispezionare.</h2>
      <span>
        Ogni caso mostra il manuale sorgente e i risultati prodotti dai modelli disponibili.
      </span>
    </div>

    <div class="case-grid">
      {#each validationCases as testCase}
        <button
          type="button"
          class:active={selectedCase.id === testCase.id}
          class="case-card"
          onclick={() => selectCase(testCase.id)}
        >
          <span>PDF {testCase.id}</span>
          <strong>{testCase.title}</strong>
          <small>{testCase.description}</small>
        </button>
      {/each}
    </div>
  </section>

  <section class="workspace-section">
    <article class="pdf-panel">
      <div class="panel-header">
        <div>
          <span>Input</span>
          <strong>{selectedCase.title}</strong>
        </div>
        <a href={selectedPdfUrl} target="_blank" rel="noopener noreferrer">Open PDF</a>
      </div>

      <iframe src={selectedPdfUrl} title={selectedCase.title} class="pdf-viewer"></iframe>
    </article>

    <article class="result-panel">
      <div class="panel-header">
        <div>
          <span>Model output</span>
          <strong>{selectedResult.model}</strong>
        </div>
        <small>{selectedResult.source}</small>
      </div>

      <div class="model-tabs">
        {#each selectedCase.results as result}
          <button
            type="button"
            class:active={selectedModelKey === result.key}
            onclick={() => (selectedModelKey = result.key)}
          >
            {result.model}
          </button>
        {/each}
      </div>

      <div class="metric-grid">
        <div class="metric-card">
          <span>steps</span>
          <strong>{selectedMetrics.steps}</strong>
        </div>
        <div class="metric-card">
          <span>commands</span>
          <strong>{selectedMetrics.commands}</strong>
        </div>
        <div class="metric-card">
          <span>repetitions</span>
          <strong>{selectedMetrics.repetitions}</strong>
        </div>
        <div class="metric-card warning" class:clean={selectedMetrics.incomplete === 0}>
          <span>incomplete</span>
          <strong>{selectedMetrics.incomplete}</strong>
        </div>
      </div>

      <div class="chip-row">
        {#each selectedMetrics.actions as action}
          <span class="chip action-chip">{action}</span>
        {/each}
      </div>

      <pre>{selectedResult.output}</pre>
    </article>
  </section>

  <section class="comparison-section">
    <div class="section-heading">
      <p>Comparison</p>
      <h2>Risultati per modello sul PDF selezionato.</h2>
      <span>
        La tabella consente di leggere rapidamente differenze strutturali: numero di step,
        comandi prodotti, ripetizioni, pagine classificate come assembly e ordini incompleti.
      </span>
    </div>

    <div class="comparison-grid">
      {#each modelSummaries as item}
        <article class="comparison-card">
          <div class="comparison-top">
            <div>
              <span>{item.source}</span>
              <h3>{item.model}</h3>
            </div>
            <strong class:clean={item.metrics.incomplete === 0}>
              {item.metrics.incomplete === 0 ? 'complete' : `${item.metrics.incomplete} incomplete`}
            </strong>
          </div>

          <div class="comparison-metrics">
            <div>
              <span>assembly y/n</span>
              <strong>{item.metrics.assemblyYes} / {item.metrics.assemblyNo}</strong>
            </div>
            <div>
              <span>steps</span>
              <strong>{item.metrics.steps}</strong>
            </div>
            <div>
              <span>commands</span>
              <strong>{item.metrics.commands}</strong>
            </div>
            <div>
              <span>repetitions</span>
              <strong>{item.metrics.repetitions}</strong>
            </div>
          </div>

          <div class="objects-box">
            <span>objects</span>
            <p>{item.metrics.objects.join(', ') || 'none'}</p>
          </div>
        </article>
      {/each}
    </div>
  </section>

  <section class="architecture-section">
    <div class="arch-card">
      <h2>Validation layout</h2>

      <div class="arch-flow">
        <div>PDF</div>
        <span>→</span>
        <div>Model output</div>
        <span>→</span>
        <div>ASP facts</div>
        <span>→</span>
        <div>Structural metrics</div>
        <span>→</span>
        <div>Comparison</div>
      </div>
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
    grid-template-columns: minmax(0, 1.05fr) minmax(320px, 0.75fr);
    gap: 3rem;
    align-items: center;
    max-width: 1220px;
    margin: 0 auto;
    padding: 6rem 1.5rem 4rem;
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

  .hero-content,
  .summary-card {
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

  .summary-card {
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

  .summary-grid {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 1rem;
  }

  .summary-grid div {
    min-height: 130px;
    padding: 1rem;
    border-radius: 1.25rem;
    background: rgba(2, 6, 23, 0.52);
    border: 1px solid rgba(148, 163, 184, 0.16);
  }

  .summary-grid strong {
    display: block;
    font-size: clamp(2rem, 5vw, 3.4rem);
    letter-spacing: -0.06em;
  }

  .summary-grid span {
    display: block;
    margin-top: 0.25rem;
    color: #94a3b8;
    text-transform: uppercase;
    letter-spacing: 0.12em;
    font-size: 0.75rem;
    font-weight: 850;
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
    line-height: 1.5;
  }

  .section,
  .workspace-section,
  .comparison-section,
  .architecture-section {
    max-width: 1220px;
    margin: 0 auto;
    padding: 4rem 1.5rem;
  }

  .section-heading {
    max-width: 780px;
    margin-bottom: 2rem;
  }

  .section-heading p {
    margin: 0 0 0.6rem;
    color: #67e8f9;
    font-weight: 850;
    text-transform: uppercase;
    letter-spacing: 0.14em;
    font-size: 0.78rem;
  }

  .section-heading h2,
  .arch-card h2 {
    margin: 0;
    color: #f8fafc;
    font-size: clamp(2rem, 5vw, 3.6rem);
    line-height: 1;
    letter-spacing: -0.055em;
  }

  .section-heading span {
    display: block;
    margin-top: 1rem;
    color: #cbd5e1;
    font-size: 1.08rem;
    line-height: 1.75;
  }

  .case-grid {
    display: grid;
    grid-template-columns: repeat(3, minmax(0, 1fr));
    gap: 1rem;
  }

  .case-card {
    min-height: 190px;
    text-align: left;
    cursor: pointer;
    padding: 1.25rem;
    border-radius: 1.5rem;
    border: 1px solid rgba(148, 163, 184, 0.18);
    background: rgba(15, 23, 42, 0.72);
    color: #f8fafc;
    transition:
      transform 0.2s ease,
      background 0.2s ease,
      border-color 0.2s ease;
  }

  .case-card:hover,
  .case-card.active {
    transform: translateY(-5px);
    border-color: rgba(103, 232, 249, 0.42);
    background:
      radial-gradient(circle at top right, rgba(103, 232, 249, 0.16), transparent 8rem),
      rgba(15, 23, 42, 0.95);
  }

  .case-card span {
    display: inline-flex;
    margin-bottom: 2rem;
    padding: 0.45rem 0.65rem;
    border-radius: 999px;
    color: #020617;
    background: linear-gradient(135deg, #67e8f9, #a78bfa);
    font-size: 0.78rem;
    font-weight: 900;
  }

  .case-card strong {
    display: block;
    font-size: 1.4rem;
    letter-spacing: -0.035em;
  }

  .case-card small {
    display: block;
    margin-top: 0.65rem;
    color: #94a3b8;
    line-height: 1.55;
    font-size: 0.95rem;
  }

  .workspace-section {
    display: grid;
    grid-template-columns: minmax(320px, 0.82fr) minmax(0, 1.18fr);
    gap: 1rem;
    align-items: stretch;
  }

  .pdf-panel,
  .result-panel,
  .comparison-card,
  .arch-card {
    border: 1px solid rgba(148, 163, 184, 0.18);
    border-radius: 2rem;
    background:
      linear-gradient(180deg, rgba(15, 23, 42, 0.82), rgba(15, 23, 42, 0.58)),
      radial-gradient(circle at top left, rgba(59, 130, 246, 0.12), transparent 22rem);
    padding: 1rem;
  }

  .panel-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 1rem;
    margin-bottom: 1rem;
  }

  .panel-header span,
  .comparison-top span,
  .objects-box span {
    display: block;
    color: #94a3b8;
    font-size: 0.75rem;
    text-transform: uppercase;
    letter-spacing: 0.12em;
    font-weight: 850;
  }

  .panel-header strong {
    display: block;
    margin-top: 0.25rem;
    color: #e2e8f0;
    font-size: 1.05rem;
  }

  .panel-header a {
    color: #020617;
    background: linear-gradient(135deg, #67e8f9, #a78bfa);
    text-decoration: none;
    font-weight: 850;
    padding: 0.6rem 0.85rem;
    border-radius: 999px;
    white-space: nowrap;
  }

  .panel-header small {
    color: #94a3b8;
    font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace;
  }

  .pdf-viewer {
    width: 100%;
    height: 790px;
    border: 0;
    border-radius: 1.35rem;
    background: #f8fafc;
  }

  .model-tabs {
    display: flex;
    flex-wrap: wrap;
    gap: 0.75rem;
    margin-bottom: 1rem;
  }

  .model-tabs button {
    border: 1px solid rgba(148, 163, 184, 0.2);
    border-radius: 999px;
    color: #e2e8f0;
    background: rgba(2, 6, 23, 0.52);
    padding: 0.7rem 1rem;
    font-weight: 850;
    cursor: pointer;
  }

  .model-tabs button.active {
    color: #020617;
    background: linear-gradient(135deg, #67e8f9, #a78bfa);
    border-color: transparent;
  }

  .metric-grid {
    display: grid;
    grid-template-columns: repeat(4, minmax(0, 1fr));
    gap: 0.75rem;
    margin-bottom: 1rem;
  }

  .metric-card {
    padding: 0.9rem;
    border-radius: 1rem;
    background: rgba(2, 6, 23, 0.5);
    border: 1px solid rgba(148, 163, 184, 0.14);
  }

  .metric-card span {
    display: block;
    color: #94a3b8;
    font-size: 0.72rem;
    text-transform: uppercase;
    letter-spacing: 0.12em;
    font-weight: 850;
  }

  .metric-card strong {
    display: block;
    margin-top: 0.35rem;
    font-size: 1.55rem;
    letter-spacing: -0.04em;
  }

  .metric-card.warning strong {
    color: #fbbf24;
  }

  .metric-card.clean strong {
    color: #86efac;
  }

  .chip-row {
    display: flex;
    flex-wrap: wrap;
    gap: 0.6rem;
    margin-bottom: 1rem;
  }

  .chip {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    padding: 0.55rem 0.75rem;
    border-radius: 999px;
    font-size: 0.78rem;
    font-weight: 850;
    border: 1px solid rgba(148, 163, 184, 0.2);
  }

  .action-chip {
    color: #020617;
    background: linear-gradient(135deg, #67e8f9, #a78bfa);
  }

  pre {
    height: 520px;
    white-space: pre-wrap;
    overflow: auto;
    margin: 0;
    padding: 1rem;
    border-radius: 1rem;
    color: #bfdbfe;
    background: #020617;
    border: 1px solid rgba(148, 163, 184, 0.15);
    font-size: 0.82rem;
    line-height: 1.6;
    font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace;
  }

  .comparison-grid {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 1rem;
  }

  .comparison-card {
    padding: 1.25rem;
  }

  .comparison-top {
    display: flex;
    align-items: flex-start;
    justify-content: space-between;
    gap: 1rem;
    margin-bottom: 1.25rem;
  }

  .comparison-top h3 {
    margin: 0.35rem 0 0;
    color: #f8fafc;
    font-size: 1.55rem;
    letter-spacing: -0.045em;
  }

  .comparison-top > strong {
    white-space: nowrap;
    padding: 0.45rem 0.65rem;
    border-radius: 999px;
    color: #451a03;
    background: #fbbf24;
    font-size: 0.78rem;
    text-transform: uppercase;
    letter-spacing: 0.08em;
  }

  .comparison-top > strong.clean {
    color: #052e16;
    background: #86efac;
  }

  .comparison-metrics {
    display: grid;
    grid-template-columns: repeat(4, minmax(0, 1fr));
    gap: 0.75rem;
  }

  .comparison-metrics div {
    padding: 0.8rem;
    border-radius: 1rem;
    background: rgba(2, 6, 23, 0.5);
    border: 1px solid rgba(148, 163, 184, 0.12);
  }

  .comparison-metrics span {
    display: block;
    color: #94a3b8;
    font-size: 0.68rem;
    text-transform: uppercase;
    letter-spacing: 0.1em;
    font-weight: 850;
  }

  .comparison-metrics strong {
    display: block;
    margin-top: 0.35rem;
    color: #f8fafc;
    font-size: 1.25rem;
  }

  .objects-box {
    margin-top: 1rem;
    padding: 1rem;
    border-radius: 1rem;
    background: rgba(2, 6, 23, 0.42);
    border: 1px solid rgba(148, 163, 184, 0.12);
  }

  .objects-box p {
    margin: 0.55rem 0 0;
    color: #cbd5e1;
    line-height: 1.6;
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
    margin-top: 2rem;
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
    .workspace-section,
    .case-grid,
    .comparison-grid {
      grid-template-columns: 1fr;
    }

    .pdf-viewer {
      height: 620px;
    }
  }

  @media (max-width: 720px) {
    .hero {
      padding-top: 3.5rem;
      gap: 2rem;
    }

    .summary-grid,
    .metric-grid,
    .comparison-metrics {
      grid-template-columns: repeat(2, minmax(0, 1fr));
    }

    .panel-header,
    .comparison-top {
      flex-direction: column;
      align-items: flex-start;
    }

    .pdf-viewer,
    pre {
      height: 420px;
    }

    .arch-flow {
      flex-direction: column;
    }

    .arch-flow span {
      transform: rotate(90deg);
    }
  }
</style>
