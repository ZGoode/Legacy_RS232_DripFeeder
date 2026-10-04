# Architecture and boundaries

```mermaid
flowchart LR
  subgraph WIN[Windows workstation — FileLinkDiscovery]
    direction TB
    W[WPF windows and user actions]
    D[DiscoveryService<br/>mDNS lookup]
    A[DeviceApiClient<br/>device, SD, settings, OTA]
    Q[TransferQueue<br/>normal file transfers]
    P[DeviceProtocolEngine<br/>protocol/device validation]
    R[Rs232ApiClient<br/>raw serial API client]
    W <--> D
    W <--> A
    W <--> Q <--> A
    W <--> P <--> R
  end
  subgraph BOUNDARY[Network boundary]
    direction TB
    MDNS[mDNS<br/>_filelink._tcp.local.]
    HTTP[Plain HTTP / JSON<br/>plus streaming bodies]
  end
  subgraph ESP[ESP32 device — FileLink firmware]
    direction TB
    H[HTTP server and handlers]
    NET[Active interface<br/>W5500 Ethernet or Wi-Fi]
    SD[SD/FATFS]
    CRC[CRC cache background task]
    UART[UART1 / RS232]
    OTA[ESP-IDF OTA partitions]
    NVS[NVS<br/>device, auth, Wi-Fi settings]
    RTC[PCF8523 RTC / system clock]
    LED[NeoPixel status / identify]
    H <--> SD <--> CRC
    H <--> UART
    H <--> OTA
    H <--> NVS
    H <--> RTC
    H <--> LED
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
  class H,NET,SD,CRC,UART,OTA,NVS,RTC,LED firmware;
```

Blue is Windows-side processing. Amber is the network boundary. Green is ESP32 firmware and device-owned state. Bidirectional arrows represent request/response, read/write, or status-feedback relationships. The HTTP boundary is plain HTTP, not HTTPS.
