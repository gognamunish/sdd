---
applyTo: "**/{test,tests}/**"
---
# Testing conventions

- Test behaviour, not implementation. One behaviour per test; name it
  `test_<expected>_when_<condition>`.
- Stack: pytest + pytest-asyncio. Use fixtures for setup; no shared mutable global state.
- Arrange-Act-Assert, with a blank line between the three sections.
- No network calls in unit tests. No `time.sleep` / arbitrary `asyncio.sleep` — control time
  by injecting `as_of` / a fake clock into the code under test.
- Every acceptance criterion in `spec.md` (the Gherkin `Given/When/Then`) maps to at least
  one test. The test docstring should name the criterion it covers.
- New endpoint or domain rule => add a contract test that asserts against the OpenAPI spec
  in the feature's `contracts/` folder.
