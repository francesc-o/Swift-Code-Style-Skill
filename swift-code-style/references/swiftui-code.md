# SwiftUI Code

- **Prefer using the `@Entry` macro to define properties inside `EnvironmentValues`.** When adding properties to SwiftUI `EnvironmentValues`, prefer using the compiler-synthesized property implementation when possible. (SwiftFormat: environmentEntry)
```swift
/// WRONG: The `EnvironmentValues` property depends on `IsSelectedEnvironmentKey`
struct IsSelectedEnvironmentKey: EnvironmentKey {
    static var defaultValue: Bool { false }
}

extension EnvironmentValues {
    var isSelected: Bool {
        get { self[IsSelectedEnvironmentKey.self] }
        set { self[IsSelectedEnvironmentKey.self] = newValue }
    }
}

/// RIGHT: The `EnvironmentValues` property uses the @Entry macro
extension EnvironmentValues {
    @Entry var isSelected: Bool = false
}
```

- **Omit `@ViewBuilder` when it is not required.** `@ViewBuilder` is implicit on `View.body` properties and `ViewModifier.body(content:)` functions, and is unnecessary on single-expression properties or functions. (SwiftFormat: redundantViewBuilder)
```swift
// WRONG
struct PlanetView: View {
    @ViewBuilder
    var body: some View {
        Text("Hello, World!")
    }

    @ViewBuilder
    var subtitle: some View {
        Text("Subtitle")
    }
}

// RIGHT
struct PlanetView: View {
    var body: some View {
        Text("Hello, World!")
    }

    var subtitle: some View {
        Text("Subtitle")
    }
}

// ALSO RIGHT: @ViewBuilder is necessary for conditionals
struct ConditionalView: View {
    var showDetails: Bool

    var body: some View {
        title
    }

    @ViewBuilder
    var title: some View {
        if showDetails {
            Text("Details")
        } else {
            Text("Summary")
        }
    }
}
```

- **In SwiftUI view builders, omit redundant `else { EmptyView() }` branches.** An `if` statement without an `else` branch implicitly produces no content when the condition is false, making an explicit `else { EmptyView() }` unnecessary. (SwiftFormat: redundantEmptyView)
```swift
// WRONG
var body: some View {
    if condition {
        Text("Launch")
    } else {
        EmptyView()
    }
}

// RIGHT
var body: some View {
    if condition {
        Text("Launch")
    }
}
```

- **Always use `foregroundStyle()` instead of `foregroundColor()`.**

- Always use `clipShape(.rect(cornerRadius:))` instead of `cornerRadius()`.

- Prefer `ForEach(x.enumerated(), id: \.element.id)` instead of `ForEach(Array(x.enumerated()), id: \.element.id)`, avoiding useless array conversion.

- Prefer `.overlay` and `.background` instead of a `ZStack` wrapper when possible.
