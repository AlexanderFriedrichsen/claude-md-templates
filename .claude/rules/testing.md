# Testing Rules

## Philosophy

- Tests are the primary verification mechanism. If it's not tested, it's not done.
- Prefer running a single test file during development — faster feedback.
- Run the full suite before committing.
- Visual/aesthetic logic cannot be fully automated — flag visual changes for manual review.

## Tools

<!-- Uncomment and adjust for your stack -->

<!-- ### JavaScript/TypeScript -->
<!-- - **Vitest** — unit and integration tests (fast, Vite-native) -->
<!-- - **Playwright** — e2e and visual regression tests -->
<!-- - **Testing Library** — component interaction tests -->

<!-- ### Python -->
<!-- - **pytest** — unit and integration tests -->
<!-- - **Playwright** — e2e tests -->

<!-- ### Go -->
<!-- - **go test** — unit tests -->
<!-- - **testify** — assertions and mocking -->

## Structure

- Test files are co-located: `src/lib/foo.ts` -> `src/lib/foo.test.ts`
- Test function names describe behavior: `test('returns empty array when no items match filter')`
- Use fixtures for shared setup. Keep them close to where they're used.

## What to Test

- **Domain logic:** core business rules, data transformations, validation
- **API integration:** mock external APIs; test request construction and response parsing
- **Database queries:** mock the database client; test query construction and response handling
- **Input validation:** test boundaries, invalid input, and error messages
- **Component behavior:** user interactions, state transitions, conditional rendering

<!-- [PROJECT-SPECIFIC] Add your domain-specific test targets here -->
<!-- Example: -->
<!-- - Edge cases: empty cart, expired session, rate-limited API -->

## What NOT to Test

- Pixel-perfect visual output — use manual review or snapshot tests cautiously
- Third-party library internals
- Framework behavior (routing, rendering engine)
- CSS/animation values directly — test the data that drives them

## Patterns

<!-- Adjust for your test framework -->

### Vitest

- `vi.mock` for module mocking
- `vi.fn()` and `vi.spyOn` for function spies
- `describe` blocks to group related behavior
- Use `beforeEach`/`afterEach` for setup/teardown, not shared mutable state

### pytest

- `@pytest.fixture` for setup
- `monkeypatch` for mocking
- `parametrize` for data-driven tests

## Notes on External APIs

- Never make real API calls in tests — always mock
- Test that requests include the correct parameters and headers
- Test that responses are parsed correctly even when the API returns unexpected formats
- Test timeout and error states — external APIs are slow and unreliable

## Notes on File Uploads (if applicable)

- Test file type validation (accept only expected formats)
- Test file size limits
- Mock storage backends — do not upload real files in unit tests
