# Product Requirements Document (PRD): Prompts CLI v2

This document outlines the requirements for the next version of the Prompts CLI, a tool for managing prompts for large language models (LLMs).

## Executive Summary

The Prompts CLI v2 enhances the developer experience by introducing a more streamlined and powerful workflow for prompt management. Key improvements include automatic storage location management, content-addressable storage using hashes for de-duplication, fuzzy search capabilities, and a more flexible interactive command structure.

This document also outlines the requirements for building a terminal user interface (TUI) and a desktop application for managing prompts for large language models (LLMs) using the Tauri framework.

## Product Overview

### Problem Statement

Developers need a frictionless way to manage their LLM prompts. The previous version of the CLI required manual file management (`--file` parameter), which was cumbersome. Naming prompts was an extra step, and finding them relied on exact matches. This version aims to remove these barriers.

For users who are less comfortable with command-line interfaces, or for those who prefer a more feature-rich graphical environment, a dedicated desktop application is the ideal solution for prompt management.

While a CLI is powerful, some users prefer a more visual and interactive way to browse, search, and manage their prompts. A TUI can provide a more intuitive user experience for these users, while still remaining within the terminal environment.

### Solution Approach

The CLI will now manage its own storage, using a default location within the user's config or data directory. Prompts will be identified by a hash of their content, eliminating the need for manual naming and enabling automatic de-duplication. Commands will support both one-shot and interactive modes, and a fuzzy finder will help users quickly locate prompts.

The Prompts Tauri application will be a desktop application built using Tauri, with a web-based frontend (likely using a framework like React or Vue) and a Rust backend. It will be part of the `prompts-cli` repository, but will be built and distributed as a separate application.

The Prompts TUI will be built in Rust using the Ratatui library. It will be an integrated part of the `prompts-cli` binary, launched with a specific command or flag (e.g., `prompts-cli tui`).

### Target Audience

- Developers who utilize LLMs for software development, content generation, or automation.
- Users who are comfortable working in a command-line environment.
- Anyone who needs to manage a large collection of prompts efficiently.
- Users who prefer a graphical user interface (GUI) for their applications.
- Developers and content creators who want a dedicated and powerful tool for managing their prompts.
- Users who want a consistent prompt management experience across different operating systems (Windows, macOS, and Linux).
- Developers and power users who spend a significant amount of time in the terminal.
- Users who prefer a more visual and interactive experience than a traditional CLI.
- Those who want to manage their prompts without switching to a full graphical desktop application.

## Product Goals and Success Metrics

| Goal | Success Metric | Target |
|---|---|---|
| **Frictionless Prompt Management** | Time to add and retrieve a prompt is significantly reduced. | < 5 seconds per operation |
| **Intuitive User Experience** | High user satisfaction and adoption rates. | >95% positive feedback from users |
| **Robust and Scalable Storage**| The system handles thousands of prompts efficiently without performance degradation. | CRUD operations remain under 100ms with 10,000+ prompts |
| **Cross-platform Compatibility** | Builds and runs seamlessly on Linux, macOS, and Windows. | 100% supported platforms |
| User-friendly prompt management   | High user satisfaction and positive reviews  | High ratings in app stores/feedback channels |
| Native desktop experience         | Seamless integration with the host OS        | Use of native notifications, menus, etc. |
| Cross-platform compatibility      | Builds and runs on Linux/macOS/Windows       | 100% supported platforms       |
| Intuitive prompt management       | User satisfaction and ease of use            | High ratings from user feedback |
| Responsive and performant UI      | Smooth and lag-free user experience          | < 100ms response to user input |
| Seamless integration with CLI     | Easy to launch and exit the TUI from the CLI | Clear and simple commands      |

## User Stories

| Story ID | User Story | Acceptance Criteria | Priority |
|---|---|---|---|
| **US-001** | As a developer, I want the CLI to automatically manage where my prompts are stored so I don't have to think about it. | The CLI uses a default, user-specific directory (e.g., `~/.config/prompts-cli`). The user can override this with a `--config` flag. | P0 |
| **US-002** | As a developer, I want to add prompts without having to name them, and the tool should handle duplicates. | A prompt's content is hashed to create a unique ID. Adding an existing prompt is a no-op. | P0 |
| **US-003** | As a developer, I want to quickly find a prompt even if I only remember parts of it. | Commands that need to identify a prompt use a fuzzy search on the prompt text. | P0 |
| **US-004** | As a developer, I want to either provide a prompt directly in a command or have the CLI ask me for it. | Commands like `add`, `show`, `edit`, `delete` support both a one-shot mode (prompt in args) and an interactive mode (reads from stdin). | P1 |
| **US-005** | As a developer, when my fuzzy search returns multiple results, I want the CLI to show me the options so I can choose the correct one. | The CLI returns a structured JSON list of matching prompts, including their text and hash, for the user to make a specific choice. | P1 |
| US-006 | As a terminal user, I want a TUI to browse and manage prompts interactively. | Responsive Ratatui interface with keyboard navigation. | P1 |
| US-007 | As a desktop app user, I want a user-friendly UI for managing prompts with drag & drop and rich controls. | Tauri app with native integrations and a smoother UX. | P1 |
| US-010 | As a TUI user, I want to see a list of my prompts with their titles and tags. | A scrollable list of prompts is displayed on launch. | P1 |
| US-011 | As a TUI user, I want to be able to select a prompt and view its full content. | A dedicated view shows the full text of the selected prompt. | P1 |
| US-012 | As a TUI user, I want to be able to edit a prompt's content and metadata directly within the interface. | An editing mode allows for in-place modification of prompts. | P1 |
| US-013 | As a desktop user, I want to be able to use my mouse to navigate the application and interact with my prompts. | The application is fully navigable with a mouse. | P1 |
| US-014 | As a desktop user, I want to receive native notifications for certain events (e.g., when a prompt is successfully saved). | The application uses the OS's native notification system. | P1 |
| US-015 | As a desktop user, I want to be able to customize the application's appearance (e.g., with a light or dark theme). | The application provides theme customization options. | P2 |
| **US-026** | As a desktop user, I want a WYSIWYG editor to write and edit my prompts. | The application has a WYSIWYG editor for prompts. | P1 |
| **US-027** | As a desktop user, I want to be able to create folders to organize my prompts. | The application supports creating folders and dragging prompts into them. | P1 |
| **US-028** | As a desktop user, I want syntax highlighting for code in my prompts. | The application has syntax highlighting for code in prompts. | P1 |
| **US-029** | As a desktop user, I want to be able to open multiple windows to work on multiple prompts. | The application supports multiple windows. | P2 |
| **US-030** | As a desktop user, I want to be able to open the application with a global shortcut. | The application can be opened with a global shortcut. | P2 |

## Technical Architecture

The CLI will be built in Rust. Key libraries will include:
- **Clap**: For command-line argument parsing.
- **Directories**: For finding the appropriate user-specific storage location on different operating systems.
- **Sha2**: for hashing prompt content to create a unique ID.
- **Fuzzy-matcher**: For implementing fuzzy search.
- **Serde**: For serializing and deserializing prompt data.

Prompts will be stored as individual JSON files in a dedicated directory. The filename for each prompt will be the SHA256 hash of its content.

The TUI will be built in Rust using the Ratatui and Crossterm libraries. It will be a module within the `prompts-cli` crate and will share the same core logic for prompt management.

The desktop application will be built using the Tauri framework. The frontend will be a single-page application (SPA) built with a modern web framework like React or Vue. The backend will be written in Rust and will be responsible for all the core prompt management logic.

## Feature Specification

- **Storage:**
    - **Default Location:** The tool will use a default directory (e.g., `~/.config/prompts-cli/prompts`).
    - **Custom Location:** A `--config` global flag will allow users to specify an alternative storage directory.
    - **Content-Addressable:** Prompts are stored in files named by the SHA256 hash of their content.
- **Commands:**
    - `add [PROMPT_TEXT]`: Adds a new prompt. If `PROMPT_TEXT` is not provided, it reads from stdin.
    - `list`: Lists all stored prompts, showing a snippet and their hash.
    - `show [FUZZY_QUERY]`: Searches for a prompt. If multiple are found, returns a JSON list. If one is found, it's displayed. If `FUZZY_QUERY` is not provided, it reads from stdin.
    - `edit [FUZZY_QUERY]`: Same search mechanism as `show`. If a single prompt is identified, it opens it for editing (e.g., in the user's `$EDITOR`).
    - `delete [FUZZY_QUERY]`: Same search mechanism as `show`. Deletes the identified prompt after confirmation.
    - `generate [FUZZY_QUERY]`: Same search mechanism as `show`. Generates text based on the identified prompt.
- **Fuzzy Search:**
    - Implemented for `show`, `edit`, `delete`, and `generate`.
    - Matches against the text of the prompts.
    - If multiple matches are found, outputs a JSON array of objects, where each object contains the prompt text and its hash.
- **Prompt Metadata:**
    - The `Prompt` struct will contain the prompt text, tags, and categories. The hash is used as the external identifier.
- **Interactive Prompt List**: A scrollable and filterable list of all prompts.
- **Prompt Content View**: A detailed view of the selected prompt's content and metadata.
- **In-TUI Editing**: The ability to edit prompts directly within the TUI.
- **Keyboard-driven Navigation**: Intuitive keybindings for all actions.
- **Help and Documentation**: A help screen that explains the keybindings and features.
- **Graphical Prompt Management**: All the core CRUD, search, and filtering features will be available through a user-friendly GUI.
- **Rich Text Editing**: A "what you see is what you get" (WYSIWYG) editor or a code editor with syntax highlighting for editing prompts.
- **Drag and Drop**: The ability to drag and drop prompts to reorder them or organize them into folders.
- **Native OS Integration**: Use of native menus, notifications, and other OS-specific features.
- **Customization**: Options to customize the application's appearance and behavior.
- **WYSIWYG Editor**: A WYSIWYG editor for writing and editing prompts.
- **Prompt Folders**: Create folders and drag and drop prompts into them.
- **Syntax Highlighting**: Syntax highlighting for code in prompts.
- **Multiple Windows**: Open multiple windows to work on multiple prompts.
- **Global Shortcut**: Open the application with a global shortcut.

## Architectural Considerations

- **Frontend Framework:** The choice of frontend framework (e.g., React, Vue, Svelte) will have a significant impact on the architecture of the frontend. The framework should be chosen based on the team's expertise and the specific needs of the application. A component-based framework is recommended to keep the UI code organized and maintainable.
- **Component Library:** To ensure a consistent and polished look and feel, the application should use a component library (e.g., Material UI, Bootstrap). The component library should be chosen based on the desired aesthetic and the features it provides.
- **State Management:** The frontend will need a robust state management solution to handle the application's state. This could be a simple solution like React's built-in state management, or a more advanced solution like Redux or MobX. The choice of state management solution will depend on the complexity of the application.
- **Communication with Rust Backend:** The frontend will communicate with the Rust backend using Tauri's API. The architecture needs to define a clear and stable API for this communication. The API should be versioned to allow for future updates without breaking older versions of the frontend.

## Testing Strategy

- **Unit Tests:** The frontend code should be unit tested to ensure that it is working correctly. This will involve using a testing framework like Jest or Vitest to test the individual components and functions.
- **Component Tests:** Each component should be tested in isolation. This will involve rendering the component and then asserting on the rendered output. The `@testing-library/react` library is recommended for this purpose.
- **E2E Tests:** E2E tests will be created to test the application as a whole. This will involve running the application and then simulating user input to test the application's functionality. A framework like Cypress or Playwright is recommended for this purpose.
- **Visual Regression Testing:** Visual regression testing should be used to ensure that the UI doesn't change unexpectedly. This will involve taking a screenshot of the application and then comparing it to a previously saved screenshot. A tool like Percy or Applitools can be used for this purpose.

## Out of Scope for v2

- GUI or TUI interfaces.
- Syncing prompts across devices.
- Advanced versioning of prompts.
