# Pre-Deployment Interface Audits for IEEE 754 Floating-Point Interoperability

**A Lean 4 methodology for verifying numerical agreement between independently-built systems before integration.**

*Public abstract, version 0.1 — May 2026*
*Apache 2.0*

---

## Summary

Two systems can both be IEEE 754 compliant and still disagree at the bit level. The standard fixes representation and the basic arithmetic operations under a chosen rounding mode, but it leaves under-specified — by design — several degrees of freedom that matter when independently-built implementations must interoperate. Where conventional integration testing can recover from interoperability defects on the ground, deployment regimes without a recovery loop (remote, safety-critical, or assembled-once) cannot. This abstract describes a methodology for **pre-deployment interface audits**: a Lean 4 toolchain that produces a machine-checkable artifact certifying that two interface specifications either (a) agree under a defined input domain to within a stated tolerance, or (b) diverge — with a concrete witness input demonstrating the divergence — in ways the specifications must resolve before deployment is safe.

## The problem

IEEE 754 is necessary but not sufficient for cross-implementation agreement. The standard prescribes bit-exact results for the basic operations (+, −, ×, ÷, √, FMA) under a specified rounding mode, but it does not prescribe — or only recommends rather than mandates — several behaviors that diverge across compliant implementations:

- **Evaluation order of compound expressions.** `(a + b) + c` and `a + (b + c)` may differ at the bit level; the standard does not select between them.
- **FMA contraction.** Whether `x*y + z` is computed with one rounding (fused) or two (separate) is implementation-defined.
- **Correctly-rounded transcendentals.** `sin`, `cos`, `exp`, `log` are *recommended* to be correctly rounded in IEEE 754-2019 but not mandated; `libm` implementations differ in the last few ULPs.
- **Subnormal handling.** Performance-oriented FPU configurations (FTZ, DAZ) produce results that diverge from the reference behavior in the subnormal range.
- **NaN bit-pattern propagation** and **exception flag interaction with control flow** are platform- and configuration-dependent.

Each of these is a degree of freedom on which two vendors can make different reasonable choices, producing an interoperability defect that survives both vendors' internal testing because each implementation is correct under its own — silently different — interpretation of the spec.

## The methodology

The methodology is built on the Lean 4 theorem prover, supported by:

- **Mathlib**, Lean's mathematical library, for the underlying real-analysis and ordered-field machinery.
- **FloatSpec** (Beneficial-AI-Foundation), the in-progress Lean 4 port of Flocq's IEEE 754 formalization, with planned upstream contributions from this project.

Proofs are developed with AI-assisted proof construction (Claude Code, LeanCopilot, and similar tooling). Lean's kernel checks every proof term independently of how the term was produced, so the trustworthiness argument is unaffected by the proof's origin — a property that makes AI-assisted formal verification trustworthy at scale rather than in spite of the AI involvement.

## Application domains

The methodology applies wherever independently-built numerical components must interoperate under conditions that do not permit post-deployment iteration:

- Aerospace and space-based infrastructure, where remote deployment forecloses field repair.
- Autonomous-systems integration, where multi-vendor sensor fusion and control-loop handoffs cross trust boundaries.
- Medical-device interoperability, where regulatory and patient-safety constraints raise the cost of post-deployment defect discovery.
- Financial-systems interconnects, where bit-level numerical disagreement between counterparty implementations creates reconciliation and audit liability.

## Status and roadmap

This is a working methodology under active development:

- **Foundations layer.** Mathlib and FloatSpec dependencies are tracked against current upstream state. Where FloatSpec contains placeholder lemmas, the audit perimeter reports them honestly rather than depending on them silently.
- **Public toolchain.** A first public release of the audit framework is planned under Apache 2.0.
- **Upstream contributions.** Lemmas completed during audits that have general utility are intended for upstream contribution to FloatSpec and, where appropriate, Mathlib.

## Limitations

A Lean proof certifies that an implementation conforms to a *formalized specification*. It does not guarantee that the formalization faithfully captures the intended standard. Specification-to-Lean translation is itself a place where errors can enter and should be peer-reviewed by domain experts.

## References

**Floating-point formalization:** Boldo & Melquiond, *Computer Arithmetic and Formal Proofs* (ISTE / Elsevier, 2017); Beneficial-AI-Foundation, *FloatSpec* (github.com/Beneficial-AI-Foundation/FloatSpec, accessed 2026); Chang, Park, Lim & Nagarakatte, *FLoPS: Semantics, Operations, and Properties of P3109 Floating-Point Representations in Lean* (arXiv:2602.15965, 2026); Harrison, *Formal Verification of Floating Point Trigonometric Functions* (FMCAD 2000); Leroy, *Formal verification of a realistic compiler* (CACM 52:7, 2009).

**IEEE 754 and numerical analysis:** IEEE Std 754-2019; Goldberg, *What Every Computer Scientist Should Know About Floating-Point Arithmetic* (ACM Computing Surveys 23:1, 1991); Higham, *Accuracy and Stability of Numerical Algorithms*, 2nd ed. (SIAM, 2002).

**Lean and Mathlib:** de Moura & Ullrich, *The Lean 4 Theorem Prover and Programming Language* (CADE 28, 2021); the mathlib Community, *The Lean mathematical library* (CPP 2020).

---

*This abstract summarizes work in progress. A more comprehensive treatment is maintained as an internal working draft and will be released publicly as the methodology matures.*
