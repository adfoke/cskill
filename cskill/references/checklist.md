# C Development Checklist

## 1. Build and Tooling

- Pin the language standard explicitly.
- Default to `-std=c17` for new projects unless there is a clear reason to require `C23`.
- Move to `-std=c23` only when the project compiler matrix is verified and the new features are worth the compatibility cost.
- Enable strict warnings such as `-Wall -Wextra -Wpedantic`; use `-Werror` in CI when practical.
- Keep separate debug and release profiles.
- In debug builds, enable `-g`, `-fsanitize=address,undefined`, and stack protection if available.
- Add static analysis where possible, such as `clang-tidy` or compiler analyzers.

## 2. Headers and Module Boundaries

- Put declarations in headers and definitions in `.c` files unless `static inline` is clearly justified.
- Use include guards or `#pragma once`.
- Keep headers minimal; avoid leaking private dependencies.
- Make ownership, mutability, and thread-safety visible in function signatures and comments.
- Avoid writable global state unless there is a strong reason.

## 3. Memory Management

- Define ownership for every allocated object: who allocates, who frees, and when.
- Check allocation size calculations for overflow before `malloc`, `calloc`, or `realloc`.
- Never lose the old pointer on `realloc` failure.
- Avoid double free, use-after-free, returning pointers to dead stack storage, and aliasing freed memory.
- Make cleanup paths explicit; `goto cleanup` is often clearer than repeated partial frees.

## 4. Strings and Buffers

- Distinguish text buffers from raw byte buffers.
- Always reserve space for the trailing `'\0'` when handling C strings.
- Prefer bounded APIs such as `snprintf`; treat `strcpy`, `strcat`, and `sprintf` as high risk.
- Do not assume input is null-terminated.
- Validate all external lengths before copying, parsing, or formatting.

## 5. Integers, Types, and Conversions

- Use `size_t` for sizes and indexes tied to memory objects.
- Use `uint32_t`, `int64_t`, and similar fixed-width types for protocol, file, or binary formats.
- Watch signed/unsigned comparisons and implicit narrowing conversions.
- Do not rely on signed overflow behavior.
- Check shifts, bit masks, and casts for width and sign errors.

## 6. Pointer Safety and Undefined Behavior

- Initialize variables before reading them.
- Stay within object bounds during pointer arithmetic and indexing.
- Do not dereference null, dangling, or misaligned pointers.
- Be careful with strict aliasing, object lifetime, and type punning.
- If code depends on layout or packing, make that dependency explicit and verify it.

## 7. Error Handling

- Check return values from allocation, I/O, parsing, and system calls.
- Keep error handling consistent: return codes, `errno`, or project-defined error objects.
- Preserve the original failure cause instead of collapsing all failures into one generic error.
- Clean up partially initialized state on every failure path.

## 8. API Design

- Keep functions small and single-purpose.
- Make preconditions and postconditions obvious.
- Prefer explicit input/output parameters over hidden global effects.
- Use `const` aggressively for read-only inputs.
- Avoid exposing internal struct layout unless callers truly need it.

## 9. Portability

- Do not assume `int`, `long`, pointer, or `time_t` sizes.
- Check endianness when reading or writing binary data.
- Avoid compiler-specific extensions unless guarded and documented.
- Keep POSIX-only code separate from portable core logic when needed.
- Verify format specifiers match the actual type, especially with `size_t` and fixed-width integers.

## 10. Security

- Treat all external input as hostile.
- Guard against buffer overflow, integer overflow, format-string bugs, and path traversal.
- Do not pass untrusted strings as format strings.
- Clear secrets from memory only when the platform and compiler behavior are understood.

## 11. Concurrency

- Assume data races are bugs.
- Protect shared mutable state with a clear ownership or locking model.
- Document thread-safe and thread-unsafe APIs.
- Be careful with signal handlers, atomics, and async-unsafe library calls.

## 12. Testing

- Test normal paths, boundary values, invalid input, and cleanup paths.
- Reproduce bugs with the smallest failing input.
- Use sanitizers and leak checks during tests.
- Add regression tests for every fixed bug that can be reproduced.

## Fast Review Pass

Use this short pass when time is limited:

1. Does it compile cleanly with strict warnings?
2. Is memory ownership clear on every path?
3. Can any copy, parse, or format step exceed a buffer?
4. Are there risky casts, signed/unsigned mixes, or overflow cases?
5. Are failure paths cleaning up correctly?
6. Does anything rely on UB, layout assumptions, or platform quirks?
