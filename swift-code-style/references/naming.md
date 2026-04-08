# Naming

- **Use PascalCase for type and protocol names, and lowerCamelCase for everything else.** _Exception: You may prefix a private property with an underscore if it is backing an identically-named property or method with a higher access level._

- **Name booleans like `isSpaceship`, `hasSpacesuit`, etc.** This makes it clear that they are booleans and not other types.

- **Acronyms in names (`ID`, `URL`, etc) should be all-caps except when it’s the start of a name that would otherwise be lowerCamelCase, in which case it should be uniformly lower-cased.**
```swift
// WRONG
class UrlValidator {

    func isValidUrl(_ URL: URL) -> Bool {
    	...
    }

    func isProfileUrl(_ URL: URL, for userId: String) -> Bool {
    	...
    }
}

let URLValidator = UrlValidator()
let isProfile = URLValidator.isProfileUrl(URLToTest, userId: IDOfUser)

// RIGHT
class URLValidator {

    func isValidURL(_ url: URL) -> Bool {
    	...
    }

    func isProfileURL(_ url: URL, for userID: String) -> Bool {
    	...
    }
}

let urlValidator = URLValidator()
let isProfile = urlValidator.isProfileURL(urlToTest, userID: idOfUser)
```

- **Event-handling functions should be named like past-tense sentences** (e.g. `didTap`, not `handleTap`). The subject can be omitted if it's not needed for clarity.
```swift
// WRONG
class ExperiencesViewController {

    private func handleBookButtonTap() {
    	...
    }

    private func modelChanged() {
    	...
    }
}

// RIGHT
class ExperiencesViewController {

    private func didTapBookButton() {
    	...
    }

    private func modelDidChange() {
    	...
    }
}
```
