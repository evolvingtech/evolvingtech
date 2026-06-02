# evolvingtech

**Lean 4 + AI for formal verification — engineering and mathematical foundations.**

I build open-source Lean 4 libraries that formalize results which are usually handled by convention, code review, or manual derivation, and that fail silently and expensively when they go wrong. The proofs are developed in collaboration with Claude Code, a workflow well-suited to formal verification because every result either type-checks against the kernel or it does not.

The work spans two tracks:

**Engineering verification** — formalizing the parts of engineering practice that the field relies on but has not historically had machine-checkable foundations for: numerical interoperability between IEEE 754 systems, dimensional consistency of physical models, and the conditions under which independent uncertainties combine.

**Mathematical foundations** — formalizing recent or under-formalized mathematical results that have downstream consequences for the engineering work above, and for the broader formal-methods community.

All repositories are Apache 2.0.

---

## Active projects

### [`ETC Verify™`](https://github.com/evolvingtech/etc-verify) — Foundational substrate for cross-domain interface composition *(active; v0.2.1 released)*
A Lean-based contract algebra for architectural-level verification of how engineered components fit together. Four named operators (sequential composition, shared-resource composition, refinement, conformance) with axiom-free soundness theorems. Interfaces are first-class objects carrying explicit assumes, guarantees, and silences (aspects deliberately left unmodeled, surfaced as typed data rather than hidden). The Contract type is parameterized over a modality marker; the substrate ships with the Untimed modality, and the architecture supports additional modalities via typeclass instances in downstream libraries without modifying the substrate.

### [`IEEE754Audit`](./IEEE754AuditAbstract.md) — IEEE 754 interface audit framework *(in progress; abstract available)*
A Lean 4 toolchain for **pre-deployment interface audits** of floating-point systems. Given two interface specifications and an input domain, the toolchain produces one of two machine-checkable artifacts: (a) a formal proof of bit-identical or tolerance-bounded equivalence, or (b) a precise enumeration of residual ambiguities, each with a concrete witness input that demonstrates divergence. Builds on Mathlib and the in-progress FloatSpec port of Flocq's IEEE 754 formalization, with planned upstream contributions to FloatSpec.

Application domains include aerospace (pre-integration verification of components built by different vendors for remote deployment), autonomous systems, medical devices, and any setting where two conformant implementations of a numerical interface need to be proven equivalent before they're allowed to talk to each other.

### `BuckinghamPi` — Dimensional analysis and the Pi theorem *(release imminent)*
A Lean 4 formalization of the Buckingham Pi theorem with engineering-facing tooling. Useful for formally checking dimensional consistency of physical models, identifying minimal dimensionless groupings, and surfacing cases where an empirical correlation is hiding a missing physical variable.

### [`AddingInQuadrature`](https://github.com/evolvingtech/AddingInQuadrature) — Uncertainty propagation *(released)*
A Lean 4 library covering the conditions under which independent uncertainties combine in quadrature, the conditions under which they don't, and the formal structure behind common engineering shortcuts.

### [`EML`](https://github.com/evolvingtech/EML) — Elementary functions from a single operator *(Phase 0 complete; active)*
A Lean 4 / Mathlib formalization of the EML operator, `eml(x, y) := exp(x) − Log(y)`, which Odrzywołek (arXiv:2603.21852) showed is — together with the constant 1 — sufficient to express all standard elementary functions on appropriate principal-branch domains. A continuous-function analogue of Sheffer/NAND completeness for Boolean logic. The mathematics is due to Odrzywołek; this repository is an independent formalization effort with phase-gated specification documents.

### Interoperability libraries *(in progress)*
Smaller Lean libraries supporting the audit framework above: formal models of unit systems, conversion equivalences, and interface-specification primitives.

---

## Why this, and why now

Formal verification has historically been gated by the cost of writing proofs. AI-assisted proof development changes that cost curve sharply, and the domains that benefit first are the ones where (a) the underlying mathematics is well-understood, (b) the failure modes are expensive or the results are subtle, and (c) the community has been waiting for tractable tooling. Engineering verification — particularly safety-critical, remote, or pre-integration — is exactly that kind of domain. So is the formalization of recent mathematical results that would otherwise sit in PDFs unverified. All of this work is an attempt to build foundations for both, in the open.

---

## Stack

Lean 4, Mathlib, FloatSpec (in progress, with planned contributions), Lean metaprogramming for engineer-facing UI layers.

## Contact

Issues and PRs welcome on individual repositories. Collaboration inquiries: open an issue on the most relevant repo.

---

"ETC Verify" is a trademark of Evolving Technologies Corporation (USPTO Serial No. 99842416, application pending).
