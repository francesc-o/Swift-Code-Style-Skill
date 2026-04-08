---
name: swift-code-style
description: Apply house style to Swift, SwiftUI, and Swift Testing code. Use when the coding agent writes, reviews, refactors, or cleans up Swift code and must enforce local conventions, run SwiftLint or SwiftFormat when configured, and manually apply rules not covered by those tools.
---

# Swift Code Style

## Operating Rules

- Code rules are grouped by topics in different files inside `references/`.

- A rule is composed of a description, an optional matching SwiftLint or SwiftFormat rule in `()`, and an optional example.

- Default to the touched Swift files. Only do repo-wide cleanup when the user explicitly asks for it.

- If a `.swiftlint.yml` file exists at the project root, try to run `swiftlint` against the touched scope. If SwiftLint is unavailable or the config file is absent, report that status and apply the rules manually.

- If a `.swiftformat` file exists at the project root, try to run `swiftformat "/path/to/your/code/"` on the touched scope. The path can be a single Swift file or a directory. If SwiftFormat is unavailable or the config file is absent, report that status and apply the rules manually.

- Manually apply every relevant rule that is not enforced automatically, including rules not covered by SwiftLint or SwiftFormat and rules from those tools when the tool or project config is unavailable.


## Workflow Decision Tree

### Review and improve existing code
- Identify the touched Swift files and the local project root for those files.
- Read the touched code and determine which topics apply by using the Topic Router below.
- Load `references/core-rules.md`, `references/naming.md`, `references/patterns.md`, and `references/file-organization.md` for every Swift task.
- Load `references/swiftui-code.md` only when the touched code uses SwiftUI.
- Load `references/testing-code.md` only when the touched code is test code or imports `Testing` or `XCTest`.
- Run SwiftFormat or SwiftLint on the touched scope when the project config files exist and the tools are available.
- Manually apply every relevant rule that is not covered by those tools.
- Re-check the touched scope and report any unresolved issues or tool availability problems.

### Implement new Swift code
- Before finishing new Swift code, run the same review flow on the new or modified files.

### Topic Router

Consult the reference file for each topic relevant to the current task:

| Topic | Reference |
|-------|-----------|
| Core Rules | `references/core-rules.md` |
| Naming Rules | `references/naming.md` |
| Code Patterns | `references/patterns.md` |
| File Organization Rules | `references/file-organization.md` |
| SwiftUI Code | `references/swiftui-code.md` |
| Testing Code | `references/testing-code.md` |

## References

- `references/core-rules.md` -- General styling rules
- `references/naming.md` -- Rules about naming conventions
- `references/patterns.md` -- Code patterns rules that should apply globally
- `references/file-organization.md` -- File organization rules
- `references/swiftui-code.md` -- SwiftUI code related rules
- `references/testing-code.md` -- Testing code related rules
