# cskill

一个给 Codex 用的 C 语言开发 skill。

默认规则：

- 新项目默认 `C17`
- 重点检查内存、越界、未定义行为、错误处理、可移植性
- CMake 项目单独看构建规则和 target 传播

目录：

- `cskill/SKILL.md`：skill 入口
- `cskill/references/checklist.md`：C 开发总清单
- `cskill/references/memory.md`：内存注意事项
- `cskill/references/cmake.md`：CMake 注意事项

适用场景：

- 总结 C 开发注意事项
- review C 代码
- 排查内存泄漏、越界、段错误
- 检查 CMake 配置
