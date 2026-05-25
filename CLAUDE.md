# CLAUDE.md

Test-driven development discipline for implementing a feature or fixing a bug. Merge with project-specific instructions as needed.

The rule: write the failing test first, then the minimal code to pass it, then refactor. One test at a time. Test behavior, not implementation.

**Tradeoff**: TDD biases toward design clarity and coverage over raw speed. For trivial or throwaway code, use judgment.

## 1. Write the failing test first

Before any implementation code exists, write one test that describes the behavior you want.

- The test names the requirement: what input, what output, what edge case.
- Run it. It must fail. A test that passes before the code exists is testing nothing.
- The failure confirms the test is wired up and will catch a regression later.

## 2. Red, Green, Refactor

The cycle, always in this order:

- Red: write one failing test for the simplest unhandled case.
- Green: write the minimal code that makes it pass. No more.
- Refactor: improve the code without changing behavior; tests stay green.

Then repeat for the next case. The order is the discipline. Skipping Red means the test proves nothing; skipping Refactor leaves Green code uncleaned.

## 3. One test at a time

Do not write twenty tests up front and then implement against all of them.

- One test, one implementation, then the next test.
- Each cycle is small enough to hold in your head and fast enough to run in seconds.
- A large batch of pre-written tests turns into a speculative spec you then chase, instead of a tight loop you drive.

## 4. Minimal code to pass

Write only what the current failing test requires.

- No abstraction the test did not ask for.
- No configurability, no extra parameters, no speculative branches.
- Hardcode if hardcoding passes the test; the next test forces the generalization.

The next failing test is what pulls the design forward. Over-building in Green hides untested code paths.

## 5. Test behavior, not implementation

Assert on what the code produces, not how it produces it.

- Test the public surface: inputs and outputs, observable state changes, side effects.
- Do not assert that an internal helper was called or that a private field holds a value.
- Mock external dependencies (network, clock, third-party APIs) only. Let internal modules run real code.

A test coupled to implementation breaks on every refactor and protects nothing. A test coupled to behavior survives refactors and catches real regressions.

---

## When to use TDD

- New features from scratch
- Complex business logic
- Bug fixes: write a failing test that reproduces the bug, then make it pass
- Refactoring: tests first, so you can prove behavior did not change
- APIs and libraries, where the contract is the product

## When TDD is overkill

- Proof-of-concept or throwaway code
- UI layout tweaks better verified visually
- Trivial CRUD with no logic
- Generated code

Default to TDD when uncertain.

---

## Anti-patterns

- Writing all tests first, then all code. Drive one cycle at a time instead.
- Testing implementation details. Assert on behavior, not internal calls.
- Over-mocking. Mock external dependencies only; mocking everything means you are testing mocks.
- Skipping Red. If the test never failed, it is not testing anything.
- Over-building in Green. Write the minimum; let the next test force more.

---

**See also**: the RIPER workflow ([riper-workflow-skills](https://github.com/HermeticOrmus/riper-workflow-skills)) uses TDD inside its Execute phase.

**License**: MIT. Use it, fork it, merge it into your own.
