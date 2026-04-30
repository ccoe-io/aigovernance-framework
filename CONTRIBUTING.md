# Contributing

This framework is published under CC BY-SA 4.0. Contributions are welcome under the same licence.

## What kind of contribution fits

| Kind | How to submit |
|---|---|
| Typo, broken link, factual error, taxonomy drift | Open a pull request directly. |
| Calibration adjustment (default bounds, control-test thresholds, registration fields) | Open an issue first; calibration changes need rationale. |
| New section, new appendix, new control | Open an issue first; substantive additions need discussion before drafting. |
| Standards anchoring update (NIST, ISO, OWASP, supervisory guidance) | Open an issue or PR with a link to the authoritative source. |
| Ports / forks of the framework to a specific sector or jurisdiction | Fork under CC BY-SA 4.0; the ShareAlike obligation requires the same licence on the fork. |

## House rules

- **Cite primary sources.** Standards, regulations, and CVE references must link to the canonical issuer. Secondary commentary is acceptable but secondary.
- **Match the existing tone.** The framework is normative where controls can be made concrete today and honest where they cannot. Hedge only where the underlying primitive genuinely does not exist; do not soften load-bearing requirements.
- **Preserve the layer / tier / threat-model structure.** New material attaches at the right layer (Inference, Orchestration, Application, Tooling, Data, Identity, Observability), the right tier, and the right seam — it does not create parallel taxonomy.
- **Update Appendix F.** Every accepted change adds an Appendix F entry. Editorial polish goes in a "Editorial:" line; factual or normative changes get their own line.
- **No vendor advocacy.** Vendor names appear when they are the de facto example of a primitive. No promotion, no comparative ranking, no commercial language.

## Licensing your contribution

By opening a pull request, you agree that your contribution is licensed under CC BY-SA 4.0, the same licence as the rest of the framework. There is no separate Contributor Licence Agreement.

If your contribution incorporates material from another source, you must:

- have the right to relicense it under CC BY-SA 4.0 (typically: original work, public-domain material, or material already under a CC BY-SA-compatible licence), and
- attribute the original source in the contribution itself.

Material under licences incompatible with CC BY-SA 4.0 (for example CC BY-NC, proprietary text) cannot be accepted.

## Reporting non-public concerns

For factual errors that could mislead adopters in production-sensitive ways, or for governance flaws you would prefer to disclose privately before opening a public issue, see [SECURITY.md](SECURITY.md).

## Direct contact

`elsaiedy@ccoe.io`
