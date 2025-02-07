# Hlæja dependency

## Service dependency

```mermaid
graph TD
;
  subgraph BE[Backend Services]
    subgraph HDR[Hlæja Device Registry]
      HDRS[Service] --> HDRD[(Postgres)]
    end
    subgraph HDD[Hlæja Device Data]
      HDDS[Service] --> HDDD[(InfluxDB)]
    end
    subgraph HDC[Hlæja Device Configuration]
      HDCS[Service] --> HDCD[(Cassandra)]
    end
    subgraph HAR[Hlæja Account Registry]
      HARS[Service] --> HARD[(Postgres)]
    end
  end
  subgraph HDA[Hlæja Device API]
    HDAS[Service] --> HDAD[(Redis)]
  end
  subgraph HRA[Hlæja Registry API]
    HRAS[Service]
  end
  subgraph HM[Hlæja Management]
    HMS[Service]
  end

  HM --> HAR
  HM -.-> HDR
  HM -.-> HDC
  HDA --> HDR
  HDA --> HDC
  HDA --> HDD
  HRA --> HAR
  HRA --> HDR
```

## Library and Gradle plugin dependency

```mermaid
graph RL
;
  CP[Core Plugin]
  subgraph SCP [Common Plugin]
    PL[Plugin Library]
    PLM[Plugin Library Manifest]
    PLP[Plugin Library Publish]
    PCo[Plugin Common]
    PCoD[Plugin Common Detekt]
    PCoK[Plugin Common Ktlint]
    PCe[Plugin Certificate]
    PS[Plugin Service]
    PSC[Plugin Service Container]
    PSIT[Plugin Service Integration Test]
    PSPR[Plugin Service Process Resource]
  end
  PLM --> PL
  PLP --> PL
  PCo ---> PL
  PCoD --> PCo
  CP ---> PCo
  PCoK --> PCo
  PCo ---> PS
  PSC --> PS
  PSIT --> PS
  PSPR --> PS

  CML[Common Messages Library]
  PL --> CML

  JL[JWT Library]
  PL --> JL

  TL[Test Library]
  PL --> TL

  DRS[Device Registry Service]
  PS --> DRS
  PCe --> DRS
  CML --> DRS
  JL --> DRS

  DDS[Device Data Service]
  PS --> DDS
  CML --> DDS

  DCS[Device Configuration Service]
  PS --> DCS
  CML --> DCS

  DAS[Device API Service]
  CML --> DAS
  JL --> DAS
  PS --> DAS
  PCe --> DAS

  RAS[Registry API Service]
  CML --> RAS
  JL --> RAS
  PS --> RAS
  PCe --> RAS

  AS[Account Service]
  CML --> AS
  JL --> AS
  TL --> AS
  PS --> AS
  PCe --> AS

  MUS[Management UI Service]
  CML --> MUS
  JL --> MUS
  PS --> MUS
  PCe -.-> MUS

```
