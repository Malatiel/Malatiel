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

A self-hosted CLI for testing business invariants across HTTP, Kafka and read-only database queries. Faultora imports OpenAPI and AsyncAPI descriptions and runs YAML scenarios with concurrency, retries and bounded faults. The [latest release](https://github.com/Malatiel/Faultora/releases/latest) includes Linux and Windows CLI archives.

The [payment example](https://github.com/Malatiel/Faultora/blob/v0.12.0/examples/payment-service/scenarios/fault-concurrent-duplicate.yaml) races two `create-payment` requests sharing one idempotency key while injected latency widens the race window. It checks that exactly one payment exists. The end-to-end suite requires the same scenario to fail against a deliberately broken variant of the API.

**Stack:** Java 21, Maven multi-module, Apache HttpClient 5, Jackson, Toxiproxy and JUnit 5.

Engineering highlights:

- Faults have a hard expiry while the Faultora process is running. Faultora reports failed or unconfirmed rollback as a run error. After a process kill, the operator may need to remove leftover Toxiproxy toxics.
- The built-in provider acts only on Faultora's own outbound requests, so fault injection needs no privileges over the target or its infrastructure; a Toxiproxy provider adds real network faults through the same SPI without changing the scenario format.
- Injected failures carry a `FAULT_` code that distinguishes them from target responses.
- Reports attribute nodes to fault windows by half-open interval overlap, stating which requests ran while a fault was active without claiming causation.
- Destination policy resolves DNS and pins the address per request, re-checks every redirect hop, rejects HTTPS-to-HTTP downgrades and strips credentials across origins.
- Scenarios refer to secret handles that Faultora resolves from the environment. The CLI run journal records evidence digests and omits raw response bodies.
- The run seed controls generated inputs and retry jitter. Request scheduling and timing can vary between runs.
- The MCP server returns preparation diagnostics and named assertion outcomes. Starting a run over MCP requires operator approval.
- An outbound runner for private networks commits run evidence to disk before sending it to the dispatcher.
- The CLI writes self-contained HTML reports. Releases ship checksums and a CycloneDX SBOM.

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
