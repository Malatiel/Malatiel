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

### Links

LinkedIn: [in/alexeynag](https://www.linkedin.com/in/alexeynag)
