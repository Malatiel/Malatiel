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

A self-hosted CLI that tests API reliability claims instead of assuming them: it imports an OpenAPI description, runs a YAML scenario, injects bounded faults and asserts the business invariants that must survive them.

The flagship scenario races two concurrent `create-payment` requests sharing one idempotency key while injected latency widens the race window, then asserts that exactly one payment exists. The end-to-end suite runs the same scenario against a deliberately broken variant of the example API and requires it to fail there, so the test proves it can detect the defect it claims to cover.

**Stack:** Java 21, Maven multi-module, Apache HttpClient 5, Jackson, Toxiproxy and JUnit 5.

Engineering highlights:

- Every injected fault carries a hard expiry: rollback runs exactly once, whichever fires first — the stop step, the expiry watchdog on a daemon thread, or the end-of-run sweep in a `finally` block. A crashed or hung scenario cannot leave the environment degraded.
- The built-in provider acts only on Faultora's own outbound requests, so fault injection needs no privileges over the target or its infrastructure; a Toxiproxy provider adds real network faults through the same SPI without changing the scenario format.
- Injected failures surface as normalized errors prefixed `FAULT_`, so evidence from a fault can never be misread as target behavior.
- Reports attribute nodes to fault windows by half-open interval overlap, stating which requests ran while a fault was active without claiming causation.
- Destination policy resolves DNS and pins the address per request, re-checks every redirect hop, rejects HTTPS-to-HTTP downgrades and strips credentials across origins.
- Secrets are passed as handles resolved from the environment; they never reach scenario files, reports or diagnostics, and the run journal stores evidence digests rather than raw response bodies.
- Retry backoff jitter is derived from the run seed, so a failing run replays with the same delays.
- Reproducibility is scoped honestly: a seed and manifest reproduce the same plan and generated inputs, not bit-identical timing.
- Reports are self-contained HTML with no CDN, scripts or telemetry, and releases ship checksums, a CycloneDX SBOM and third-party license attribution.

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
