# Hlæja dependency

## Build Release Order

*need to make pipeline for this.*

Level 1

- hlaeja-version-catalog

Level 2

- hlaeja-core-plugin

Level 3

- hlaeja-common-plugin

Level 4

- hlaeja-common-messages
- hlaeja-jwt
- test-library

Level 5

- hlaeja-account-registry
- hlaeja-device-registry
- hlaeja-device-configuration
- hlaeja-device-data
- hlaeja-device-api
- hlaeja-registry-api
- hlaeja-management

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
    HDAS[Service] --> HDAR[(Redis)]
  end
  subgraph HRA[Hlæja Registry API]
    HRAS[Service]
  end
  subgraph HM[Hlæja Management]
    HMS[Service] -.-> HMR[(Redis)]
  end


  HM --> HAR
  HM --> HDR
  HM -.-> HDC

  HRA --> HAR
  HRA --> HDR

  HDA --> HDR
  HDA --> HDC
  HDA --> HDD
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
  TL -.-> DRS
  CML --> DRS
  JL --> DRS

  DDS[Device Data Service]
  PS --> DDS
  TL -.-> DDS
  CML --> DDS

  DCS[Device Configuration Service]
  TL -.-> DCS
  PS --> DCS
  CML --> DCS

  AS[Account Service]
  TL --> AS
  CML --> AS
  PS --> AS
  PCe --> AS
  JL --> AS

  DAS[Device API Service]
  PS --> DAS
  CML --> DAS
  JL --> DAS
  PCe --> DAS

  RAS[Registry API Service]
  CML --> RAS
  JL --> RAS
  PS --> RAS
  PCe --> RAS

  MUS[Management UI Service]
  CML --> MUS
  JL --> MUS
  PS --> MUS
  PCe -.-> MUS
```
