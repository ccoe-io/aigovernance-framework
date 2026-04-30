# Reporting governance or factual issues

This repository hosts a reference framework, not running software. The matching report channel is for governance flaws or factual errors that could mislead adopters — not code vulnerabilities.

## What to report here

- A control statement that is unsound (would not actually achieve the stated security property in practice).
- A standards or regulatory reference that is materially wrong (wrong article, wrong issuer, superseded without acknowledgement).
- A taxonomy mapping that misrepresents the source taxonomy (for example an OWASP, NIST, or ISO reference that does not match the cited document).
- A factual claim about a vendor primitive, CVE, or research result that is incorrect.

If the issue does not affect anyone's adoption decisions, open a normal pull request or issue instead.

## How to report

Email `elsaiedy@ccoe.io` with:

- the section or appendix the issue affects,
- the specific claim and the corrected claim,
- a primary-source link if the correction depends on it,
- whether you believe the issue warrants a published correction (a minor patch release with a CHANGELOG entry) or a private acknowledgement.

You will receive an acknowledgement within seven calendar days. Substantive issues are landed in the next patch release, with attribution unless you ask otherwise.

## What is out of scope

- Code vulnerabilities — there is no code in this repository other than the GitHub Actions workflow that builds the documentation site. If you find an issue with that workflow, open a normal issue or pull request.
- Disagreements with framework opinions or trade-offs — the public issues queue and Appendix F's living-document discipline are the right venue.
