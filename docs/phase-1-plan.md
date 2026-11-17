# Phase 1 — Scope and Runnable Foundation

**Status:** Planned. The checkboxes below represent future work.

**Goal:** Build a small C++20 program that loads and displays synthetic test frames, supported by a clear initial scope and labeled examples for later detection work.

**References:** [Main roadmap](../PLAN.md), [project goals](../PROJECT_CONTEXT.md), [project overview](../README.md), and [mentoring rules](../AGENTS.md).

The user implements the project with mentoring. Work through the tasks in order, discussing the relevant concepts and decisions before each task. Review and verify each result before moving on at the user's request. This plan does not authorize implementation.

## Scope

Phase 1 covers initial research, scope definition, a minimal build, synthetic samples, and a local frame viewer. Detection, OCR, redaction, networking, live desktop capture, model training, concurrency, and optimization belong to later phases.

Use saved still images as the initial input. They make failures reproducible and provide a foundation for the detection baseline. Phase 1 displays the original synthetic images; it provides no privacy protection yet.

## Proposed Files

These are suggested destinations for the implementation tasks, not files to create all at once.

| Path | Responsibility | Task |
|---|---|---|
| `README.md` | Brief initial scope, research links, and instructions for building and running the program. | 1, 2, 4 |
| `CMakeLists.txt` | Minimal C++20 build for one executable. | 2, 4 |
| `.gitignore` | Exclude generated build output and local environment artifacts. | 2 |
| `src/main.cpp` | Start with a runnable entry point, then add input handling and frame preview. | 2, 4 |
| `samples/phase-1/` | Small synthetic images with and without sensitive-looking content. | 3 |
| `samples/phase-1/annotations.json` | Image dimensions and labeled sensitive regions for future evaluation. | 3 |

Keep the initial structure small. Introduce additional files, classes, or a test framework only when the implementation has a clear need for them. The viewer does not need to parse annotations in this phase.

## Task 1 — Define Initial Scope and Review Existing Solutions

**Objective:** Decide what the first prototype should handle and what engineering contribution is worth pursuing.

**Steps:**

- [ ] Review two or three relevant existing tools or approaches using their documentation or repositories. Record source links and distinguish documented capabilities from assumptions.
- [ ] Compare how they detect sensitive content, apply redaction, and integrate with screen sharing. Identify limitations relevant to SafeShare without claiming that its idea is new.
- [ ] Choose two or three initial sensitive-data categories. Candidates include email addresses and recognizable API-key formats; these are examples to discuss, not selected categories.
- [ ] Define the initial test conditions, such as readable text, a small set of layouts, and saved images. Record important exclusions so the prototype's scope is explicit.
- [ ] Explain what would count as correct detection and masking in later phases, including an example that should remain visible.
- [ ] Add a concise scope summary and useful research links to `README.md`. Identify at least one engineering contribution to investigate, such as failure handling or measured latency.

**Done when:** You can explain the initial categories, test conditions, limitations, and intended contribution. The scope is small enough to exercise with synthetic samples and makes no unmeasured accuracy or performance claims.

**Skills:** Requirements analysis, technical research, scope management, and evaluation design.

## Task 2 — Establish a Minimal C++20 Build

**Depends on:** Task 1.

**Objective:** Make a small program that you can build and run reliably in WSL2 Ubuntu.

**Steps:**

- [ ] Check the available C++ compiler, CMake, Ninja, and Git. Identify missing requirements before installing anything.
- [ ] Discuss how source files, compilation, linking, and the build directory fit together.
- [ ] Write a minimal `CMakeLists.txt` that requires C++20 and builds one executable from `src/main.cpp`. Use `safeshare-preview` as the proposed executable name.
- [ ] Implement a minimal entry point that prints a short startup message and exits successfully.
- [ ] Keep generated files in `build/` and exclude that directory through `.gitignore`.
- [ ] Configure, build, and run the program using the commands below. Add the working instructions to `README.md`.
- [ ] Confirm that the same source configures and builds in a second, empty build directory, without relying on generated files from the first build.

**Proposed verification commands, to run during implementation:**

```bash
cmake -S . -B build -G Ninja
cmake --build build
./build/safeshare-preview
```

**Done when:** Configuration and compilation succeed, the executable prints the expected message and returns success, and the build can be reproduced in an empty build directory. No image-processing dependency is needed for this task.

**Skills:** C++20 project setup, compilation and linking, CMake, Linux tooling, and Git hygiene.

## Task 3 — Prepare Synthetic Test Frames and Labels

**Depends on:** Task 1; complete after Task 2 in the initial workflow.

**Objective:** Create a small, understandable set of examples for the viewer and later detection evaluation.

**Steps:**

- [ ] Create a few saved PNG images representing familiar screen content, such as a terminal, editor, or form. Use invented values only.
- [ ] Include examples for each selected category, a frame with multiple sensitive regions, and clean frames with no sensitive content. Include ordinary text that resembles a selected category but should remain visible.
- [ ] Include at least two image sizes so the viewer is not exercised only with one fixed resolution.
- [ ] Record each image's filename, pixel width and height, and list of sensitive regions in `annotations.json`.
- [ ] For each region, record its category and bounding box as `x`, `y`, `width`, and `height`, using pixels measured from the image's top-left corner. Represent a clean frame with an empty region list.
- [ ] Inspect every label: it should cover the intended sensitive content, have positive dimensions, and stay within the image boundaries. Confirm that recorded image dimensions match the files.

**Done when:** Every chosen category has a synthetic example, clean and mixed cases are present, and every image has inspected annotations. The samples are development fixtures; they are not evidence of real-world accuracy or a held-out evaluation dataset.

**Skills:** Test-data design, image coordinates, annotation consistency, and preparation for later evaluation.

## Task 4 — Load and Display Test Frames

**Depends on:** Tasks 2 and 3.

**Objective:** Turn the minimal executable into a local frame viewer.

**Steps:**

- [ ] Discuss the image-loading and display approach. Evaluate the already-planned OpenCV first and confirm that a local preview window can open in the development environment before building around it.
- [ ] Define the acceptance cases below before implementation. Decide which behavior checks are useful to automate and which require visual inspection.
- [ ] Accept one image path as a command-line argument and decode the image using the selected library.
- [ ] Display the image, report its decoded dimensions, and provide a clear way to close the viewer.
- [ ] Handle missing arguments, nonexistent paths, and unreadable or corrupt images with an understandable error and a nonzero exit status.
- [ ] Handle display initialization failure visibly, with a nonzero exit status. Investigate any WSL2 display issue without expanding this task into desktop capture.
- [ ] Run the acceptance checks and update `README.md` with the dependency setup, invocation, and exit behavior actually verified.

**Proposed invocation:** `./build/safeshare-preview samples/phase-1/<image-name>.png`.

| Acceptance case | Expected result |
|---|---|
| Each valid sample, including both image sizes | Correct image appears, reported dimensions match the source, and the image has the expected orientation and colors. |
| Close the preview normally | Program exits successfully and releases its resources. |
| No argument | Usage guidance and a nonzero exit status. |
| Nonexistent path or corrupt image | Clear error, nonzero exit status, and no attempt to display invalid image data. |
| A valid image whose path contains spaces | Image loads when the path is passed as one quoted argument. |
| Display unavailable | Clear failure and a nonzero exit status instead of reporting a successful preview. |

**Done when:** The viewer passes the applicable acceptance checks, displays all synthetic samples correctly, and has reproducible build and run instructions. Record any environment-dependent check that could not be exercised rather than marking it passed.

**Skills:** C++ input handling, image representation, library integration, resource lifetime, debugging, and behavior verification.

## Suggested Branching Workflow

For sequential solo work, create a short-lived task branch from the updated `main`, review and verify the task, then merge it before starting the next branch. Suggested names are `docs/initial-scope`, `chore/cpp-build`, `test/synthetic-frames`, and `feat/frame-preview`.

If you choose the previously discussed `phase-1` integration branch instead, create and merge task branches against it, then verify the combined phase before merging into `main`. The task boundaries and completion criteria stay the same. Choose the workflow before beginning implementation.

## Phase 1 Completion Checklist

- [ ] The initial scope and relevant existing approaches are recorded concisely.
- [ ] The C++20 project builds reproducibly in the chosen development environment.
- [ ] Synthetic frames and inspected labels cover the selected categories and clean cases.
- [ ] The program loads and displays those frames and handles the specified input failures.
- [ ] Build and run instructions match commands actually verified.
- [ ] The user can explain the build process, the image's dimensions and coordinates, and the viewer's basic resource lifetime.
- [ ] Remaining limitations and unverified checks are explicit.

Phase 2 begins only when the user requests it. Its next focus is baseline detection and redaction using the foundation built here.
