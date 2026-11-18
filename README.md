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

Faultora is a self-hosted reliability testing platform for APIs and distributed
systems. Run checks against staging or in CI to see whether your service keeps
its promises when requests overlap, clients retry, or dependencies fail. In
the [payment scenario](https://github.com/Malatiel/Faultora/blob/main/examples/payment-service/scenarios/fault-concurrent-duplicate.yaml),
two requests with one idempotency key must create exactly one payment.

Start from an OpenAPI contract, generate a YAML scenario, and preview it
without sending requests. Add parallel steps, retries, or bounded fault
injection. Inspect the results in an offline HTML report or use JSON and
JUnit output in CI.

- **Verify a workflow across components.** Exercise HTTP and Kafka and check
  the resulting state with read-only SQL queries.
- **Draft scenarios with an AI assistant.** Connect your MCP client to
  Faultora's local server to draft, validate, and preview scenario YAML.
- **Run checks inside a private network.** Deploy the separate runner with a
  compatible dispatcher.

[Download Faultora](https://github.com/Malatiel/Faultora/releases/latest) ·
[Run a check against your API](https://github.com/Malatiel/Faultora#first-ten-minutes) ·
[Run the payment example](https://github.com/Malatiel/Faultora/blob/main/examples/README.md#run-the-http-examples)

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
