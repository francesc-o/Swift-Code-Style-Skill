# File Organization

- **Alphabetize and deduplicate module imports within a file.** Place all imports at the top of the file below the header comments. Do not add additional line breaks between import statements. Add a single empty line before the first import and after the last import. _Exception: `@testable import` should be grouped after the regular import and separated by an empty line._ (SwiftFormat: sortedImports, duplicateImports)

- **Limit consecutive whitespace to one blank line or space (excluding indentation).** Favor the following formatting guidelines over whitespace of varying heights or widths. (SwiftFormat: consecutiveBlankLines, consecutiveSpaces)
```swift
// WRONG
struct Planet {

  	let mass:          Double
    let hasAtmosphere: Bool


    func distance(to: Planet) { }

}

// RIGHT
struct Planet {
    let mass: Double
    let hasAtmosphere: Bool

    func distance(to: Planet) { }
}
```

- **Files should end in a newline.** (SwiftFormat: linebreakAtEndOfFile)

- **Declarations that include scopes spanning multiple lines should be separated from adjacent declarations in the same scope by a newline.** Insert a single blank line between multi-line scoped declarations (e.g. types, extensions, functions, computed properties, etc.) and other declarations at the same indentation level. (SwiftFormat: blankLinesBetweenScopes)
```swift
// WRONG
struct SolarSystem {
    var numberOfPlanets: Int {
      ...
    }
    func distance(to: SolarSystem) -> AstronomicalUnit {
      ...
    }
}
struct Galaxy {
    func distance(to: Galaxy) -> AstronomicalUnit {
      ...
    }
    func contains(_ solarSystem: SolarSystem) -> Bool {
      ...
    }
}

// RIGHT
struct SolarSystem {
    var numberOfPlanets: Int {
      ...
    }

    func distance(to: SolarSystem) -> AstronomicalUnit {
      ...
    }
}

struct Galaxy {
    func distance(to: Galaxy) -> AstronomicalUnit {
      ...
    }

    func contains(_ solarSystem: SolarSystem) -> Bool {
      ...
    }
}
```

- **Remove blank lines at the top and bottom of scopes**, excluding type bodies which can optionally include blank lines. (SwiftFormat: blankLinesAtStartOfScope, blankLinesAtEndOfScope)
```swift
// WRONG
class Planet {
    func terraform() {

    	generateAtmosphere()
    	generateOceans()

    }
}

// RIGHT
class Planet {
    func terraform() {
        generateAtmosphere()
    	generateOceans()
    }
}
```

- **Each type and extension which implements a conformance should be preceded by a `MARK` comment.** (SwiftFormat: markTypes)
  - Types should be preceded by a `// MARK: - TypeName` comment.
  - Extensions that add a conformance should be preceded by a `// MARK: - TypeName + ProtocolName` comment.
  - Extensions that immediately follow the type being extended should omit that type's name and use `// MARK: ProtocolName`.
  - If there is only one type or extension in a file, the `MARK` comment can be omitted.
  - If the extension in question is empty (e.g. has no declarations in its body), the `MARK` comment can be omitted.
  - For extensions that do not add new conformances, consider adding a `MARK` with a descriptive comment.

- **Within each top-level section, place content in the following order.** This allows a new reader of your code to more easily find what they are looking for. (SwiftFormat: organizeDeclarations)
  - Nested types and type aliases
  - SwiftUI dynamic properties (@State, @Binding, etc), grouped by type
  - Static properties
  - Instance properties
  - Static property with body
  - Class properties with body
  - Instance properties with body
  - Properties with property observers
  - Computed properties
  - Static methods
  - Class methods
  - Instance methods

- **Only define a single property or enum case per line.** (SwiftFormat: singlePropertyPerLine, wrapEnumCases)
```swift
// WRONG
let mercury, venus: Planet

let earth = planets[2], mars = planets[3]

let (jupiter, saturn) = (planets[4], planets[5])

enum IceGiants {
    case neptune, uranus
}

// RIGHT
let mercury: Planet
let venus: Planet

let earth = planets[2]
let mars = planets[3]

let jupiter = planets[4]
let saturn = planets[5]

enum IceGiants {
    case neptune
    case uranus
}

// ALSO RIGHT: Tuple destructing is fine for values like function call results.
let (ceres, pluto) = findAndClassifyDwarfPlanets()
```