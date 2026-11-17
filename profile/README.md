<div align="center">
  <p><img src="https://avatars.githubusercontent.com/u/281187319?s=240&v=4" width="120" alt="AeP Labs shield logo"/></p>
  <h1>AeP Labs — AeP Sentinel</h1>
  <p><strong>Physical Safety Switch for Autonomous AI Agents</strong></p>
  <p><em>Solely conceived and authored by founder Stephen D. Smith (AeP Labs) · Patent Pending (USPTO Customer #225171) · SAM.gov Active (CAGE 1A8N0 · UEI Q4AXQU1JK7A7)</em></p>
  <p>
    <img src="https://img.shields.io/badge/Website-aep--labs.com-0D9488?style=flat-square" alt="Website"/>
    <img src="https://img.shields.io/badge/Math-I64F64%20Fixed--Point-emerald?style=flat-square" alt="Deterministic Math"/>
    <img src="https://img.shields.io/badge/Formal%20Proofs-40%20Kani%20%7C%209%20dudect%20%7C%201%2C273%20Tests-blueviolet?style=flat-square" alt="Formal Verification"/>
    <img src="https://img.shields.io/badge/Hardware%20TEEs-Nitro%20%7C%20SEV--SNP%20%7C%20TDX%20%7C%20Hopper%20%2F%20B200%20CC-0052CC?style=flat-square" alt="Hardware TEEs"/>
    <img src="https://img.shields.io/badge/Edge%20Interlock-RP2350%20(~%2421%20BOM)-orange?style=flat-square" alt="Edge Interlock"/>
    <img src="https://img.shields.io/badge/License-Apache%202.0%20%7C%20BUSL--1.1%20%7C%20Commercial-blue?style=flat-square" alt="3-Tier License"/>
  </p>
</div>

---

## 🔒 Your AI Agent Cannot Spend or Act Without Hardware Permission

**AeP Sentinel** locks each autonomous AI agent's cryptographic signing key (`ECDSA P-256/P-384`, `Ed25519`, `ML-DSA-87`) inside an isolated hardware chip — cloud hardware Trusted Execution Environments or a `~$21` bare-metal edge microcontroller (`RP2350`) — and checks cumulative behavior over time in deterministic 128-bit `I64F64` fixed-point arithmetic (`<50 µs` Phase I target) before signing any action.

- **Unexported Hardware Signing Key**: Generated inside volatile enclave or microcontroller RAM and never exported; relying APIs pin the attested public key. A compromised host OS can deny, delay, or reorder calls, but cannot inspect volatile memory, extract the key, or obtain out-of-policy signatures.
- **Catches Slow, Multi-Step Drains**: Static hardware/MPC signers enforcing single-call caps and static allowlists (e.g., Turnkey on AWS Nitro, Fireblocks) approve 49 consecutive `$4,800` transfers (`$235.2k` drained) under a `$5,000` single-call limit. AeP Sentinel evaluates stateful sequential drift (`OU + GARCH(1,1) + Page's CUSUM`) over RFC 8785 canonicalized payloads into 5 rulings (`ALLOW`, `MODIFY`, `STEP_UP`, `DEFER`, `DENY`) — with zero second-AI judges and zero model weights required.
- **Cryptographic Proof of Consequence**: On `DENY`, the chip latches authority, zeroizes the volatile signing key, emits attested evidence of key zeroization and a fresh-nonce-bound Proof of Physical Consequence (`PoPC`) state-delta receipt, and commits to a tamper-evident RFC 6962 (`SHA-256 / SHA-384 / SHA3-256`) audit trail without storing raw prompts.

---

## ⚙️ Supported Hardware & Trust Boundaries

| Platform | Hardware Target | Trust Boundary |
|---|---|---|
| **AWS Nitro Enclaves** | `c6i.xlarge` / `c7i.16xlarge` | Isolates volatile key custody from the parent EC2 OS (trusts the AWS Nitro hypervisor). |
| **AMD EPYC SEV-SNP** | AWS SEV-SNP · Azure `Standard_DC2as_v5` · GCP `N2D` | Hardware-encrypted Confidential VM memory protecting against a hostile cloud hypervisor. |
| **Intel Xeon TDX** | DCAP Quote v4 (`ConfigFS-TSM` / `/dev/tdx_guest`) | Hardware-encrypted Confidential VM memory protecting against a hostile cloud hypervisor. |
| **NVIDIA Hopper / Blackwell CC** | H100 / B200 Confidential Computing (`SPDM 1.2` PCIe) | Enterprise GPU Confidential Computing attestation (local dev simulations on consumer GPUs lack CC fuses). |
| **Edge Physical Interlock** | `RP2350 + INA260 + PROFET+ + TPL5010 + ISO7721` (`~$21` BOM) | Bare-metal hardware safety switch cutting payload power (`<1 ms` Phase I target) while logging electrical `ΔI` `PoPC` receipts. |

---

## 📊 Measured TRL 4 Baselines vs. Phase I Hardware Targets

* **Deterministic 128-Bit `I64F64` Math (`10^7` steps vs. 106-bit Dekker/Knuth Double-Double)**: `2.93 × 10⁻¹⁷` max OU error (~324× tighter than IEEE-754 `f64`), `2.04 × 10⁻¹⁹` max exogenous `GARCH(1,1)` error (~5,309× tighter), and `4.76 × 10⁻¹⁵` max endogenous `GARCH(1,1)` error (~239× tighter).
* **Software Stream Severance (Measured · Local OS Loopback TCP)**: **`0.62 ms` (`0.6188 ms`) p99** (`0.4768 ms` p50) across **24 adversarial streams** with **`0/12` false halts** across concurrent benign streams (`1.73 ms` [`1.7258 ms`] p99 with synchronous disk `PoPC` persistence; `simulation=true`, outside a hardware TEE).
* **Automated & Formal Assurance**: **1,273 static automated test definitions** across Rust, Python, TypeScript, and simulation suites; **40 Kani bounded model-checking harnesses** (`26` fixed-point math, `6` enclave runtime, `8` TEE attestation checks); **9 user-space `dudect` timing suites** (`20,000` samples/class, CI gate `|t| < 10`; no detected timing leakage); and machine-code disassembly audits verifying volatile key-wipe retention.
* **Phase I Targets**: `<50 µs` full in-enclave mediation score target; `<1 ms` COTS `RP2350` payload-power cutoff target; bare-metal SEV-SNP / TDX speed benchmarking and adaptive sleeper-agent evaluations scoped for prospective Phase I independent testing with **George Mason University (SECSAT Lab)** (no sponsored research agreement executed; zero background-IP ownership, code authorship, or license).

---

## ⚖️ 3-Tier Open-Core Architecture

| Tier | License | Functional Scope |
|---|---|---|
| **Tier 1: Developer Commons & Verifiers** | **`Apache-2.0`** | Client SDKs (`aep-sdk` / `@aep/sdk` preview kits), RFC 8785 JCS canonicalization helpers, Envoy/MCP gateway policy filters, and standalone multi-cloud hardware quote & `PoPC` receipt verifiers (`Apache-2.0 § 3` grants zero implied patent license to Tier 2 or Tier 3 runtimes). |
| **Tier 2: Protocol Core & Risk Engine** | **`BUSL-1.1`** *(Source-Available)* | `zero-std / bare-metal` deterministic fixed-point sequential behavioral risk kernel (`OU + GARCH(1,1) + Page's CUSUM`), hardware TEE quote & attestation drivers, tamper-evident Merkle `PoPC` audit journal, streaming containment server, and formal/timing verification suites. |
| **Tier 3: Enterprise Platform & Hardware Interlock** | **Proprietary Commercial** | Production Confidential VM enclave custodian runtime (holding unexported volatile signing keys), bare-metal `RP2350` physical power interlock firmware, `PoPC` compliance/insurability packs, and enterprise SLAs (**$5,000–$8,000 per authority signing key / protected enclave per year**; target **$50k/yr** 10-key pilots scaling to **$250k+/yr** enterprise deployments). |

> **Pre-Filing Visibility Posture:** All code repositories in the `AeP-Agent-Economic-Protocol` organization remain `Private` through USPTO Nonprovisional Omnibus Patent Application filing confirmation.

---

## 🔗 Canonical Resources (`aep-labs.com`)

* **[System Overview & Interactive Architecture](https://aep-labs.com/)**: Plain-English overview, 3-step containment stage, 10-line gateway/SDK setup, and honest threat limits.
* **[Interactive Browser Verifier](https://aep-labs.com/verify)**: Live in-browser tests for RFC 8785 duplicate-key tampering, genuine AWS Nitro `COSE_Sign1` receipts, tamper-evident audit logs, and `RP2350` power cutoff proofs.
* **[Speed & Proof Benchmarks](https://aep-labs.com/benchmarks)**: Disaggregated TRL 4 measurements vs. Phase I targets, `1,273` tests, `40` Kani proofs, and `9` `dudect` suites.
* **[Research & Prospective Phase I Agenda](https://aep-labs.com/research)**: AeP Sentinel open research agenda and prospective Phase I GMU SECSAT Lab evaluation scope.
* **[Government Capability Statement](https://aep-labs.com/federal)**: Printable 1-page federal brief (`CAGE 1A8N0` · `UEI Q4AXQU1JK7A7` · `NAICS 541715` · `DFARS 252.227-7014/7015`).
* **[Security Disclosure Policy](https://aep-labs.com/disclosure)**: RFC 9116 manifest (`/.well-known/security.txt`), 3-step metadata-first encrypted handshake, and good-faith safe harbor (zero implied patent license).

---

<div align="center">
  <sub><strong>AeP Labs</strong> · 5191 78th St Cir E, Bradenton, FL 34203 · SAM.gov UEI <code>Q4AXQU1JK7A7</code> · CAGE <code>1A8N0</code> · Contact: <a href="mailto:stephen@aep-labs.com">stephen@aep-labs.com</a></sub>
  <br/>
  <sub>Sole Inventor &amp; Code Author: Stephen D. Smith · Patent Pending (6 U.S. Provisional Applications + Nonprovisional Omnibus in preparation; USPTO Customer #225171) · Prospective NIST AISIC black-box candidate (no CRADA executed; zero IP license granted)</sub>
</div>
