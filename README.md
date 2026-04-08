# Swift Code Style Skill

An agent skill that helps AI coding agents write, review, and refine Swift code using consistent, production-oriented style rules. It applies conventions across core Swift, naming, architectural patterns, file organization, SwiftUI, and Swift Testing, using SwiftLint and SwiftFormat when a project is configured for them and falling back to manual enforcement when needed. The skill is heavily based on [Airbnb's Swift Style Guide](https://github.com/airbnb/swift); it's highly recommended the usage with SwiftLint and SwiftFormat tools.

## Installing Swift Code Style

### Option A: Using skills.sh (recommended)

Install this skill with a single command:

```bash
npx skills add https://github.com/francesc-o/Swift-Code-Style-Skill --skill swift-code-style
```

### Option B: Manual Install

1. **Clone** this repository
2. **Install or symlink** the `swift-code-style/` folder following your tool's skills installation docs

## License

This skill is open-source and available under the MIT License. See [LICENSE](LICENSE) for details.
