# SwiftHeadlessWebKit

[![CI](https://github.com/ShawnBaek/SwiftHeadlessWebKit/actions/workflows/ci.yml/badge.svg)](https://github.com/ShawnBaek/SwiftHeadlessWebKit/actions/workflows/ci.yml)
[![Swift 6](https://img.shields.io/badge/Swift-6.2-orange.svg?style=flat)](https://swift.org)
[![Platform](https://img.shields.io/badge/Platform-iOS%20%7C%20macOS%20%7C%20Linux-blue.svg?style=flat)](https://swift.org)
[![License](https://img.shields.io/badge/License-MIT-lightgrey.svg?style=flat)](LICENSE)

A **headless web browser** for Swift that works on **iOS, macOS, and Linux**.

> Developed by [Shawn Baek](https://github.com/ShawnBaek) using [Spec Kit](https://github.com/github/spec-kit) methodology with [Claude Code](https://claude.ai/claude-code).

---

## Highlights

- **Cross-Platform** - Single API works on iOS, macOS, and Linux
- **JavaScript Execution** - Full client-side rendering on all platforms
- **Swift 6** - Built with strict concurrency (`async/await`, `Sendable`)
- **Simple API** - Just `import SwiftHeadlessWebKit` on any platform

---

## Installation

Add to your `Package.swift`:

```swift
dependencies: [
    .package(url: "https://github.com/ShawnBaek/SwiftHeadlessWebKit.git", from: "2.0.0")
]
```

Add the dependency to your target:

```swift
.target(
    name: "YourApp",
    dependencies: ["SwiftHeadlessWebKit"]
)
```

That's it! The package automatically uses the right engine for each platform.

---

## Quick Start

```swift
import SwiftHeadlessWebKit

// Create browser
let browser = WKZombie()

// Fetch and parse a webpage
let page: HTMLPage = try await browser.open(url: URL(string: "https://example.com")!).execute()

// Find elements using CSS selectors
let links = page.findElements(.cssSelector("a.product-link"))
```

---

## Linux Server Example

SwiftHeadlessWebKit enables web scraping with full JavaScript rendering on Linux servers - perfect for Vapor apps:

```swift
import Vapor
import SwiftHeadlessWebKit

func routes(_ app: Application) throws {
    app.get("scrape") { req async throws -> String in
        let browser = WKZombie()
        let page: HTMLPage = try await browser.open(
            url: URL(string: "https://example.com")!
        ).execute()

        let titleResult = page.findElements(.cssSelector("title"))
        if case .success(let titles) = titleResult, let title = titles.first {
            return title.text ?? "No title"
        }
        return "No title found"
    }
}
```

---

## CSS Selectors

| Selector | Example | Description |
|----------|---------|-------------|
| `.id("value")` | `.id("header")` | Find by ID |
| `.class("value")` | `.class("btn")` | Find by class |
| `.name("value")` | `.name("email")` | Find by name attribute |
| `.cssSelector("query")` | `.cssSelector("div.card > a")` | Custom CSS selector |

---

## Platform Details

| Platform | Engine | JavaScript |
|----------|--------|------------|
| macOS/iOS | WKWebView | ✅ Full |
| Linux | WPE WebKit | ✅ Full |

---

## Development with Spec Kit

This project uses **Spec Kit** methodology with Claude Code:

```bash
# Define new features
/speckit.specify Add WebSocket support

# Check test cases
/speckit.specify check test cases for JavaScript rendering
```

### Guidelines
- Use `/speckit.specify` before implementing features
- Follow Swift 6 conventions with strict concurrency
- Write tests using Swift Testing (`@Test`, `#expect`)
- Ensure CI passes on all platforms

---

## Credits

- Original [WKZombie](https://github.com/mkoehnke/WKZombie) by [Mathias Köhnke](https://twitter.com/mkoehnke)
- Modernization by [Shawn Baek](https://github.com/ShawnBaek) with [Claude Code](https://claude.ai/claude-code)
- HTML parsing by [SwiftSoup](https://github.com/scinfu/SwiftSoup)

## License

MIT License - See [LICENSE](LICENSE) file.

---

**Copyright (c) 2025 Shawn Baek**
