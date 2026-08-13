# Low-Effort PR Examples & Quality Expectations

This document outlines common examples of "low-effort" pull requests in the aegis-contracts repository. PRs exhibiting these anti-patterns will typically fail maintainer evaluation and be closed. 

To ensure your contributions are valuable, thoroughly reviewed, and merged, study the contrast between unacceptable submissions and their expected alternatives.

---

## 1. Superficial or Cosmetic Changes

**The Anti-Pattern:** Submitting a PR that solely fixes a minor typo in a comment, reorders imports without a structural reason, or tweaks markdown formatting, while claiming a feature or substantial bounty.

**Unacceptable Example:**
```rust
// Changes:
// - // Check if the admin exists
// + // Checks if the admin exists
```

**Expected Alternative:** 
If you notice typos, batch them together across the entire codebase or include them as a secondary commit in a PR that delivers meaningful, tested logic changes.

---

## 2. Under-Tested Logic (Missing Coverage)

**The Anti-Pattern:** Adding or modifying core compliance logic, role checks, or state transitions in Soroban without providing corresponding unit tests that prove the logic works (and fails) as expected.

**Unacceptable Example:**
```rust
// Adding a new transfer restriction without testing it
pub fn transfer_with_lock(env: Env, from: Address, to: Address, amount: i128) {
    from.require_auth();
    // Implementation added...
    // But no test file updated!
}
```

**Expected Alternative:**
Every new public contract invocation must include robust tests.

---

## 3. Unsafe or Unidiomatic Rust (Panics)


**Unacceptable Example:**
```rust
let admin = env.storage().instance().get(&DataKey::Admin).unwrap();
```

**Expected Alternative:**
```rust
let admin: Address = env.storage().instance().get(&DataKey::Admin)
    .ok_or(Error::NotAuthorized)?;
```

---

## 4. Failing CI / Ignored Checks

**The Anti-Pattern:** Opening a PR, noticing the GitHub Actions CI pipeline fails, and requesting a maintainer review anyway.

**Expected Alternative:**
Before requesting review, run `make verify` locally. If CI fails after pushing, inspect the logs, fix the formatting, linter warnings, or broken tests, and push the corrections.

---

## Summary of a "High-Effort" PR

1. **Meaningful:** Solves a documented issue.
2. **Tested:** Includes robust positive and negative tests.
3. **Safe:** Uses idiomatic Rust error handling.
4. **Green CI:** Passes all local and remote checks.
5. **Descriptive:** Explains what changed and why.
