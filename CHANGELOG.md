# Changelog

All notable changes to this framework are recorded in
[Appendix F (Document history)](docs/AI-GOVERNANCE-FRAMEWORK.md#appendix-f--document-history)
of the framework itself. This file is a high-level mirror; consult Appendix F
for the authoritative per-version diff.

The framework follows a semantic-by-intent convention:

- **Major** — restructuring of the control model, threat model, or layer model.
- **Minor** — new sections, appendices, or substantive control additions.
- **Patch** — editorial polish, factual corrections, taxonomy alignment, calibration refinements.

## [1.1.1] — 2026-04-29

Editorial polish and publication-readiness pass for public release.

- Added author and maintainer attribution to the front-matter table and the
  "How to cite" block to satisfy CC BY-SA 4.0 attribution requirements.
- Tightened licence wording (CC BY-SA 4.0, no "recommended" hedge) and the
  "not legal, regulatory, audit, or licensing advice" statement.
- Generic Risk Committee definition for adopters of varying size.
- §12.7 restructured: notification categories in body, named regimes (GDPR,
  SEC, NIS2, DORA, HIPAA) moved to Appendix D as illustrative.
- Appendix C provenance moved above the OWASP Agentic 2026 mapping table with
  a coverage-drift caveat.
- Appendix G expanded with Owner, Frequency, and Evidence-artefact columns.
- Factual fix: §8.2 / §20.1 OAuth 2.1 token-exchange → OAuth 2.0 Token Exchange
  (RFC 8693) within the OAuth 2.1 framing adopted by the MCP authorisation spec.
- Appendix D U.S. banking MRM row rewritten for the joint OCC / Fed / FDIC
  issuance of 17 April 2026 (Fed SR 26-2 / OCC Bulletin 2026-13), which
  supersedes SR 11-7 / OCC Bulletin 2011-12 and expressly excludes generative
  and agentic AI from its scope.
- Product-name accuracy: Vertex AI Agent Builder, Anthropic Claude Agent SDK,
  OpenAI Agents SDK, AWS Strands, Nudge Security, Grip Security.
- Calibration: §10.3 numeric starter threshold for high-tool-call frequency;
  §11.1 5% sample defined as the human-spot-check fraction of LLM-judged samples;
  §11.2 golden-set protection sharpened (read-once / audit-logged path / bulk
  programmatic access prohibited; Tier 1 Outcome-analysis golden sets held
  third-party, escrowed, or by a separated internal function).
- Appendix C ASI07 row footnoted to acknowledge the OWASP-published mapping
  reproduces the OWASP source verbatim despite apparent semantic mismatch.
- §13.5 OTel "preferred interchange format" wording sharpened to distinguish
  stabilising model-span attributes from still-experimental agent-span and
  tool-span attributes.
- §17 named the adapter-pattern implementation primitives (LiteLLM, Portkey,
  Cloudflare AI Gateway, hand-rolled provider adapter) and clarified that
  vendor-internal routing primitives such as Bedrock cross-region inference
  profiles do not by themselves satisfy provider-portability.

For the full v1.1.0 → v1.1.1 list, see Appendix F of the framework.

## [1.1.0] — Earlier

Working-draft milestones prior to v1.1.1 are recorded in the framework's
Appendix F.

[1.1.1]: https://github.com/ccoe-io/aigovernance-framework/releases/tag/v1.1.1
