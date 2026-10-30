---
name: ux-track
description: UX design in Penpot before build. Use when work changes screens, flows, UI states or components.
---

Requires the Penpot MCP server. Run each stage as a ticket, in order. A stage closes when the user approves it. Commit `docs/ux` changes with their ticket. Per-repo `docs/ux/design-system.md` names the component library and maps Penpot components to code.

1. **Flows**: Mermaid state diagrams in `docs/ux/<feature>/flows.md`: happy path, loading, empty, error, no-permission. One-screen work starts at stage 2.
2. **Screen spec**: `docs/ux/<feature>/screens.md`, one section per state: each UI element, what the user can do with it, and what follows.
3. **Design**: one board per state, named `<flow>/<state>`, in Penpot file `<repo> / <feature>`, built from library components. Report a missing component to the user and wait for their decision. Human edits win: re-read Penpot before each change. The ticket names the file and pages, and is done when every state has its board. On approval, run `penpot.currentFile.saveVersion("approved-<n>")` and cite that version in build tickets. A later design change reopens this stage with the next `<n>`.
4. **Contract**: OpenAPI or GraphQL for exactly what the boards render, reusing existing endpoints. Check the field mapping in the ticket note. A gap goes back to Design.
5. **Build**, test-first: tokens, components, screens on fixtures, real services. Test targets come from the flow transitions; fixtures validate against the contract.
6. **Verify UI**, with the user: screenshot each state of the running app beside its exported board.

## Penpot

Code runs in a live browser tab. Before Design, the user connects a tab to MCP (**MCP → Connect** on self-hosted or hosted Penpot, the plugin's connect button with a local MCP server) and keeps it open; a reload needs a reconnect. Board names store as `flow / state`, with spaces. On Penpot 2.18, reading `penpot.currentUser` throws because the plugin lacks `user:read`.
