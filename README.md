## Alexey N.

Lead Java Developer at Innotech. Backend for banks: real-time FX, cross-border payments, core banking.

Currently: an FX engine (rate calculation, order lifecycle, settlement), a configurable commission engine for transfers and account products, and event-driven payment routing with compliance validation. Java 21, Spring Boot 3, Kafka, ActiveMQ Artemis, PostgreSQL and Oracle, Kubernetes.

Four and a half years at Sovcombank before that, first as a system analyst, then as a developer on OSAGO and CASCO insurance backends.

Almost everything I write at work is private, so this profile is side projects.

### Projects

[rayline-macos](https://github.com/Malatiel/rayline-macos) — macOS proxy client (SwiftUI) for VLESS, VMess, Shadowsocks and Trojan. Runs sing-box as the protocol backend rather than shipping a hand-rolled one.

What's interesting engineering-wise: it's a desktop app that handles credentials and rewrites system state, so most of the work went into not leaving the machine broken. It routes by setting the system SOCKS5 proxy rather than creating a TUN interface — a real scope limit, documented as one rather than glossed over.

Before enabling the proxy the app snapshots each network service's existing settings and restores them on disconnect; after a crash or force quit, startup recovery restores them and kills orphaned sing-box processes. The subtle failure was reconnecting after a drop that had deliberately left the proxy up: the fresh snapshot recorded Rayline's own proxy as the state to restore, so a later disconnect stranded the user on a dead endpoint. A snapshot is now taken once and never overwritten.

Connection health turned out to be similarly deceptive — sing-box answers a SOCKS5 CONNECT before it has dialled the server, so a completed handshake proves nothing. The health check sends a request through the tunnel and requires a response back; the first version passed against a server that didn't exist.

sing-box is pinned to a release tag and SHA256-verified on download. Profiles and the generated config are written to ~/.rayline/ at 0600. networksetup is called through Process with an argument array and no shell, so service names can't inject commands. Diagnostics exports strip UUIDs, passwords and local paths before writing to disk.

[Tg-Bot-Gpt](https://github.com/Malatiel/Tg-Bot-Gpt) — Telegram bot on the OpenAI
API with per-user billing, Telegram Stars payments and encrypted chat history.
Java 22, Spring Boot 3, PostgreSQL, Flyway, WebFlux, Docker.

What's interesting engineering-wise: every message costs real money and the bot
sits on someone else's rate limits, so most of the work went into the two places
that quietly leak — metering and access. Free/Pro/owner plans with monthly token
and message ceilings, a seven-day trial that downgrades itself on expiry, native
Stars invoices with idempotent refund handling, and per-user history trimmed to a
token budget rather than a message count.

Access control is fail-closed: an empty whitelist means owner-only, not
open-to-all. That is the opposite of the convenient default and it is the reason
a misconfigured deploy is useless instead of expensive; the bot logs its
resulting posture at startup so the state is visible rather than assumed.
Encryption is the same shape — AES-256-GCM with the IV prepended to the
ciphertext, and ENCRYPTION_REQUIRED makes startup fail rather than let a caller
silently persist plaintext. Legacy rows stay readable, so turning it on for an
existing database is safe.

The deceptive failure was model capability. Image support was decided by
prefix-matching the model name, so when a newer model family was added, a user
who had switched to it and sent a photo was silently moved back to the default
model — no error, just a quietly worse answer they had not asked for and were
still billed for. Its sibling was worse: the newer reasoning models reject any
temperature but their own, and the bot sends 0.7. Every request would have
failed with a 400, and the test suite could not have caught it — the tests mock
the OpenAI client, so 283 green tests prove the model name flows through the
code, not that the API accepts it.

Shutdown is ordered rather than assumed: polling stops first, in-flight tasks
drain, and only then does the HTTP client close — closing it first fails every
reply already being written. Prompt-injection handling is deliberately labelled
a heuristic and not a boundary: untrusted document text is wrapped with explicit
instructions and common override phrasing is matched in English and Russian, but
it is a speed bump, and calling it a defence would be the lie.

### Links

LinkedIn: [in/alexeynag](https://www.linkedin.com/in/alexeynag)
