# Antigravity — Project Guide & Task Division

A minimal, modern design-focused code IDE, built via vibe coding.

---

## 1. Vision

- **Simple, not sparse** — fewer features, done well.
- **Modern aesthetic** — clean typography, smooth motion, cohesive theming.
- **Core pillars**: Syntax Highlighting · Extensions · Aesthetic

---

## 2. Tech Stack (proposed)

| Layer | Choice | Why |
|---|---|---|
| Core / backend | Rust | Performance, memory safety, matches team strength |
| Shell / UI | Tauri + React (or Svelte) | Full CSS control for a modern look, lightweight vs Electron |
| Syntax highlighting | Tree-sitter | Industry standard, incremental parsing, huge grammar library |
| Extensions | WASM plugins (wasmtime/wasmer) | Sandboxed, language-agnostic, crash-isolated |
| Theming | JSON/TOML theme files | Data-driven, shared by UI + syntax colors |

---

## 3. Task Division

Splitting by **Core/Systems** vs **UI/Experience** keeps ownership clear and avoids merge conflicts on the same files. Adjust names based on who's stronger where — this is a starting proposal.

### 🧠 Mohit — Core & Systems
- [ ] Set up Rust workspace (`cargo` monorepo: `core`, `editor`, `plugin-host` crates)
- [ ] Text buffer implementation (rope data structure — e.g. `ropey` crate)
- [ ] Integrate Tree-sitter for syntax highlighting (start with 1–2 languages)
- [ ] LSP client integration (autocomplete, diagnostics, go-to-definition)
- [ ] Plugin/extension host using WASM (sandboxing, minimal API surface)
- [ ] File system + project/workspace management (open folder, file tree backend)
- [ ] Performance benchmarking (Hyperfine/Valgrind, matching your Volt workflow)

### 🎨 Mayank — UI & Experience
- [ ] Tauri shell setup + build pipeline
- [ ] Design system: color palette, spacing scale, typography (pick a monospace font)
- [ ] Editor UI shell (tabs, sidebar, status bar, command palette)
- [ ] Theme engine (JSON/TOML → CSS variables + syntax color mapping)
- [ ] Settings UI + keybinding customization
- [ ] Extension marketplace/UI panel (how installed plugins surface to the user)
- [ ] Animations/motion polish (cursor, scroll, panel transitions)

### 🤝 Shared
- [ ] Define the plugin API contract together *before* either side builds against it
- [ ] Weekly sync to merge core ↔ UI integration points
- [ ] Write ADRs (Architecture Decision Records) for major choices, so decisions don't get relitigated

---

## 4. Suggested Build Order

1. Minimal text buffer + Tree-sitter highlighting (Mohit) in parallel with UI shell + theming skeleton (Mayank)
2. Wire buffer into UI — first working editor loop
3. LSP integration → real IDE feel (autocomplete, diagnostics)
4. Extension API — build 1 sample plugin to validate the contract
5. Aesthetic polish pass — animations, theme presets, empty states

---

## 5. GitHub Workflow

- **Branching**: `main` (stable) ← `dev` ← feature branches (`feat/tree-sitter-highlighting`, `feat/theme-engine`, etc.)
- **Commits**: Conventional Commits style (`feat:`, `fix:`, `refactor:`, `docs:`) for a clean history
- **PRs**: Every feature branch → PR into `dev`, reviewed by the other person before merge (2-person review is easy to keep 100% coverage)
- **Issues**: Use GitHub Issues + a simple Kanban Project board (`Todo` / `In Progress` / `Review` / `Done`) mirroring the checklists above
- **README**: Keep a running `README.md` with setup instructions (`cargo build`, `pnpm install`, etc.) so onboarding stays trivial
- **.gitignore**: Standard Rust (`/target`) + Node (`/node_modules`, `/dist`) ignores from the start

---

## 6. Definition of "Done" for v0.1

- [ ] Open a folder, see a file tree
- [ ] Open a file, see syntax-highlighted content
- [ ] Edit and save a file
- [ ] At least one working extension loaded via the plugin API
- [ ] A cohesive default theme (dark + light)

---

*Living document — update as decisions are made. Consider logging major pivots in a `/docs/decisions/` folder as ADRs.*
