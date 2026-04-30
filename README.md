# AI Governance Framework

A public reference framework for governing generative and agentic AI systems in an enterprise context. Engineering-first: a layered control model, a four-seam threat model, a data-governance discipline, and a lifecycle. External standards (NIST, ISO, OWASP, supervisory guidance) anchor the framework; they do not structure it, because the controls must stand on their own before they are mapped to anyone else's taxonomy.

The framework is normative where controls can be made concrete today and honest where they cannot. Section 20 is an explicit register of edges — controls the framework asserts that current industry primitives do not fully support.

**Read it:** <https://ccoe-io.github.io/aigovernance-framework/>
**Source:** [`docs/AI-GOVERNANCE-FRAMEWORK.md`](docs/AI-GOVERNANCE-FRAMEWORK.md)
**Current version:** 1.1.1 (2026-04-29)

## Who this is for

Platform engineering, application security, application development, ML/AI engineering, product, risk, compliance, AI and CISO practitioners adapting this framework for their organisation. The framework applies to all AI systems an adopting organisation designs, deploys, operates, embeds, or consumes — including third-party SaaS features that use AI server-side on the organisation's data.

This framework is technical and governance reference material. It is not legal, regulatory, audit, or licensing advice. Adopters apply it in conjunction with their own legal, privacy, risk, internal-audit, and supervisory functions.

## How to adopt

1. **Read the framework end-to-end** before tailoring. The tier model (§4), the four-seam threat model (§5), the layered control model (§3), and the known-limits register (§20) are interlocking — partial adoption misses load-bearing pieces.
2. **Tailor the calibration points:** the registration schema (Appendix A), the default bounds (Appendix B), and the control-test matrix (Appendix G) are starter values. Calibrate against your task class, blast radius, latency and cost SLOs, and incident-response capability.
3. **Run a legal-desk review** covering supervisory scope, data-protection regime, and any sector-specific obligations before operational use.
4. **Assign a document owner, an attestation executive, and a Risk Committee** sized to your regulatory context. The Risk Committee may be a board-level committee, an executive AI-risk council, a security council, or a single executive risk owner — whatever holds approval authority for Tier 1 systems and exception decisions in your organisation.

## How to cite

```
Elsaiedy, M. (2026). AI Governance Framework (Version 1.1.1) [Reference framework].
https://github.com/ccoe-io/aigovernance-framework
```

The repository includes a [`CITATION.cff`](CITATION.cff) file, which GitHub uses to power the **Cite this repository** button on the repository page.

## License and attribution

Published under [Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)](LICENSE).

You may share and adapt this framework, including for commercial purposes, provided you:

- give appropriate credit to the author and indicate if changes were made, and
- license adaptations under CC BY-SA 4.0 (the share-alike obligation).

Adopters separately determine the licensing and confidentiality posture of any internal policy or implementation derived from this text — for example, a private internal policy may be circulated within the adopting organisation while the source framework remains under CC BY-SA 4.0.

## Contributing and errata

Issues and pull requests are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md). Substantive proposals are best opened as an issue first so the change can be discussed before drafting. Direct contact: `elsaiedy@ccoe.io`.

## Versioning

Semantic by intent, not by enforcement:

- **Major** — restructuring of the control model, threat model, or layer model.
- **Minor** — new sections, new appendices, or substantive control additions.
- **Patch** — editorial polish, factual corrections, taxonomy alignment, calibration refinements.

Per-version detail lives in [Appendix F (Document history)](docs/AI-GOVERNANCE-FRAMEWORK.md#appendix-f--document-history) of the framework itself; this repository's [CHANGELOG.md](CHANGELOG.md) mirrors it.

## Releases

Each tagged release attaches the canonical Markdown plus rendered DOCX, PDF, and HTML to the corresponding [GitHub Release](https://github.com/ccoe-io/aigovernance-framework/releases). The Markdown source in `docs/` is always the source of truth; rendered artefacts are convenience.
