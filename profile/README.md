# Agent Economic Protocol (AeP) — AeP Labs
### *Hardware-Enforced Runtime Containment & Complete Mediation for Autonomous AI*

[![Audit: GMU SECSAT](https://img.shields.io/badge/Research%20Partner-GMU%20SECSAT%20Lab-blue?style=for-the-badge)](https://aep-labs.com)
[![License: Apache-2.0 / BUSL-1.1](https://img.shields.io/badge/Open%20Core-Apache--2.0%20%7C%20BUSL--1.1-green?style=for-the-badge)](https://aep-labs.com)
[![USPTO: Patent Pending](https://img.shields.io/badge/USPTO-Patent%20Pending%20(%23225171)-gold?style=for-the-badge)](https://aep-labs.com)

---

> "A computer can never be held accountable, therefore a computer must never make a management decision."
> — **IBM Presentation**, 1979

## 🛡️ Complete Mediation Architecture
**AeP Labs** builds hardware-isolated runtime containment for autonomous AI agents operating across financial infrastructure, cloud APIs, and cyber-physical systems. Through **in-line complete mediation** inside commodity Trusted Execution Environments (**Intel TDX**, **AMD SEV-SNP**, **AWS Nitro Enclaves**) and embedded `#![no_std]` microcontrollers (**RP2350**), the agent's signing key is generated inside the enclave and never exported—requiring every proposed action to pass through a deterministic 128-bit fixed-point (`I64F64`) sequential risk monitor (`ALLOW`, `DENY`, `MODIFY`, `STEP_UP`, `DEFER`) in `<50 µs` before signing.

Upon a behavioral invariant breach, the enclave zeroizes the volatile signing key in RAM (`<1 ms` cutoff target) and emits a hardware-signed **Proof of Physical Consequence (PoPC)** record binding pre-halt and post-halt state deltas to a fresh challenge nonce.

---

## 🏛️ 3-Tier Open-Core Repository & Licensing Architecture

All code repositories in the `AeP-Agent-Economic-Protocol` organization are currently maintained as **Private** pending USPTO Nonprovisional Omnibus filing confirmation, and are organized by licensing tier:

| Tier | Repository | License | Scope & Components |
| :--- | :--- | :--- | :--- |
| **Tier 1: Open-Source Commons** | `aep-commons-apache` | **`Apache-2.0`** | Client SDKs (`sdk/python`, `sdk/typescript`), Multi-Cloud TEE Quote & PoPC Verifiers (`attestation-verifier`), Async Telemetry Server (`aep-telemetry-server`), Hardware TEE Driver Shims (`aep-tee-drivers`), and Reproducible Evaluation Suites. |
| **Tier 2: Protocol Core & Risk Engine** | `aep-protocol-core-busl` | **`BUSL-1.1`** *(converts to `AGPL-3.0` on `2029-05-04`)* | `#![no_std]` `I64F64` Fixed-Point Filter Kernel (`aep-math-core`), Post-Quantum Keystore (`aep-core`), RFC 6962 Merkle Audit Journal (`aep-journal`), Complete Mediation MCP Server (`aep-mcp-server`), and Kani / `dudect` / Ghidra Verification Suites. Free for academic research, formal verification, and non-production security auditing. |
| **Tier 3: Enterprise & Defense** | `aep-enterprise-enclave` | **Commercial Enterprise License** | Production `#![no_std]` Confidential VM Enclave Runtime (`aep-enclave`), `RP2350` Physical Power Interlock Firmware (`aep-firmware-rp2350`), Confidential Containers (CoCo) Trustee KBS Policies, PoPC Audit/Insurability Packs, and Enterprise SLAs (`$5,000–$8,000` per authority signing key / protected enclave per year). |

---

## 📜 Patent Portfolio Notice (USPTO Customer #225171)
AeP technology is patent-pending across **6 U.S. Provisional Patent Applications** (`63/939,634`; `63/940,806`; `63/948,978`; `63/948,999`; `63/962,519`; `63/999,618`), with the **U.S. Nonprovisional Omnibus Application** (`35 U.S.C. § 111(a)`) in preparation. Sole Inventor: **Stephen D. Smith**.

---

[Website](https://aep-labs.com) • [Enterprise & Academic Licensing](mailto:stephen@aep-labs.com)
