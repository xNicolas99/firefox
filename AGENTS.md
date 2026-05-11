# Repository Knowledge Base (Jules Context)

## 🏗️ Architecture & Stack
- **Core Stack:** Rust, Python, JavaScript/Node.js, C++
- **Architecture Pattern:** Multi-process components (browser, dom, layout, gfx, js, netwerk)
- **Key Dependencies:**
  - **JavaScript/Node.js**: ESLint, Prettier, Stylelint, TypeScript (from `package.json`)
  - **Rust**: Various internal crates managed via `Cargo.toml`
  - **Python**: Ruff (from `pyproject.toml`)

## 🗺️ Project Map
- `/browser` -> Browser core components and app
- `/dom` -> Document Object Model implementation
- `/gfx` -> Graphics subsystem
- `/js` -> SpiderMonkey JavaScript engine
- `/layout` -> Layout and rendering engine
- `/mobile` -> Mobile specific components
- `/netwerk` -> Necko networking library
- `/services` -> Application services
- `/servo` -> Servo components
- `/testing` -> Testing infrastructure and frameworks
- `/toolkit` -> Core toolkit and shared components

## 📜 Coding Conventions & Rules
- **Comments:** Limit the amount of comments you put in the code to a strict minimum. You should almost never add comments, except sometimes on non-trivial code, function definitions if the arguments aren't self-explanatory, and class definitions and their members. Do not remove existing comments unless they are directly related to what you are changing.
- **Styling:** Our style guide forbids the use of emoji.
- **Workflow & Reviews:**
  - Never submit patches to Phabricator without explicit user approval.
  - In commit messages, group reviewers use a `#` prefix: `r?#group-name` (e.g. `r?#linter-reviewers`), while individual reviewers do not: `r?username`
  - Never put `DONTBUILD` (or `CLOSED TREE`) in the `-m` message of `mach try fuzzy` / `mach try compare` when you want builds to actually run. The Gecko decision task scans the message and on `DONTBUILD` strips every task from the graph.
- **Fixing review comments:** Use `@moz:phabricator://revision/D{revision_id}` to retrieve the revision and its comments. You can find the review identifier by inspecting the commit log with `jj log -T builtin_log_detailed` if using `jj` or `git log -v -l 10` if using git.

## 🧪 Testing & Commands
- **Test Runner & Environment Commands:**
  - `./mach format`: Format code. Run it without additional parameters to format all the files you have modified.
  - `./mach build`: Build the project. Full builds can take a long time, up to tens of minutes.
  - `./mach build faster`: Do not run this when only front-end test files (JS, HTML, etc.) were modified — they don't need compilation. When doing Android and Desktop front-end-only changes, use `./mach build faster` to skip all C++/Rust compilation.
  - `./mach build binaries`: Skip all front-end-related tasks for C++/Rust only changes.
  - `./mach test --auto`: Run tests. Ask if you should run a test. If you do, you probably want to run the test with `--headless`.
  - `mach try auto`: Run tests in CI once you are satisfied with the tests you run locally.
  - `./mach run`: Run the project.
  - `./mach doc --no-serve --no-open`: Build the documentation.
  - `./mach python --virtualenv <virtualenv_name>`: Execute Python of a Mach command's virtualenv.
- **Tooling:**
  - Use `searchfox-cli` if you want to know about something. `--define` for function/class defs, `--path` for restricting by path, `--id` for identifiers, `--regexp` for regex. Prefer searching for identifiers with `searchfox-cli` for C++, Rust and Java. Do not use identifier search for front-end identifiers. Only use `rg` or usual local tools if you need to find information about something that has definitely changed locally.
  - `treeherder-cli`: Pull CI results for a try push.
  - `@moz:bugzilla://bug/{bug_id}`: Retrieve a bug.
  - `@moz:phabricator://revision/D{revision_id}`: Retrieve a Phabricator revision.
- **Important workflow detail:** When running slow commands like `./mach test`, NEVER pipe their output through `tail`, `grep`, `head`, or other filters. Instead redirect output to a temporary file in `artifacts/` and selectively read this file.
