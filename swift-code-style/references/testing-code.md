# Testing Code

- In Swift Testing, don't prefix test case methods with "`test`". (SwiftFormat: swiftTestingTestCaseNames)

- **Omit the Swift Testing `@Suite` macro** unless providing configuration like `@Suite(.serialized)`. A `@Suite` macro without any arguments is redundant: `@Test` method discovery behaves the same with or without the `@Suite` macro. (SwiftFormat: redundantSwiftTestingSuite)
```swift
import Testing

/// WRONG
@Suite
struct SpaceshipTests { ... }

/// RIGHT
struct SpaceshipTests { ... }

/// ALSO RIGHT: @Suite with arguments is not redundant.
@Suite(.serialized)
struct SpaceshipTests { ... }
```

- **In Swift Testing, avoid expectation message strings that restate the expectation without adding additional context.** Unlike `XCTAssert`, the Swift Testing `#expect` macro generates detailed failure messages that include the expectation condition.
```swift
// WRONG: Restates what #expect already reports in failure output
@Test
func engageWarpDrive() {
    spaceship.engageWarpDrive()
    #expect(spaceship.isWarpDriveActive, "Warp drive should be active")
    #expect(spaceship.speed > lightSpeed, "Speed should be greater than light speed")
}

// RIGHT: Omits the message string, or adds valuable context
@Test
func engageWarpDrive() {
    spaceship.engageWarpDrive()
    #expect(spaceship.isWarpDriveActive)
    #expect(spaceship.speed > lightSpeed, "Spaceship must reach light speed before the warp bubble can form")
}

// ALSO RIGHT: Code comments work well to add additional context
@Test
func engageWarpDrive() {
    spaceship.engageWarpDrive()
    #expect(spaceship.isWarpDriveActive)

    // Spaceship must reach light speed before the warp bubble can form
    #expect(spaceship.speed > lightSpeed)
}
```

- **Avoid `guard` statements in unit tests**. Swift Testing has APIs for unwrapping an optional and failing the test, which are much simpler than unwrapping the optionals yourself. Use assertions like `try #require`. (SwiftFormat: noGuardInTests)
```swift
import Testing

struct SomeTests {
    @Test
    func something() throws {
        // WRONG:
        guard let value = optionalValue, value.matchesCondition else { return }

        // RIGHT:
        let value = try #require(optionalValue)
        #expect(value.matchesCondition)
    }
}
```

- **Prefer throwing tests to `try!`**. `try!` will crash your test suite like a force-unwrapped optional. Swift Testing supports throwing test methods, so use that instead. (SwiftFormat: noForceTryInTests)
```swift
import Testing

struct SomeTests {
    // WRONG
    @Test
    func something() {
        try! Something().doSomething()
    }

    // RIGHT
    @Test
    func something() throws {
        try Something().doSomething()
    }
}
```

- **Avoid force-unwrapping in unit tests**. Force-unwrapping (`!`) will crash your test suite. Use safe alternative like `try #require`, which will throw an error instead, or standard optional unwrapping (`?`). (SwiftFormat: noForceUnwrapInTests)
```swift
import Testing

struct SpaceshipTests {
    // WRONG
    @Test
    func canLaunchSpaceship() {
        let spaceship = (dependencies!.shipyardService as! DefaultShipyardService).build()
        spaceship.engine!.prepare()
        spaceship.launch(to: nearestPlanet()!)

        #expect(spaceship.hasLaunched)
        #expect((spaceship.destination! as! Planet) == nearestPlanet())
    }

    // RIGHT
    @Test
    func canLaunchSpaceship() throws {
        let spaceship = try #require((dependencies?.shipyardService as? DefaultShipyardService)?.build())
        spaceship.engine?.prepare()
        spaceship.launch(to: try #require(nearestPlanet()))

        #expect(spaceship.hasLaunched)
        #expect((spaceship.destination as? Planet) == nearestPlanet())
    }
}
```

- **Remove redundant `throws` and `async` effects from test cases**. If a test case doesn't throw any errors, or doesn't `await` any `async` method calls, then `throws` and `async` are redundant. (SwiftFormat: redundantThrows, redundantAsync)
```swift
import Testing

struct PlanetTests {
    // WRONG
    @Test
    func habitability() async throws {
        #expect(earth.isHabitable)
        #expect(!mars.isHabitable)
    }

    // RIGHT
    @Test
    func habitability() {
        #expect(earth.isHabitable)
        #expect(!mars.isHabitable)
    }
}
```