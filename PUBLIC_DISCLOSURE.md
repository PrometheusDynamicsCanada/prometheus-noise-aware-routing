# Public Disclosure & Intellectual Property Boundary

This repository provides primary hardware measurement evidence while preserving Prometheus Dynamics' intellectual property boundary.

## Included
- Unrouted logical workloads (`*.qasm` where supplied)
- Experimental descriptions, aggregate tables, and figures
- IBM Quantum Job IDs and file-integrity checksums (`IBM_JOB_EVIDENCE_INDEX.csv`)
- Returned IBM `SamplerV2` result payloads (`job-*-result.json`) and decoded bitstring counts

## Excluded
- Proprietary hardware-aware optimization software, compilation and execution-control implementations, and internal parameters (Prometheus uses circuit structure and device calibration data to guide compilation and execution)
- Routed physical OpenQASM circuits and physical qubit coordinate mappings
- Serialized IBM request payloads, account identifiers, and full request-embedded ZIP bundles

Removal of request metadata does not alter returned hardware result payloads. All published measurements remain offline-verifiable directly from the archived `job-*-result.json` files.
