# SafeShare — Project Context and Goals

This document preserves the project's intended outcome, learning goals, constraints, and success criteria. The capabilities and CV achievements below are targets, not claims of completed work. See [README.md](README.md) for the project overview, [PLAN.md](PLAN.md) for the approved development roadmap, and [AGENTS.md](AGENTS.md) for the mentoring rules.

## Project Vision

**SafeShare — Real-Time AI Privacy Protection for Screen Sharing**

During screen sharing, people can accidentally expose passwords, API keys, personal information, confidential documents, and other sensitive data. SafeShare aims to detect and automatically redact sensitive information before video frames are transmitted to other participants.

Target users include developers, remote workers, technical support teams, and organizations handling sensitive information.

The goal is a functioning, technically substantial application that solves a recognizable real-world problem. Investigate existing solutions and identify meaningful engineering improvements; do not assume the idea itself is new.

## Intended Functionality

- Capture or receive screen video in real time.
- Detect sensitive information using computer vision, OCR, and machine learning where appropriate.
- Automatically redact sensitive regions before transmission.
- Stream sanitized video to another participant.
- Handle processing delays, disconnections, and failures without knowingly transmitting unverified frames.
- Maintain low latency and efficient memory usage.
- Provide measurable evidence of detection accuracy, privacy protection, and runtime performance.

Successful processing does not prove that every sensitive region was detected. Evaluate detector misses separately from pipeline failures that allow unprocessed frames through.

## Career and Learning Goals

The primary goal is to build a strong project for my CV and improve my chances of securing software engineering positions, particularly early-career, student, C++, systems, and AI engineering roles.

I want to personally implement and understand substantial parts of the system. The project should demonstrate:

| Area | Intended engineering experience |
|---|---|
| C++20 | Modern C++, OOP, multithreading, concurrency, and software architecture. |
| Linux | System-level development, debugging, profiling, and process management. |
| Networking | Real-time video streaming, WebRTC, asynchronous communication, and connection reliability. |
| Memory management | Explicit resource ownership, buffer management, memory efficiency, and optimization. |
| Python | Data preparation, experimentation, AI development, and evaluation. |
| PyTorch and deep learning | Training or fine-tuning models for sensitive-information detection where justified. |
| Computer vision | OCR, image processing, region detection, and redaction. |
| Testing and performance engineering | Correctness, reproducible benchmarks, latency measurements, memory profiling, and failure testing. |

Technologies should play meaningful roles rather than being included merely as keywords. Established libraries are encouraged where appropriate, but distinguish their functionality from my own engineering contributions. Avoid a project that only assembles existing libraries without substantial engineering of my own.

CUDA is optional. Introduce it only if a demonstrated need justifies it.

## Target CV Presentation

The following is an aspirational presentation, not a record of completed achievements:

**SafeShare — Real-Time AI Privacy Protection for Screen Sharing**

*Target technologies: C++, Python, PyTorch, OpenCV, Linux, WebRTC, Networking, Multithreading*

- Built a real-time screen-sharing system that detects and hides sensitive information before video transmission.
- Engineered a fail-closed C++/Linux WebRTC pipeline with multithreading, bounded queues, and reusable frame buffers.
- Fine-tuned a PyTorch vision model; measured detection recall, leaked frames, and p95 added latency.

The actual CV must reflect only what I genuinely implement and measure. Revise these statements to match the eventual implementation, including whether model training or fine-tuning proves justified.

## Evidence of Success

The project should eventually report:

- Detection recall and false-positive rates.
- Number of sensitive frames leaked during controlled tests, measured at the receiver.
- End-to-end latency and p95 added processing latency.
- Memory consumption, frame throughput, and dropped frames.
- Behavior during processing failures and network interruptions.
- Performance improvements supported by credible, reproducible baselines.

Do not invent results or claim guaranteed protection against every possible sensitive-information leak. Report the tested scope and limitations alongside measurements.

## Intended Development Environment

- **IDE:** VS Code.
- **Development OS:** WSL2 Ubuntu.
- **Main languages:** C++20 and Python.
- **AI framework:** PyTorch.
- **Development assistant:** Codex.

Decide the eventual runtime platform and screen-capture integration at the appropriate stage. Windows-native capture may be necessary for capturing a Windows desktop. Explore other tools, libraries, frameworks, configurations, and architectural decisions when they become relevant.

## Working Approach

I develop the project myself, with Codex acting as a software engineering mentor. Follow the rules in [AGENTS.md](AGENTS.md): explanations and hints before complete solutions, no file changes or implementation without an explicit request, and no automatic progression to the next stage.

Keep development practical, incremental, testable, and focused on one stage at a time. Prioritize a working end-to-end prototype before advanced features and optimizations. Discuss technical details immediately before their implementation rather than deciding the entire architecture upfront.

Avoid unnecessary documentation, abstractions, dependencies, and complexity. Use relevant skills such as brainstorming, writing-plans, test-driven-development, systematic-debugging, and verification-before-completion when appropriate, while respecting the mentoring rules and my explicit instructions.
