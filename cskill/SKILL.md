---
name: cskill
description: "Summarize C language development practices, review C code risks, and produce actionable checklists for C projects. Use when working on `.c` or `.h` files, Makefile/CMake-based C builds, memory or pointer bugs, portability issues, undefined behavior, or when the user asks for C development notes, review points, or best practices."
---

# C Skill

## Overview

Use this skill to reason about C code with a strict safety and correctness lens.
Default to `C17` for new C projects unless the user explicitly needs `C23` and the toolchain is known to support it well.
Start with language standard, build and warning settings, then check memory, strings, integers, UB, error paths, portability, and tests.

## Working Method

When reviewing or summarizing C work, follow this order:

1. Confirm target standard, compiler, platform, and build system.
2. Check warning level and debug tooling first.
3. Audit memory ownership, bounds, lifetime, and cleanup paths.
4. Audit integer conversions, pointer use, and undefined behavior.
5. Check headers, API contracts, globals, and portability assumptions.
6. Finish with tests, sanitizers, and reproducible failure cases.

## Output Shape

Prefer short, concrete output:

- If the user asks for a summary, group points by topic and keep each point actionable.
- If the user asks for review, list findings by severity and point to the exact file and line.
- If the user asks for fixes, state the bug, the risk, and the smallest safe change.

## Core Rules

Treat warnings as defects until proven otherwise.

Recommend `C17` by default. Move to `C23` only when the project controls its compiler baseline and actually benefits from newer features.

Assume manual memory management is the main risk unless the code is trivially simple.

Prefer `size_t` for sizes and counts, fixed-width integer types for external data, and `const` whenever mutation is not required.

Prefer `snprintf`, explicit length checks, and single-exit cleanup blocks over ad hoc error handling.

Do not assume signed overflow, pointer provenance, struct packing, endianness, or null termination.

Avoid macro tricks when a function, enum, or `static inline` helper is clearer.

## Reference

Read [checklist.md](references/checklist.md) when you need the full review checklist or a structured summary of C development concerns.
Read [cmake.md](references/cmake.md) when the project uses CMake and you need a short build-system checklist.
Read [memory.md](references/memory.md) when the question is only about memory safety, ownership, leaks, or invalid access.

## Common Triggers

Use this skill for requests like:

- "总结一下 C 语言开发有哪些注意事项"
- "帮我 review 这段 C 代码"
- "这个段错误/内存泄漏可能在哪"
- "这个头文件设计有没有问题"
- "这段 C 代码有哪些未定义行为风险"
- "给我一份 C 项目自查清单"
- "CMake 有哪些注意事项"
- "C 语言内存方面有哪些注意事项"
