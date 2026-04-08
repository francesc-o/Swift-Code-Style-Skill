# Core Rules

- Follow the [Swift API Design Guidelines](https://swift.org/documentation/api-design-guidelines/) when designing new APIs.

- New code shouldn't create new swiftlint issues or errors: check for them after a build report or by using the `swiftlint` command line tool and resolve if present.

- New code shouldn't create new swiftformat issues or errors: use the `swiftformat "/path/to/your/code/"` command line tool to format the code.

- Each line should have a maximum column width of 150 characters. (SwiftFormat: wrap)

- Trim trailing whitespace in all lines. (SwiftFormat: trailingSpace)