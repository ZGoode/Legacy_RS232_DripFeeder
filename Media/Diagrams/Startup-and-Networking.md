# Startup and networking

```mermaid
flowchart TD
  A[app_main] --> B[NVS and peripheral initialization]
  B --> C[W5500 Ethernet initialization]
  C --> D{Ethernet link and IP available?}
  D -->|Yes| E[Select Ethernet]
  D -->|No| F[Initialize Wi-Fi]
  F --> G{Primary slot-0 credentials connect?}
  G -->|Yes| H[Select Wi-Fi]
  G -->|No| I[Start open provisioning AP]
  I --> J[Save submitted credentials]
  J --> K[Restart]
  E --> L[Initialize SD and CRC cache]
  H --> L
  L --> M[Arm Network Fallback]
  M --> N[Start HTTP server]
  N --> O[Advertise mDNS]
```

This diagram is intentionally directional: it is a lifecycle sequence, not a request/response map. Ethernet is always attempted first. The Network Fallback setting affects runtime behavior after startup, not that initial preference.
