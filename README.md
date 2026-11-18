# Alexey N.

**Lead Java Engineer · FinTech · Payments · Core Banking**

I build backend systems where correctness, recoverability and predictable failure modes matter: real-time FX, cross-border payments, commissions and event-driven payment routing.

At **Innotech**, I work on:

- an FX engine covering rate calculation, order lifecycle and settlement;
- a configurable commission engine for transfers and account products;
- event-driven payment routing with compliance validation.

**Production stack:** Java 21, Spring Boot 3, Kafka, ActiveMQ Artemis, PostgreSQL, Oracle and Kubernetes.

Previously, I spent four and a half years at **Sovcombank**, first as a system analyst and later as a backend developer for OSAGO and CASCO insurance products.

Most of my professional work is private, so this profile focuses on side projects where I can show the engineering decisions behind the code — Faultora is the closest to what I do daily.

## Selected projects

### [Faultora](https://github.com/Malatiel/Faultora)

I build Faultora, a self-hosted Java 21 CLI for checking what happens when
staging APIs receive concurrent requests, retries, or injected failures.
The [payment example](https://github.com/Malatiel/Faultora/blob/main/examples/payment-service/scenarios/fault-concurrent-duplicate.yaml)
sends two requests with one idempotency key and checks that only one payment
is created.

Generate a YAML scenario from OpenAPI, preview it without contacting your
service, and run it locally or in CI. Faultora checks outcomes across HTTP,
Kafka, and read-only database queries and saves HTML, JSON, and JUnit reports.

[Download for Windows or Linux](https://github.com/Malatiel/Faultora/releases/latest) ·
[First ten minutes](https://github.com/Malatiel/Faultora#first-ten-minutes)

**I'm looking for testers.** Try the [HTTP examples](https://github.com/Malatiel/Faultora/blob/main/examples/README.md#run-the-http-examples)
or bring one staging API.
If setup stalls, a report is unclear, or a check is missing,
[tell me what happened](https://github.com/Malatiel/Faultora/issues/new/choose).
The Faultora version, your OS, and a small sanitized example are enough to start.

### [Rayline for macOS](https://github.com/Malatiel/rayline-macos)

A native SwiftUI proxy client supporting VLESS, VMess, Shadowsocks and Trojan through a pinned and SHA256-verified sing-box backend.

The project deliberately uses the macOS SOCKS5 system proxy instead of a TUN interface. That keeps its scope clear, but makes safe system-state management essential.

Engineering highlights:

- Snapshots the existing proxy configuration before connecting and restores it on disconnect.
- Recovers system settings after crashes or force quits and removes orphaned sing-box processes.
- Keeps the original snapshot for the entire connection lifecycle, preventing Rayline from restoring its own stale proxy configuration.
- Verifies connectivity with an end-to-end request through the tunnel; a successful SOCKS5 handshake alone is not treated as proof of a working connection.
- Stores profiles and generated configuration with `0600` permissions.
- Calls `networksetup` without a shell and sanitizes exported diagnostics to remove credentials, UUIDs and local paths.

### [Tg-Bot-Gpt](https://github.com/Malatiel/Tg-Bot-Gpt)

A Telegram bot built on the OpenAI API with per-user billing, Telegram Stars payments and encrypted chat history.

**Stack:** Java 22, Spring Boot 3, PostgreSQL, Flyway, WebFlux and Docker.

Engineering highlights:

- Free, Pro and owner plans with monthly token and message limits.
- Seven-day trials with automatic expiry and downgrade.
- Native Telegram Stars invoices and idempotent refund handling.
- Conversation history trimmed by token budget instead of message count.
- Fail-closed access control: an empty whitelist permits only the owner.
- AES-256-GCM encryption with an explicit startup failure when encryption is required but unavailable; existing legacy rows remain readable.
- Explicit handling of model capabilities and model-specific API parameters.
- Ordered shutdown: polling stops first, in-flight work drains, and only then does the HTTP client close.
- Untrusted document content is isolated and checked for common prompt-injection patterns, while the limitation of this heuristic is documented rather than presented as a security boundary.

## Links

- [LinkedIn](https://www.linkedin.com/in/alexeynag)
