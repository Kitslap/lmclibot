# Security policy

## Reporting a vulnerability

Please do not open a public issue for a security problem. Use GitHub's private reporting instead:
open the **Security** tab of this repository (Kitslap/lmclibot), then **Report a vulnerability**
(a private security advisory). There is no e-mail address for this. Include the affected version, the
steps to reproduce and the impact you observed.

## What is checked at every release

Every release is built from source by continuous integration. The build fails, and nothing is published,
if any of these checks fails:

- Trivy on the source tree: Go and npm dependencies (HIGH and CRITICAL, fixable) and embedded secrets.
- Trivy on the built binary: the Go standard library and the modules compiled in.
- govulncheck on the built binary (Go standard library and modules actually reachable).
- A CycloneDX SBOM of the binary.
- An Ed25519 signature of the version and of `SHA256SUMS.txt` (see the README to verify it).

## Where to find the scan reports

They are attached to each release, next to the installer and the portable exe, and are covered by
`SHA256SUMS.txt` and its signature:

- `LMClibot-<version>-trivy.json`: Trivy report of the binary, with the full package list.
- `LMClibot-<version>-govulncheck.txt`: govulncheck output for the binary.
- `LMClibot-<version>-sbom.cdx.json`: the SBOM.

A scan of the portable exe alone with an external tool lists nothing for a Go binary, so a clean result
there proves little; these reports are the reference.

## Fix policy

A corrected version is intended to be published as soon as a fixable HIGH or CRITICAL vulnerability affects the
binary. This is already enforced by the release pipeline: such a vulnerability blocks publication.
