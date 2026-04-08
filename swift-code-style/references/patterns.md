# Patterns

## Initialization and Closures

- **Avoid performing any meaningful or time-intensive work in `init()`.** Avoid doing things like making network requests, reading large amounts of data from disk, etc. Create something like a `start()` method if these things need to be done before an object is ready for use.

- **Omit redundant memberwise initializers.** The compiler synthesizes `internal` memberwise initializers for structs, so explicit `internal` initializers equivalent to the synthesized initializer should be omitted. (SwiftFormat: redundantMemberwiseInit)
```swift
// WRONG
struct Planet {
    let name: String
    let mass: Double

    init(name: String, mass: Double) {
      self.name = name
      self.mass = mass
    }
}

// RIGHT
struct Planet {
    let name: String
    let mass: Double
}

// ALSO RIGHT: Custom logic in initializer makes it non-redundant
struct Planet {
    let name: String
    let mass: Double

    init(name: String, mass: Double) {
      self.name = name.capitalized
      self.mass = max(0, mass)
    }
}

// ALSO RIGHT: Public initializer is not redundant since compiler-synthesized
// memberwise initializers are always internal
public struct Planet {
    public let name: String
    public let mass: Double

    public init(name: String, mass: Double) {
      self.name = name
      self.mass = mass
    }
}
```

- **Extract complex callback blocks into methods** to reduced nestedness.
```swift
// WRONG
final class MyClass {

    func performRequest() {
    	API.request { [weak self] response in
        	guard let self else { return }
        	// Complicated processing and side effects...
      	}
    }
}

// RIGHT
final class MyClass {

    func performRequest() {
    	API.request { [weak self] response in
        	guard let self else { return }
        	handleResponse(response: response)
      	}
    }

    func handleResponse(_ response: SomeResponseClass) {
      	// Complicated processing and side effects...
    }
}
```

## Control Flow and Namespacing

- **When validating preconditions at the start of a scope, prefer using `guard` statements over `if` statements.** This reduces nesting, and allows the compiler to verify that the `return` statement is present.

- **Omit the `internal` keyword** when defining types, properties, or functions with an internal access control level. (SwiftFormat: redundantInternal)
```swift
// WRONG
internal class Spaceship {
    internal init() { ... }
    internal func travel(to planet: Planet) { ... }
}

// RIGHT
class Spaceship {
    init() { ... }
    func travel(to planet: Planet) { ... }
}
```

- **Avoid global functions whenever possible.** Prefer methods within type definitions.

- **Use `enum`s for organizing `public` or `internal` constants and functions into namespaces.** Avoid creating non-namespaced global constants and functions. (SwiftFormat: enumNamespaces)
```swift
// WRONG
struct Environment {
    static let earthGravity = 9.8
    static let moonGravity = 1.6
}

// WRONG
struct Environment {

    struct Earth {
      static let gravity = 9.8
    }

    struct Moon {
      static let gravity = 1.6
    }
}

// RIGHT
enum Environment {

    enum Earth {
      static let gravity = 9.8
    }

    enum Moon {
      static let gravity = 1.6
    }
}
```

## Mutability and Type Design

- **Prefer immutable values whenever possible.** Use `map` and `compactMap` instead of appending to a new collection. Use `filter` instead of removing elements from a mutable collection.
```swift
// WRONG
var results = [SomeType]()
for element in input {
    let result = transform(element)
    results.append(result)
}

// RIGHT
let results = input.map { transform($0) }
```

- **Prefer immutable or computed static properties over mutable ones whenever possible.** Use stored `static let` properties or computed `static var` properties over stored `static var` properties whenever possible, as stored `static var` properties are global mutable state.
```swift
// WRONG
enum Fonts {
    static var title = UIFont(...)
}

// RIGHT
enum Fonts {
    static let title = UIFont(...)
}
```

- **Default classes to `final`** if they're not designed to have subclasses. (SwiftFormat: preferFinalClasses)

- **When defining type functions in classes, prefer `static func` over `class func`.** `static func` and `class func` have the same behavior unless the class is subclassed and a subclass needs to override the declaration. If a method needs to be overridden, the author should opt into that functionality by using the `class` keyword instead.
```swift
// WRONG
class Fruit {
    class func eatFruits(_ fruits: [Fruit]) { ... }
}

// RIGHT
class Fruit {
    static func eatFruits(_ fruits: [Fruit]) { ... }
}
```

- **When switching over an enum, generally prefer enumerating all cases rather than using the `default` case.**

- **Check for nil rather than using optional binding if you don't need to use the value.** Checking for nil makes it immediately clear what the intent of the statement is. (SwiftLint: unused_optional_binding)
```swift
var thing: Thing?

// WRONG
if let _ = thing {
    doThing()
}

// RIGHT
if thing != nil {
    doThing()
}
```

## Redundancy and Simplification

- **Omit the `return` keyword when not required by the language.** (SwiftFormat: redundantReturn)

- **Use `AnyObject` instead of `class` in protocol definitions.** (SwiftFormat: anyObjectProtocol)

- **Prefer dedicated logging systems like `OSLog` framework or `swift-log` over writing directly to standard out using `print(…)`, `debugPrint(…)`.**

- **Avoid single-expression closures that are always called immediately**. Instead, prefer inlining the expression. (SwiftFormat: redundantClosure)
```swift
// WRONG
lazy var universe: Universe = {
    Universe()
}()

// RIGHT
lazy var universe = Universe()
```

- **Omit the `get` clause from a computed property declaration that doesn't also have a `set`, `willSet`, or `didSet` clause.** (SwiftFormat: redundantGet)
```swift
// WRONG
var universe: Universe {
    get {
      Universe()
    }
}

// RIGHT
var universe: Universe {
    Universe()
}

// RIGHT
var universe: Universe {
    get { multiverseService.current }
    set { multiverseService.current = newValue }
}
```

- **Avoid defining properties that are then returned immediately.** Instead, return the value directly. Property declarations that are immediately returned are typically redundant and unnecessary. Cleaning them up automatically simplifies the code. In some cases this also results in the `return` keyword itself being unnecessary. (SwiftFormat: redundantProperty)
```swift
// WRONG
var spaceship: Spaceship {
    let spaceship = spaceshipBuilder.build(warpDrive: warpDriveBuilder.build())
    return spaceship
}

// RIGHT
var spaceship: Spaceship {
    spaceshipBuilder.build(warpDrive: warpDriveBuilder.build())
}

// WRONG
var spaceship: Spaceship {
    let warpDrive = warpDriveBuilder.build()
    let spaceship = spaceshipBuilder.build(warpDrive: warpDrive)
    return spaceship
}

// RIGHT
var spaceship: Spaceship {
    let warpDrive = warpDriveBuilder.build()
    return spaceshipBuilder.build(warpDrive: warpDrive)
}
```

- **Prefer using a generated Equatable implementation when comparing all properties of a type.** For structs, prefer using the compiler-synthesized Equatable implementation when possible. Manually-implemented Equatable implementations are verbose, and keeping them up-to-date is error-prone. In projects that provide an `@Equatable` macro, prefer using that macro to generate the `static func ==` for classes. (SwiftFormat: redundantEquatable)
```swift
/// WRONG: The `static func ==` implementation is redundant and error-prone.
struct Planet: Equatable {
    let mass: Double
    let orbit: OrbitalElements
    let rotation: Double

    static func ==(lhs: Planet, rhs: Planet) -> Bool {
      lhs.mass == rhs.mass
        && lhs.orbit == rhs.orbit
        && lhs.rotation == rhs.rotation
    }
}

/// RIGHT: The `static func ==` implementation is synthesized by the compiler.
struct Planet: Equatable {
    let mass: Double
    let orbit: OrbitalElements
    let rotation: Double
}

/// ALSO RIGHT: The `static func ==` implementation differs from the implementation that
/// would be synthesized by the compiler and compared all properties, so is not redundant.
struct CelestialBody: Equatable {
    let id: UUID
    let orbit: OrbitalElements

    static func ==(lhs: CelestialBody, rhs: CelestialBody) -> Bool {
      lhs.id == rhs.id
    }
}
```

- **Avoid using `()` as a type**. Prefer `Void`. (SwiftFormat: void)
```swift
// WRONG
let result: Result<(), Error>

// RIGHT
let result: Result<Void, Error>
```

- **Avoid using `Void()` as an instance of `Void`**. Prefer `()`. (SwiftFormat: void)
```swift
let completion: (Result<Void, Error>) -> Void

// WRONG
completion(.success(Void()))

// RIGHT
completion(.success(()))
```

## Collection APIs

- **Prefer using `count(where: { ... })` over `filter { ... }.count`**. (SwiftFormat: preferCountWhere)
```swift
// WRONG
let planetsWithMoons = planets.filter { !$0.moons.isEmpty }.count

// RIGHT
let planetsWithMoons = planets.count(where: { !$0.moons.isEmpty })
```
