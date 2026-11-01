# Automotive Gateway — Architecture v0.2

Status: APPROVED_CONCEPTUALLY
Governance: BH-SEP v2.3 / BH-SDP v2.3
Contract: AGW-KEY-INTEGRATION-001

## Objective

Develop a cross-platform automotive diagnostics and
authorized key-transponder integration platform.

Deliver a ready-to-use application, gateway firmware,
supported hardware interfaces and guided workflows
that minimize manual work for locksmiths.

## Target platforms

- Linux
- macOS
- Windows
- Android
- iOS

Development host: Linux Mint.

## Architecture

1. Cross-platform application: Flutter.
2. Shared diagnostics and authorization core.
3. ESP32-S3 N16R8 gateway with ESP-IDF firmware.
4. Dedicated compatible transponder reader/writer.
5. Protected vehicle communication interface.
6. Deterministic simulators for host-first testing.
7. Audit, validation and human authorization layer.

## Key integration workflow

1. Establish legitimate operator authorization.
2. Identify vehicle and key characteristics.
3. Verify transponder compatibility.
4. Prepare compatible transponder using dedicated hardware.
5. Register key through supported authorized procedures.
6. Validate operation and generate an audit report.

Preparation and vehicle registration are separate
operations with independent compatibility gates.

## Security

Fail closed on missing authorization, incompatibility,
unsupported procedures or communication failures.

Do not bypass immobilizers, extract protected secrets,
disable anti-theft functions or circumvent authentication.

Critical operations require explicit human approval.

## Qualification

Documentary, simulated and physical qualification
must remain independently evaluated.

No simulated GREEN establishes physical compatibility.

## Current state

ARCHITECTURE_SCOPE=APPROVED_CONCEPTUALLY
ARCHITECTURE_DESIGN=IN_PROGRESS
IMPLEMENTATION=NOT_STARTED
HARDWARE_SELECTION=PARTIAL
TRANSPONDER_INTEGRATION=NOT_QUALIFIED
IMMOBILIZER_INTEGRATION=NOT_QUALIFIED
PHYSICAL_EXECUTION=NOT_AUTHORIZED

## Next milestone

AGW-M01: host-first architecture, shared interfaces,
platform compatibility matrix and deterministic
simulators without vehicle access.
