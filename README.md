# Physics Quiz Generator

Makes a unique paper quiz or exam for every student (different questions and numbers), in seating-chart order, with printed answer keys and a grading key for AI-assisted grading.

## Running it

- **On your computer:** open `index.html` in Chrome, Edge, Firefox or Safari. No internet connection or installation is needed.
- **On GitHub Pages:** push this folder to a repository and turn on Pages. Rosters are read in the browser and are never uploaded, so don't commit roster files.
- **As one file:** run `python3 tools/build.py` to create `dist/quiz-generator.html`, a single file you can email or put on a USB drive.

## Folder layout

| Path | What it is |
|---|---|
| `index.html`, `styles.css` | The page and its styles |
| `engine/core.js` | Registry, course settings, random numbers, number formatting, part types |
| `engine/figures.js` | Reusable drawings: people, boxes, vehicles, graphs, QR codes |
| `engine/app.js` | Roster, blueprints, quiz assembly and printing, answer keys, preview |
| `course.js` | Course-wide settings (g, sig figs, tolerance, page budget) and saved blueprints |
| `units/units.js` | **The list of units to load**, in course order |
| `units/unit-07-friction.js` | Unit 7 question bank (11 groups) |
| `units/_unit-template.js` | Starting point for a new unit (not loaded) |
| `blueprints/` | Saved quiz and exam layouts (JSON) |
| `test.html`, `tools/test-runner.js` | Automatic checks for every question group |
| `tools/build.py` | Bundles everything into one HTML file |
| `grading-instructions.md` | Instructions for grading scans with Claude |

## Blueprints: quizzes, midterms and finals

A blueprint is a saved layout: which question groups, how many from each, the title, page budget, tolerance and work space. A unit quiz draws from one unit; a midterm or final is just a blueprint that draws from several units.

- **Save** in the app downloads the current layout as a `.json` file. **Load…** opens one.
- To put a blueprint in the "Start from a saved blueprint" menu, paste it into `presets` in `course.js`.
- The blueprint panel shows the total points and estimated minutes as you choose groups.

## Adding a unit

1. Copy `units/_unit-template.js` to `units/unit-NN-topic.js` (for example `unit-03-forces.js`).
2. Set the unit's `id` (`U3`), `number`, `title` and `version`, and write its groups. `unit-07-friction.js` has complete examples of every part type.
3. Add the file name to `units/units.js`.
4. Open `test.html`. Fix anything marked **Fail**, and review anything marked **Check**.

Group IDs only need to be unique within a unit; the app combines them with the unit ID (`U3-A1`, `U7-A1`).

## Versions: keeping old answer keys valid

A student's numbers come from the Quiz ID, the student's name, and the group ID. If a group's generator changes, regenerating an old quiz gives different numbers. So:

- **Keep the downloaded grading-key JSON with your scans.** It is the permanent record of what each student saw.
- **Bump a group's `version`** whenever you change what students see or the correct answers. The version is recorded in every grading key and saved blueprint, and loading an old blueprint warns you about groups that have changed.
- Use Git tags (for example `2026-fall-unit7`) so any past quiz can be reproduced exactly.

## Course rules for question writers

These are in `course.js` and are checked where possible by `test.html`:

- Equations go on the separate reference sheet, not in question text, unless the teacher asks.
- Given values and required answers use the course's significant figures (3).
- Every numeric part has a work point (equation and substitution) and an answer point (value, sig figs, units).

## Printing

Print with margins set to Default, scale 100%, two-sided. Each quiz takes an even number of pages, so students never share a sheet.
