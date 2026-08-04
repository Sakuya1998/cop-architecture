# COP Architecture Agent Instructions

## Repository Boundary
- This repository contains architecture assets only.
- Do not add product implementation code.
- `cop-platform` is the implementation repository.

## Authority Rules
1. Read `README.md` and `docs/README.md` before making changes.
2. Treat only `accepted` authoritative documents and ADRs as implementation constraints.
3. Never promote a document from `draft` or `review` to `accepted` without explicit human approval.
4. Never make a major architecture decision inside an implementation task; create or update an RFC first.
5. When an RFC is accepted, create or link an ADR and update all affected authoritative documents.
6. Preserve stable document IDs and relative links.
7. Use Chinese prose, English filenames, and established English technical terms.
8. Prefer Mermaid for diagrams.
9. Do not add `TBD`, `TODO`, filler text, or invented requirements to accepted documents.

## Change Discipline
- Keep each document focused on one responsibility.
- State scope and non-goals explicitly.
- Update directory indexes when adding, moving, deprecating, or superseding documents.
- Keep RFC discussion, ADR decisions, and current-state architecture separate.
