### Security Advisory & Project Context

#### About This Project
This repository contains a **historical Proof of Concept (POC)** conducted in _April 2023_. The goal was to evaluate open‑source tools (Trivy, Grype, Syft) for Docker vulnerability scanning and SBOM generation. The primary artifacts are **scan reports** for 30+ Docker images—a dataset that illustrates risk profiles of various base images.

> **Note:** This was a **methodology POC**, not a production CI/CD pipeline. No scripts are included; the scanning commands are available in the main `README.md`.

#### Supply Chain Context: The March 2026 Trivy Incident
In March 2026, a supply chain attack compromised Trivy binaries distributed via official channels. This incident highlights the importance of **defense in depth** and **toolchain integrity**—principles that were central to this POC:

- **Multiple scanners** – I used **Trivy *and* Grype** to reduce reliance on a single tool.
- **Rebuild, don’t patch** – The guidance emphasised rebuilding images with newer base layers, aligning with Docker’s immutable design.
- **SBOM focus** – Generating SBOMs (via Syft) provides a component inventory independent of any scanner’s output.

#### Modern Best Practices (Post‑Incident)
If you are using this POC as a reference, please adopt these practices for production use:

1. **Pin tool versions** – Use immutable SHAs (e.g., `aquasecurity/trivy-action@a7c5f...`), never `latest`.
2. **Verify binary signatures** – Before execution, verify the tool’s signature using `cosign` or GPG.
3. **Sign SBOMs** – Attach signed SBOMs (CycloneDX/SPDX) to every image release.
4. **Run scanners with least privilege** – Use ephemeral containers with minimal permissions.
5. **Rotate on compromise** – If a tool is compromised, rebuild your CI/CD environment from a known‑good state.

#### Contact
For any questions or security concerns, feel free to reach out via [LinkedIn](https://in.linkedin.com/in/hitendesai).
