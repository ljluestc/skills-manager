# PR Description — Add Hermes Workspace to supported tools

## Summary
This PR adds Hermes Workspace as a first-class supported tool in skills-manager. Hermes uses `SKILL.md` as its native skill format, which aligns directly with skills-manager’s multi-format architecture and enables low-friction integration.

Closes #148.

## Motivation
Hermes is an actively maintained desktop skill manager with meaningful community adoption. Adding support expands interoperability and allows users to discover, validate, and manage Hermes-native skills through the same registry and tooling workflows already used by skills-manager.

Key reasons:
- Native format alignment (`SKILL.md`) minimizes translation complexity.
- Existing Hermes ecosystem quality is already demonstrated by registry skills.
- Hermes Plugin Bridge offers practical integration points for future enhancements.

## Problem
Today, Hermes users cannot treat Hermes as an explicit supported tool target in skills-manager metadata and workflows. This limits discoverability, compatibility signaling, and automated checks for Hermes-specific use cases.

## Proposed Changes
1. Add Hermes Workspace to the supported tools catalog/config.
2. Wire Hermes into tool capability metadata and validation paths.
3. Ensure `SKILL.md`-based workflows are recognized as Hermes-compatible where relevant.
4. Update docs/user guidance with Hermes support details and usage examples.

## Scope
- In scope:
  - tool registry/config updates for Hermes
  - compatibility and validation plumbing
  - documentation updates
- Out of scope:
  - deep Hermes runtime/plugin implementation beyond support declaration and compatibility integration
  - unrelated tool behavior changes

## Compatibility
- Backward compatible for all existing tools.
- Hermes support is additive and opt-in.
- No breaking changes expected in current parsing/validation pipelines.

## Risk Assessment
- **Risk**: differences in interpretation of `SKILL.md` fields between ecosystems.
- **Risk**: metadata validation drift if Hermes-specific assumptions are incomplete.
- **Mitigation**:
  - add focused tests for Hermes support paths
  - keep shared format handling canonical and tool-specific deltas explicit
  - document any Hermes-specific constraints

## Validation Plan
- Unit tests:
  - supported-tool registration includes Hermes
  - capability metadata exposes Hermes correctly
  - `SKILL.md` compatibility paths remain stable
- Integration/CLI checks:
  - list/inspect supported tools includes Hermes
  - registry skill validation recognizes Hermes target
- Regression checks:
  - existing tool support remains unchanged

## Rollout
- Merge as additive support.
- Announce in release notes/changelog and docs.
- Follow up with optional Hermes Plugin Bridge enhancements if desired.

## Checklist
- [x] Full local PR description drafted.
- [ ] Functional implementation completed.
- [ ] Tests added/updated and passing.
- [ ] Documentation and changelog updated.
