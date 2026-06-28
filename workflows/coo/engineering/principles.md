# Engineering Principles

**Owned by:** Engineering | **Enforced by:** All pods and sub-agents

These are non-negotiable principles that every engineering activity must follow — from initial analysis through final integration. They apply to Engineering itself, all pods, and all sub-agents.

---

## 1. Think Before Coding

**No code is written until the problem is understood and modeled.**

- Analyze the delta (current state → desired state) before touching any file.
- Update UML/SysML models first. The model IS the specification.
- If you can't explain what you're building and why, stop and ask.
- Spending time modeling saves more time than it costs — rework from unclear specs is the most expensive work there is.

**In practice:**
- Engineering produces models before decomposing features into tasks.
- Pods receive task briefs that include the model context — they don't have to guess.
- If a pod discovers the model is wrong during execution, it stops and reports back rather than coding around the gap.

---

## 2. Simplicity First

**The simplest solution that meets the requirement is the correct solution.**

- Don't add features "just in case." Build what's needed, not what might be needed.
- Don't introduce abstractions for a single use point. Abstraction earns its cost when it's used at least twice.
- Don't over-engineer. A working simple solution beats a perfect complex one.
- Prefer composition over inheritance. Prefer functions over classes when both work.
- Prefer existing patterns in the codebase over novel approaches.

**In practice:**
- Every feature must justify its complexity. If a simpler approach exists, use it.
- "We might need this later" is not a reason to build something now.
- Code that is easy to delete is better than code that is "flexible."

---

## 3. Surgical Changes

**Every change must be as small and focused as possible.**

- One task = one coherent change. If a task touches unrelated files, split it.
- Don't refactor unrelated code in the same PR as a feature change.
- Don't fix "nearby" bugs in a feature branch — log them as separate backlog items.
- Don't reformat files you're not actively changing.
- A reviewer should be able to understand a PR in under 5 minutes.

**In practice:**
- Pods work within their task scope only. No drive-by refactors.
- If a pod discovers technical debt during execution, it logs it — it doesn't fix it in the same task.
- PRs should be reviewable in under 200 lines of meaningful diff.

---

## 4. Goal-Driven Execution

**Every piece of work must trace back to a clear goal.**

- Features trace to product goals (user needs, business outcomes).
- Tasks trace to features.
- Tests trace to acceptance criteria.
- If you can't trace your current work to a goal, stop and ask why you're doing it.

**In practice:**
- Every task brief includes the feature it serves and the user need that feature addresses.
- Pods don't add scope beyond what the task specifies — if they see a better approach, they report it back.
- "It would be nice to have" is not a goal. "This enables X which delivers Y" is a goal.

---

## 5. Update Models Before Code

**UML/SysML models are the contract. Code that doesn't match the model is wrong.**

- Models are updated before code — not after, not alongside.
- Models must show: what classes/blocks exist, how they relate, what they do, and how data/control flows between them.
- Code that implements a feature must match the approved model. If the model is wrong, update the model first, then update the code.
- Models are reviewed by PLM (product correctness) and CSO (strategic alignment) before coding begins.

**In practice:**
- Engineering updates models as the first step in any feature work.
- Pods receive model context in their task briefs — they implement what the model specifies.
- If a pod finds a model inconsistency during execution, it stops and reports — it doesn't silently deviate.

---

## 6. Technical Debt Must Be Accounted For

**Technical debt is real work. It goes on the backlog. It doesn't get swept under the rug.**

- When Engineering or a pod discovers technical debt, it gets logged as a backlog item — not silently fixed, not ignored.
- Technical debt items have: description, impact, estimated cost, and risk if unaddressed.
- Engineering prioritizes technical debt alongside features — the PLM and CSO decide the balance.
- "We'll fix it later" without a backlog item is not acceptable.

**In practice:**
- Pods log technical debt they encounter during execution as a separate backlog item, not as part of their current task.
- Engineering triages technical debt items during sprint planning.
- Refactoring is a first-class backlog item — it requires the same justification and approval as a feature.

---

## 7. Tests Represent the User

**Tests verify that the user's need is met. Nothing more, nothing less.**

- Tests should exercise real user workflows, not implementation details.
- A test that passes but wouldn't catch a real user-facing bug is worthless.
- Integration tests that verify cross-product flows are more valuable than unit tests of trivial getters.
- Test names should describe the user scenario: "approving a timesheet updates the approval status" not "test_timesheet_approve_001."

**In practice:**
- Every task includes test scenarios that represent real user workflows.
- Trivial tests (getters, setters, pure data access with no logic) provide no value — don't write them.
- If a test would pass even with a broken implementation, delete it and write a better test.
- Test coverage is measured by "user scenarios covered" not "lines covered."

---

## 8. Trivial Tests Provide No Value

**If a test can't fail in a meaningful way, it shouldn't exist.**

- Don't write tests for: simple getters/setters, data access with no logic, framework boilerplate, type declarations.
- A test that only verifies a mock returns what it was told to return is testing the test framework, not your code.
- Every test must be able to fail due to a real bug in the implementation.
- If removing a test would not reduce confidence in the code, remove it.

**In practice:**
- Pods write tests for: business logic, state transitions, cross-service interactions, error paths, and user-facing workflows.
- Pods do NOT write tests for: property accessors, simple data reads, framework configuration, or type-only code.
- Engineering reviews test quality during PR review — trivial tests are rejected.

---

## 9. Challenge → Clarify → Commit

**When uncertain, follow this sequence. Never guess.**

1. **Challenge** — If a requirement is unclear, a model doesn't make sense, or a task is ambiguous, speak up immediately. Don't make assumptions.
2. **Clarify** — Ask the right person. Model question → Engineering. Product question → PLM. Strategic question → CSO. Don't ask peers who have the same information you do.
3. **Commit** — Once clarified, commit fully. Don't half-implement while waiting for more answers. Either the task is ready to execute or it goes back to Engineering for re-decomposition.

**In practice:**
- Pods that receive an ambiguous task brief report back immediately — they don't guess and produce wrong code.
- Engineering responds to clarification requests before the pod continues — blocking a pod is Engineering's responsibility.
- "I think this means X" is not clarification. "The model shows X but the spec says Y — which is correct?" is clarification.

---

## Summary

| Principle | Core Idea |
|---|---|
| Think Before Coding | Model first, code second |
| Simplicity First | Build only what's needed, simplest approach wins |
| Surgical Changes | Small, focused, no scope creep |
| Goal-Driven Execution | Every task traces to a user/business goal |
| Update Models Before Code | Models are the contract |
| Technical Debt Must Be Accounted For | Log it, prioritize it, don't hide it |
| Tests Represent the User | Test real workflows, not implementation |
| Trivial Tests Provide No Value | If it can't catch a real bug, delete it |
| Challenge → Clarify → Commit | Never guess — ask, then commit fully |
