# SafeShare

**SafeShare** is a project aiming to detect and redact sensitive information in real time before it is transmitted during screen sharing.

## Problem
Screen sharing can unintentionally expose passwords, API keys, personal information, and confidential data.

## Solution
SafeShare aims to analyze screen content using OCR, detection rules, and machine learning where justified, then mask sensitive regions before encoding and transmitting video frames.

## Initial Prototype Scope

- **Categories:** Common, fully visible email addresses and GitHub classic personal access tokens using the `ghp_` format. Exact matching rules will be defined during detection work; the prefix alone is not a complete rule.
- **Input:** Synthetic PNG screenshots of a terminal, code editor, and simple browser form, using invented data only.
- **Conditions:** Clear, horizontal text around 16–24 px, with each sensitive value fully visible on one line; light and dark backgrounds; 1280×720 and 1920×1080 images.
- **Expected redaction:** Hide every character of each supported sensitive value with an opaque mask while preserving nearby labels and ordinary text. Clean samples should remain unmasked during successful processing.

Arbitrary passwords, other token formats, cropped or wrapped values, difficult-to-read text, multilingual layouts, and live or rapidly changing content are outside the first evaluation.

Phase 1 will prepare, label, and display samples. Detection and redaction begin in Phase 2. The samples will establish a controlled development baseline; broader claims require independent evaluation.

## Engineering Focus

Investigate a C++ pipeline that controls which frames reach encoding and transmission, withholding frames when processing fails or is incomplete. Measure detector misses separately: successful processing does not guarantee that all sensitive content was found.

Later evaluation will inspect received video for leaked sensitive content and measure latency, memory use, throughput, and failure behavior. These are planned contributions to implement and validate; no accuracy or performance results are available yet.

## Research References

- [PrivyShield](https://github.com/aditrynacode/PrivyShield): Reference for combining screen capture, OCR, sensitive-text detection, and a blur overlay. Protection of outgoing video frames remains unverified in our review.
- [PaddleOCR](https://www.paddleocr.ai/latest/en/version3.x/pipeline_usage/OCR.html): Candidate for extracting text and its image coordinates.
- [Presidio](https://presidio.dataprivacystack.org/analyzer/): Reference for sensitive-text recognition; its [Image Redactor](https://presidio.dataprivacystack.org/image-redactor/) also combines OCR, detection, and masking.

These are documentation-based references, not selected dependencies or tools benchmarked for SafeShare. OCR-based redaction already exists; the project makes no claim that the idea is new.

## Tech Stack (Planned)

- **Languages:** C++20, Python
- **AI & Computer Vision:** PyTorch, OpenCV, OCR
- **Systems:** Linux, WebRTC, Multithreading
- **Build & Development:** CMake, Ninja, Git

## Development Roadmap

1. Establish scope and a runnable foundation: research, a minimal C++20 build, labeled synthetic samples, and a frame viewer.
2. Build baseline detection and redaction with OCR, simple rules, opaque masks, and processing-failure handling.
3. Deliver the first end-to-end stream using controlled video input and WebRTC, with checks at the receiver.
4. Integrate live screen capture after deciding the capture platform and Windows/WSL boundary.
5. Strengthen runtime reliability with measured concurrency, bounded queues, reusable buffers, and failure tests.
6. Improve detection with justified AI, comparing any trained or fine-tuned model against the baseline.
7. Measure and optimize the complete system using reproducible accuracy, privacy, and runtime evaluations.

## Status
Phase 1 is in progress. Task 1 (research and initial scope definition) is complete. The minimal build, synthetic samples, and frame viewer remain to be implemented.

*Architecture, implementation details, and measured results will be added as development progresses.*
