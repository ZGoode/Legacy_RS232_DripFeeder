# Transfer and RS232 flows

## SD transfer integrity

```mermaid
flowchart LR
  subgraph WIN[Windows]
    L[Local file / destination]
    Q[TransferQueue<br/>normal transfers only]
    C[CRC32 calculation or verification]
    T[Temporary .downloading file]
    L <--> Q
    Q <--> C
    C <--> T
  end
  subgraph B[Plain HTTP boundary]
    X[Download / upload request]
  end
  subgraph ESP[ESP32]
    H[SD handlers]
    S[SD/FATFS]
    K[Persistent CRC cache]
    H <--> S <--> K
  end
  Q <--> X <--> H
  classDef windows fill:#dbeafe,stroke:#2563eb,color:#172554;
  classDef boundary fill:#fef3c7,stroke:#d97706,color:#78350f;
  classDef firmware fill:#dcfce7,stroke:#16a34a,color:#14532d;
  class L,Q,C,T windows;
  class X boundary;
  class H,S,K firmware;
```

## RS232 command path

```mermaid
flowchart LR
  subgraph WIN[Windows]
    UI[DeviceManagerWindow]
    E[DeviceProtocolEngine]
    R[Rs232ApiClient]
    UI <--> E <--> R
  end
  subgraph B[Plain HTTP boundary]
    HN[RS232 API]
  end
  subgraph ESP[ESP32]
    H[RS232 handlers]
    U[UART1 / RS232]
    X[External serial device]
    H <--> U <--> X
  end
  R <--> HN <--> H
  classDef windows fill:#dbeafe,stroke:#2563eb,color:#172554;
  classDef boundary fill:#fef3c7,stroke:#d97706,color:#78350f;
  classDef firmware fill:#dcfce7,stroke:#16a34a,color:#14532d;
  class UI,E,R windows;
  class HN boundary;
  class H,U,X firmware;
```

The direct SD text-editor transfer path bypasses `TransferQueue`; it therefore is not represented as queue-managed work in the first diagram.
