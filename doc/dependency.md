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
      direction LR
      HARS[Service] --> HARD[(Postgres)]
      HARS[Service] --> HDRK[/KAFKA\]
    end
  end
  subgraph HDA[Hlæja Device API]
    HDAS[Service] --> HDAR[(Redis)]
  end
  subgraph HRA[Hlæja Registry API]
    HRAS[Service]
  end
  subgraph HM[Hlæja Management]
    direction LR
    HMS[Service] --> HMR[(Redis)]
    HMK[/KAFKA\] --> HMS[Service]
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

## Gradle Plugin Dependency

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
    PUS[Plugin UI Service]
    PUSTM[Plugin UI Service Thymeleaf Minify]
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

  PUSTM --> PUS
  PS --> PUS

  CL[Common Library]
  PL ---> CL

  CS[Common Service]
  PCe -.-> CS
  PS --> CS

  CUS[Common UI Service]
  PUS --> CUS
  PCe -.-> CUS
```

## Library And Gradle Plugin Dependency

```mermaid
graph RL
;
  
  HGP[Hlaeja Gradle Plugin]

  CML[Common Messages Library]
  HGP --> CML
  
  JL[JWT Library]
  HGP --> JL
  
  TL[Test Library]
  HGP --> TL

  DRS[Device Registry Service]
  HGP --> DRS
  TL --> DRS
  CML --> DRS
  JL --> DRS
    
  DDS[Device Data Service]
  HGP --> DDS
  TL -.-> DDS
  CML --> DDS
  
  DCS[Device Configuration Service]
  TL -.-> DCS
  HGP --> DCS
  CML --> DCS
  
  AS[Account Registry Service]
  TL --> AS
  HGP --> AS
  CML --> AS
  JL --> AS
  
  DAS[Device API Service]
  CML --> DAS
  JL --> DAS
  HGP --> DAS
  
  RAS[Registry API Service]
  CML --> RAS
  JL --> RAS
  HGP --> RAS
  
  MUS[Management UI Service]
  CML --> MUS
  JL --> MUS
  HGP --> MUS
```
