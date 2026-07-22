# Area Codes Home And Modes Requirements

Date: 2026-07-17

## Original Prompt

Review the below requirements. Spawn a subagent to explore the codebase and come up with questions to ask me to fill in holes or to make pivotal decisions regarding the requirements. Once that subagent is finished, use the question tool repeatedly until I've answered all questions. Once all questions have been exhausted, save my answers, and this prompt, to docs/prompts and if you need to compact tell the next agent to read that file after compaction.

Then, implement the requirements (and answered questions) using subagents:
Spawn multiple subagents to implement different disjoint areas of the requirements, when appropriate.
They should be instructed to read the file you created in docs/prompts containing the requirements and answered questions, but they should only complete a given subset of the requirements.
Instruct them not to run any tests or compile any code or ask any questions, they should just write code.
After all subagents are done writing code, spawn another subagent to audit the codebase against every individual requirement.
It should be instructed to read the file you created in docs/prompts containing the requirements and answered questions.
It should be instructed to return a table with three columns: The requirement, whether or not it's satisfied, and what changes need to be made (if any).
Use the returned table of the audit agent to spawn another swarm of subagents to fix any issues. They must also be instructed not to run tests or compile code. Separate tasks across multiple agents where appropriate.
Continue auditing, fixing, auditing, fixing until only a few small issues remain from the audit.
Once there are only a few small issues in the audit, spawn a final subagent to compile and test the code. It should fix any compilation errors or failed tests. If e2e tests are requested below, this subagent should also perform those.

Game Overview
Add a landing/home screen for the game.
Navigation
Include a title at the top which says “Area Codes” and has an arrow on the right side pointing to the right. When clicked the title quickly flies to the left and fades out and another title called “States” flies in and fades in from the right. When this second title is visible there should be an arrow on the left side which works the same the other way. These two titles are for different games. The following is an outline of the Area Code game. The State game page will just be a place holder for now.

Area Code Game
Mode Selection: Include two big buttons side by side for two modes: Training and challenge. These buttons will take you to another page for each.
Training Mode
Difficulty: Include a section for difficulty with buttons for easy, medium, hard, and custom (ordered left to right), which illuminate when selected.
Settings Toggles: Include a vertical list with toggles for:
Area code borders
Unique colors
Reveal found codes
Configuration Logic:
Easy: Settings 1, 2, and 3 are enabled.
Medium: Settings 1 and 3 are enabled.
Hard: Settings 1, 2, and 3 are disabled.
Custom: Automatically selected/illuminated if toggles are manually altered to a configuration that does not match easy, medium, or hard.
Behavior: When "Reveal found codes" is turned off, area code numbers and borders/regions remain hidden, even after they are correctly guessed.
Challenge Mode
Disabled Features: Borders, colors, and revealed areas are disabled and unavailable.
Time Limit: Introduce a customizable time limit via a slider (varying pseudo-continuously from 1 second to infinity, with 1 minute approximately in the middle).
Scoring System:
Guesses inside the area code receive the max score of 5000.
Guesses outside the area code are scored based on the distance from the correct area.
Guesses across the country result in 0 points.

## Clarifying Answers

- Navigation/routing style: In-page views only, no URL changes.
- Area Codes landing/mode selection behavior: Clicking Area Codes enters a separate mode-selection page.
- States placeholder: Show a Brazil states map but no gameplay.
- Training difficulty presets/visibility: Exactly as written. Easy enables area code borders, unique colors, and reveal found codes. Medium enables area code borders and reveal found codes, with unique colors disabled. Hard disables all three.
- Reveal found codes off feedback: Correct guesses may briefly flash only the clicked area. Wrong guesses should not reveal the target.
- Challenge scoring falloff: GeoGuessr-like. 5000 points inside the correct area; exponential falloff by distance; very far guesses approach 0.
- Challenge timer infinity: The slider's highest position means infinity / untimed.
- Maps dependency: Keep Google Maps.

## Codebase Exploration Notes

- The app is currently a single static file: `brazil_ddd_atlas_google_template.html`.
- CSS and JavaScript are inline in that file.
- GitHub Pages deploy copies that file to `public/index.html`.
- No package manager, build tooling, or automated tests were found.
- Current gameplay starts directly in the area-code map.
- Current map uses Google Maps JavaScript API and remote GeoJSON for Brazil DDD polygons and country/state boundaries.
- Current UI includes toggles for unique colors, area code borders, state borders, and reveal all DDDs.
- Current label visibility reveals labels when reveal-all is checked or a code is solved; this must change for the new reveal-found setting.

## Instructions If Context Is Compacted

Read this file before continuing implementation. Treat the Original Prompt plus Clarifying Answers as the source of truth.
