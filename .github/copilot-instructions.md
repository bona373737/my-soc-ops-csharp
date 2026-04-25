---
description: Development guidelines and workspace conventions for the Soc Ops project
---

# Soc Ops — Copilot Workspace Instructions

Welcome! This document guides Copilot in understanding the **Soc Ops** project architecture, conventions, and development workflow.

## 📋 Project Overview

**Soc Ops** is an interactive **Social Bingo game** for in-person mixers and community events. Built with **Blazor WebAssembly (.NET 10)**, it's designed as both a working application and an educational lab showcasing modern development practices with AI assistance.

**Live Demo:** https://dotnet-presentations.github.io/vscode-github-copilot-agent-lab/  
**Lab Guide:** [workshop/GUIDE.md](../../workshop/GUIDE.md)

## 🏗️ Architecture

```
SocOps/
├── Components/          # Reusable Blazor components
│   ├── BingoBoard.razor          # Main game board (5x5 grid)
│   ├── BingoSquare.razor         # Individual square component
│   ├── BingoModal.razor          # Win celebration modal
│   ├── GameScreen.razor          # Active game view
│   └── StartScreen.razor         # Game initialization
├── Pages/               # Routable pages
│   ├── Home.razor               # Main entry point
│   ├── Counter.razor            # Demo page
│   ├── Weather.razor            # API demo
│   └── NotFound.razor           # 404 handler
├── Services/            # Business logic & state management
│   ├── BingoGameService.cs      # State management, localStorage, event publishing
│   └── BingoLogicService.cs     # Pure logic: board generation, win detection
├── Models/              # Data structures
│   ├── GameState.cs             # Game phase enum (Start, Playing, Won)
│   ├── BingoSquareData.cs       # Single square (ID, question, marked)
│   └── BingoLine.cs             # Winning line (position, direction)
├── Data/                # Static content
│   └── Questions.cs             # Hardcoded bingo questions
├── Layout/              # UI shells
│   ├── MainLayout.razor
│   ├── MainLayout.razor.css
│   ├── NavMenu.razor
│   └── NavMenu.razor.css
└── wwwroot/             # Static assets
    ├── index.html               # SPA host
    ├── css/app.css              # Custom utility classes
    └── lib/bootstrap/           # Bootstrap assets
```

### Key Design Patterns

- **Event-Driven State Management:** `BingoGameService` publishes `OnStateChanged` events; components subscribe to react to state updates
- **Separation of Concerns:** `BingoLogicService` contains pure logic (no dependencies); `BingoGameService` handles state & persistence
- **localStorage Integration:** Game state persists via JSInterop; survives page reloads
- **Component Composition:** Reusable components (`BingoSquare`, `BingoBoard`) avoid duplication

## 🚀 Development Setup

### Prerequisites
- **.NET 10 SDK** (verified: `dotnet --version`)
- **VS Code** with GitHub Copilot enabled
- **Git** for version control

### Quick Start Commands

```bash
# From project root
cd SocOps

# Build the project
dotnet build SocOps.csproj

# Run dev server (port 5166, hot-reload enabled)
dotnet run

# (Future) Run tests
dotnet test
```

**Dev Server:** http://localhost:5166  
**Framework:** Blazor WebAssembly (CSR)  
**Target Platform:** Browser (any modern browser with WebAssembly support)

## 📐 Code Conventions

### C# Standards
- **Naming:** PascalCase for public members, `_camelCase` for private fields
- **Nullability:** `#nullable enable` enforced; use nullable types explicitly
- **Usings:** Implicit usings enabled; no duplicate namespaces
- **Async:** Prefer `async/await` for all I/O operations

### Blazor Component Patterns
- **Naming:** `*.razor` for components, `*.razor.css` for scoped styles
- **Parameters:** Public properties decorated with `[Parameter]`
- **Callbacks:** Event handlers named `Handle[Event]` (e.g., `HandleSquareClicked`)
- **Lifecycle:** Use `OnInitializedAsync()` for initialization, `OnParametersSetAsync()` for dependency changes

### CSS & Styling

**See:** [.github/instructions/css-utilities.instructions.md](./instructions/css-utilities.instructions.md)

- Custom utility classes (Tailwind-inspired) in `wwwroot/css/app.css`
- No external CSS frameworks; Bootstrap lib included but not required
- Scoped styles in `.razor.css` files for component-specific styles
- CSS variables for theming and consistency

### Frontend Design

**See:** [.github/instructions/frontend-design.instructions.md](./instructions/frontend-design.instructions.md)

When designing UI components:
- Avoid generic "AI slop" aesthetics; commit to a cohesive visual direction
- Use distinctive typography, thoughtful color palettes, and intentional animations
- Prefer CSS-only animations for Blazor components
- Match implementation complexity to the design vision

## 🔧 Specialized Agents & Workflows

This workspace includes custom **agents** for specific development tasks:

| Agent | Purpose | Invocation |
|-------|---------|-----------|
| **Quiz Master** | Curate theme-specific icebreaker questions | `/agents/quiz-master` |
| **UI Review** | Critique and improve component designs | `/agents/ui-review` |
| **TDD (Red/Green/Refactor)** | Test-driven development workflow | `/agents/tdd{-red,-green,-refactor}` |
| **Pixel Jam** | Rapid UI/UX iteration | `/agents/pixel-jam` |

**See:** [.github/agents/](./agents/) for detailed agent definitions.

## 📝 Specialized Prompts

| Prompt | Purpose | Invocation |
|--------|---------|-----------|
| **Setup** | Bootstrap development environment | `/prompts/setup` |
| **Cloud Explore** | API discovery and integration | `/prompts/cloud-explore` |

**See:** [.github/prompts/](./prompts/) for detailed prompt definitions.

## ✅ Development Checklist

Before committing or pushing changes:

- [ ] Code builds: `dotnet build SocOps.csproj` passes
- [ ] No compiler errors or warnings
- [ ] No unused imports or variables
- [ ] C# naming conventions followed (PascalCase public, `_camelCase` private)
- [ ] Components have clear, documented parameters
- [ ] Styling uses existing utilities; avoids inline styles
- [ ] localStorage state changes are intentional and persist correctly
- [ ] Game logic is in `BingoLogicService` (pure, testable)
- [ ] State changes trigger `OnStateChanged` events appropriately

## 📚 Documentation & Learning

- **README:** [README.md](../../README.md) — Project overview and quick start
- **Lab Guide:** [workshop/GUIDE.md](../../workshop/GUIDE.md) — Multi-part educational lab
- **Contributing:** [CONTRIBUTING.md](../../CONTRIBUTING.md) — CLA and code of conduct
- **Code of Conduct:** [CODE_OF_CONDUCT.md](../../CODE_OF_CONDUCT.md) — Microsoft Open Source CoC

## 🎯 Common Development Tasks

### Adding a New Bingo Question
1. Edit [SocOps/Data/Questions.cs](../../SocOps/Data/Questions.cs)
2. Add your question to the list
3. Run `dotnet run` to test — board regeneration is automatic

### Creating a New Component
1. Create `SocOps/Components/MyComponent.razor`
2. Add a corresponding `SocOps/Components/MyComponent.razor.css` for scoped styles
3. Update component that uses it to include `<MyComponent />`
4. Component automatically hot-reloads during development

### Modifying Game Logic
1. Edit `SocOps/Services/BingoLogicService.cs` (pure functions)
2. Update `BingoGameService.cs` if state management is affected
3. Ensure any state change calls `OnStateChanged?.Invoke()`
4. Test win detection, board generation, etc.

### Styling Components
1. Use existing utility classes from `wwwroot/css/app.css`
2. Add scoped styles in `ComponentName.razor.css` when utilities are insufficient
3. Avoid inline `style=""` attributes
4. Check [css-utilities.instructions.md](./instructions/css-utilities.instructions.md) for available utilities

## 🤝 Working with Copilot

### Recommended Practices
- Use `@workspace` context reference to ground suggestions in your codebase
- Request specific agents for specialized tasks (e.g., `/agents/ui-review` for design feedback)
- Ask for explanations when learning the architecture
- Request test cases alongside implementations
- Use `Ctrl+I` for inline suggestions while editing

### Avoid Anti-patterns
- Don't commit changes without running `dotnet build`
- Don't add external CSS frameworks without consulting the team
- Don't put business logic in components; use services
- Don't create localStorage state without verifying persistence
- Don't mix pure logic with side effects in `BingoLogicService`

## 🔗 Related Resources

- [.NET Blazor Documentation](https://learn.microsoft.com/en-us/aspnet/core/blazor/)
- [.NET 10 Release Notes](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-10)
- [GitHub Copilot in VS Code](https://docs.github.com/en/copilot/getting-started-with-github-copilot)

---

**Last Updated:** April 2026  
**Framework:** Blazor WebAssembly + .NET 10  
**Language:** C# • Razor • CSS
