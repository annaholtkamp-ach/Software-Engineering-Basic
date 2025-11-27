# Software-Engineering-Basic
Master Course Software Engineering Basic

Very simplified version 
```mermaid
flowchart LR

  customer["Customer"]
  channel["Voice channel or partner app"]
  backend["Voice platform backend"]
  voiceAI["Voice AI engine (ASR, NLU, TTS)"]
  apis["Partner and backend APIs"]
  storage["Data and feature storage"]
  observability["Monitoring and observability"]

  customer --> channel
  channel --> backend
  backend --> voiceAI
  voiceAI --> backend
  backend --> apis
  backend --> storage
  backend --> observability
  voiceAI --> channel
  channel --> customer
```
Simplified but clearer client server database structure

```mermaid
flowchart LR

  %% Client layer
  subgraph Client
    customer["Customer"]
    channel["Voice channel or partner app"]
  end

  %% Server layer
  subgraph Server["Voice platform (server side)"]
    api["HTTP or gRPC API"]
    voiceAI["Voice AI engine (ASR, NLU, TTS)"]
    logic["Business logic and integrations"]
  end

  %% Data layer
  subgraph Data["Databases and storage"]
    metaDB["Metadata database"]
    audioStore["Audio file storage"]
    featureStore["Feature store"]
  end

  %% Client to server
  customer --> channel --> api

  %% Inside server
  api --> voiceAI
  api --> logic
  voiceAI --> api
  logic --> api

  %% Server to data
  api --> metaDB
  api --> audioStore
  voiceAI --> featureStore

  %% Responses back to client
  api --> channel --> customer
```

Simplified but clearer client server database structure + observability 

```mermaid
flowchart LR

  %% Client layer
  subgraph Client
    customer["Customer"]
    channel["Voice channel or partner app"]
  end

  %% Server layer
  subgraph Server["Voice platform (server side)"]
    api["HTTP or gRPC API"]
    voiceAI["Voice AI engine (ASR, NLU, TTS)"]
    logic["Business logic and integrations"]
  end

  %% Data layer
  subgraph Data["Databases and storage"]
    metaDB["Metadata database"]
    audioStore["Audio file storage"]
    featureStore["Feature store"]
  end

  %% Observability layer
  subgraph Observability["Monitoring and observability"]
    telemetry["Telemetry collector"]
    dashboards["Dashboards and alerts"]
  end

  telemetry --> dashboards

  %% Client to server
  customer --> channel --> api

  %% Inside server
  api --> voiceAI
  api --> logic
  voiceAI --> api
  logic --> api

  %% Server to data
  api --> metaDB
  api --> audioStore
  voiceAI --> featureStore

  %% Responses back to client
  api --> channel --> customer

  %% Telemetry from server and data
  api --> telemetry
  voiceAI --> telemetry
  logic --> telemetry
  metaDB --> telemetry
  audioStore --> telemetry
  featureStore --> telemetry
```
With API Specifications 

```mermaid
flowchart LR

  %% Client layer
  subgraph Client
    customer["Customer"]
    channel["Voice channel or partner app"]
  end

  %% Server layer
  subgraph Server["Voice platform (server side)"]
    api["HTTP or gRPC API"]
    voiceAI["Voice AI engine (ASR, NLU, TTS)"]
    logic["Business logic and integrations"]
  end

  %% Data layer
  subgraph Data["Databases and storage"]
    metaDB["Metadata database"]
    audioStore["Audio file storage"]
    featureStore["Feature store"]
  end

  %% Observability layer
  subgraph Observability["Monitoring and observability"]
    telemetry["Telemetry collector"]
    dashboards["Dashboards and alerts"]
  end

  telemetry --> dashboards

  %% Public API endpoints
  subgraph API_Endpoints["Public API endpoints"]
    epIngest["POST /api/v1/voice/ingest"]
    epGovernance["POST /api/v1/voice/governance"]
    epInteract["POST /api/v1/voice/interaction"]
    epRecording["GET /api/v1/voice/recordings/{id}"]
    epHealth["GET /api/v1/health"]
  end

  %% Client to server
  customer --> channel --> api

  %% Inside server
  api --> voiceAI
  api --> logic
  voiceAI --> api
  logic --> api

  %% Server to data
  api --> metaDB
  api --> audioStore
  voiceAI --> featureStore

  %% Responses back to client
  api --> channel --> customer

  %% Telemetry from server and data
  api --> telemetry
  voiceAI --> telemetry
  logic --> telemetry
  metaDB --> telemetry
  audioStore --> telemetry
  featureStore --> telemetry

  %% Endpoints mapped to the main API component
  epIngest --> api
  epGovernance --> api
  epInteract --> api
  epRecording --> api
  epHealth --> api
```

Whithout AI Engine specification 
```mermaid
flowchart LR

  %% Clients
  subgraph Clients
    partner["Partner systems / Voice agents"]
  end

  %% Edge / offline mode
  subgraph Edge["On-device Lite Voice Agent"]
    edgeASR["Lite ASR and NLU (on-device)"]
    edgeQueue["Local FIFO queue and encrypted cache"]
    edgeASR --> edgeQueue
  end

  partner -->|"HTTPS or gRPC (audio and metadata)"| ingestionAPI
  edgeQueue -->|"Sync on reconnect"| ingestionAPI

  %% Core backend
  subgraph Backend
    ingestionAPI["Ingestion API (FastAPI)"]
    mq["Message broker (RabbitMQ)"]
    workers["Extraction workers (Celery)"]
    voiceEngine["Voice AI Engine (ASR, Prosody, Timbre, Emotion)"]
    governance["Voice Governance Service"]
    rawStore["Raw audio storage (S3)"]
    metaDB["Metadata DB (PostgreSQL)"]
    featureStore["Feature Store (Layer embeddings)"]
  end

  ingestionAPI -->|"auth and validate"| mq
  ingestionAPI -->|"store metadata"| metaDB
  mq --> workers
  workers --> rawStore
  workers --> voiceEngine
  voiceEngine --> featureStore
  governance -->|"consent and retention"| metaDB

  %% Compute backends (GPU & INT)
  subgraph Compute["Compute backends"]
    gpu["GPU nodes"]
    intNode["INT nodes"]
  end

  voiceEngine --> gpu
  voiceEngine --> intNode

  %% Observability
  subgraph Observability
    otel["OpenTelemetry SDK / Collector"]
    dash0["dash0 observability platform"]
  end

  ingestionAPI --> otel
  workers --> otel
  voiceEngine --> otel
  governance --> otel
  otel --> dash0
```

Whith AI Engine specification 

```mermaid
flowchart LR

  %% Clients
  subgraph Clients
    customer["Customer"]
    partner["Voice channel or partner platform"]
  end

  %% Edge / offline mode
  subgraph Edge["On-device Lite Voice Agent"]
    edgeASR["Lite ASR and NLU on device"]
    edgeQueue["Local FIFO queue and encrypted cache"]
    edgeASR --> edgeQueue
  end

  customer --> partner
  partner -->|"HTTPS or gRPC (audio and metadata)"| ingestionAPI
  edgeQueue -->|"Sync on reconnect"| ingestionAPI

  %% Core backend
  subgraph Backend
    ingestionAPI["Ingestion API (FastAPI)"]
    mq["Message broker (RabbitMQ)"]
    workers["Extraction workers (Celery)"]
    governance["Voice Governance Service"]
    rawStore["Raw audio storage (S3)"]
    metaDB["Metadata DB (PostgreSQL)"]
    featureStore["Feature Store (Layer embeddings)"]

    subgraph VoiceAI["Voice AI Engine (runtime detail)"]
      asr["ASR (speech to text)"]
      nlu["NLU or LLM (intent and context)"]
      dialog["Dialog orchestrator (business logic)"]
      tts["TTS (brand voice synthesis)"]
    end
  end

  %% Backend data flow
  ingestionAPI -->|"auth and validate"| mq
  ingestionAPI -->|"store metadata"| metaDB
  mq --> workers
  workers --> rawStore
  workers --> asr
  asr --> nlu --> dialog --> tts
  tts --> featureStore

  %% Runtime voice loop
  partner --> asr
  tts --> partner
  partner --> customer

  %% External systems used by dialog
  backendAPIs["Partner and backend APIs (Shopify, internal systems)"]
  dialog -->|"read and write data"| backendAPIs

  %% Compute backends
  subgraph Compute["Compute backends"]
    gpu["GPU nodes"]
    intNode["INT nodes"]
  end

  asr --> gpu
  nlu --> gpu
  tts --> intNode

  %% Observability
  subgraph Observability
    otel["OpenTelemetry SDK or Collector"]
    dash0["dash0 observability platform"]
  end

  ingestionAPI --> otel
  workers --> otel
  asr --> otel
  nlu --> otel
  dialog --> otel
  tts --> otel
  governance --> otel
  otel --> dash0
  ```
