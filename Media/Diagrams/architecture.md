# Architecture diagram

```mermaid
flowchart LR
  subgraph WIN[Windows workstation — FileLinkDiscovery]
    direction TB
    W[WPF UI]
    D[DiscoveryService]
    A[DeviceApiClient]
    Q[TransferQueue]
    P[DeviceProtocolEngine]
    R[Rs232ApiClient]
    W <--> D
    W <--> A
    W <--> Q <--> A
    W <--> P <--> R
  end

  subgraph B[Network boundary]
    MDNS[mDNS discovery]
    HTTP[Plain HTTP / JSON]
  end

  subgraph ESP[ESP32 device — FileLink firmware]
    direction TB
    H[HTTP server]
    NET[W5500 Ethernet / Wi-Fi]
    S[SD/FATFS]
    C[CRC cache task]
    U[UART1 / RS232]
    O[OTA partitions]
    N[NVS settings]
    H <--> S <--> C
    H <--> U
    H <--> O
    H <--> N
    H <--> NET
  end

  D <-->|mDNS queries / responses| MDNS
  MDNS <-->|advertisement / resolution| NET
  A <--> HTTP <--> H
  R <--> HTTP

  classDef windows fill:#dbeafe,stroke:#2563eb,color:#172554;
  classDef boundary fill:#fef3c7,stroke:#d97706,color:#78350f;
  classDef firmware fill:#dcfce7,stroke:#16a34a,color:#14532d;
  class W,D,A,Q,P,R windows;
  class MDNS,HTTP boundary;
  class H,NET,S,C,U,O,N firmware;
```

Blue: Windows-side components. Amber: network boundary. Green: device-owned firmware and peripherals. Bidirectional arrows represent request/response or read/write interaction. HTTP is plain HTTP in the supplied implementation; no global endpoint authorization layer is visible.
