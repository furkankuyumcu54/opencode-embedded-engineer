# Embedded Engineer Skill

[![skills.sh](https://skills.sh/b/furkankuyumcu54/opencode-embedded-engineer)](https://skills.sh/b/furkankuyumcu54/opencode-embedded-engineer)

Version: 1.0.0
Status: Stable
Last Reviewed: 2026-06

## Purpose

Production-oriented embedded firmware engineering skill focused on:

- STM32
- ARM Cortex-M
- HAL / LL
- DMA
- Interrupts
- FreeRTOS
- Driver architecture
- Firmware code review
- Production readiness
- Failure-mode analysis

## Design Principles

1. Correctness over cleverness
2. Measured evidence over assumptions
3. Maintainability over short-term convenience
4. Consistency over personal preference
5. Incremental improvement over large rewrites

## Version History

### v1.0.0

Initial stable release.

Major features:

- Self-review and redundancy control
- Production-readiness checklist
- Embedded-specific code review framework
- DMA / interrupt / polling decision guidance
- RTOS synchronization guidance
- Race-condition analysis requirements
- Architecture consistency enforcement
- Evidence-driven debugging workflow
- Anti-pattern detection
- Explicit tradeoff documentation

## Installation

### Via skills.sh

```sh
npx skills add furkankuyumcu54/opencode-embedded-engineer
```

### Manual

```sh
mkdir -p ~/.config/opencode/skills
cp -r embedded-engineer ~/.config/opencode/skills/
```

Restart OpenCode. The skill activates automatically when the conversation involves STM32, Cortex-M, RTOS, DMA, or driver development.

## Engineering Philosophy

This skill was built from a single premise: an AI assistant for embedded development should behave like a senior engineer reviewing your code, not like a textbook or a code generator.

Every rule in the skill was written to prevent a real failure mode seen in production firmware:

- ARM Cortex-M memory model rules prevent compiler-optimized race conditions
- CPU budget calculations prevent over-recommending DMA when interrupts suffice
- Cortex-M variant-specific atomicity guarantees prevent under- or over-using critical sections
- FreeRTOS priority inheritance footnotes prevent false assumptions about RTOS behavior
- The hypothesis-first debugging protocol prevents shotgun-debugging

The skill underwent a technical review that identified and corrected 13 issues before release, including inaccurate Cortex-M0 atomicity claims, exaggerated stack overflow examples, and overly restrictive priority assignment rules.

## Key Technical Decisions

| Decision | Why |
|----------|-----|
| CPU budget formula for DMA (`ISR_cycles * freq / CPU_speed`) | Absolute frequency thresholds don't port across Cortex-M0 to M7 |
| Hardware atomicity tables per Cortex-M variant | M0 guarantees differ from M3/M4/M7; one-size-fits-all rules cause false positives |
| RTOS priority inheritance footnoted as config-dependent | FreeRTOS disables it by default on some ports for code size |
| `volatile` + atomicity split in code review | `volatile` prevents compiler caching but does not guarantee hardware atomicity |
| Bounded busy-waiting allowed with timeout | Absolute "no busy loops" rule would flag legitimate register sync and microsecond-delay patterns |
| Production readiness includes power + determinism + WCET | DMA enables deep sleep; heap after scheduler start breaks determinism; cache misses affect timing |

## Who it's for

Embedded engineers writing production firmware for ARM Cortex-M microcontrollers. Assumes comfort with C and electronics — no beginner explanations. Particularly relevant for STM32 developers working with HAL, LL, FreeRTOS, DMA, and peripheral drivers.

## Sections

| # | Section | Purpose |
|---|---------|---------|
| 1 | Role | Senior mentor persona |
| 2 | User Profile | Calibrates explanation depth |
| 3 | Self Review & Redundancy Control | Code reuse discipline, token efficiency, context awareness |
| 4 | Behavioral Rules | 11 directives that override default model behavior |
| 5 | Decision Frameworks | Poll/Int/DMA, HAL/LL/Direct, RTOS priority, Mutex/Sem/Queue |
| 6 | Code Review Prompts | Systematic NVIC/DMA/stack/volatile/error-propagation checks |
| 7 | Teaching Interjections | Compulsory explanations of peripheral architecture, register behavior, DMA flow, interrupt dispatch |
| 8 | Anti-Pattern Triggers | 12 patterns flagged with specific failure-mode explanations |
| 9 | Production Readiness | Gate checklist with power, determinism, and WCET coverage |
| 10 | Debugging Mindset | Hypothesis-first protocol |
| 11 | Project Memory | Infer and follow existing project conventions |

## Technical Review

This skill was reviewed for technical accuracy by a senior embedded firmware architect. All frequency thresholds, atomicity guarantees, and RTOS behavior claims were verified against ARM architecture reference manuals and FreeRTOS documentation.


