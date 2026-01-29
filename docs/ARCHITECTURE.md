# AI-PMO System Architecture

## Overview

AI-PMO is built on an **Orchestrator-Worker pattern** where specialized AI agents work in perfect coordination to manage projects intelligently.

## Core Architecture Pattern

```
┌─────────────────────────────────────┐
│    PMO Director Agent (Orchestrator)│
│         (Claude Opus)               │
└────┬────────┬────────┬────────┬─────┘
     │        │        │        │
     ▼        ▼        ▼        ▼
  Planning  Monitor  Financial Knowledge
  Agent     Agent    Agent     Agent
```

### Key Characteristics

- **Asynchronous Communication**: Agents don't wait for each other
- **Event-Driven**: Triggered by data changes or user requests
- **Resilient**: Fallback mechanisms for agent failures
- **Scalable**: Add agents without modifying core architecture

## The 5 AI Agents

### 1. PMO Director Agent (Orchestrator)
**Role**: Central coordinator and decision maker

**Technology**: Claude Opus (complex reasoning)

**Capabilities**:
- Smart task distribution based on project state
- Result synthesis from multiple agents
- Error handling and fallbacks
- Project context awareness
- Risk assessment across all domains

**Integration Points**:
- Routes requests to all 4 worker agents
- Synthesizes responses before returning to client
- Handles escalations and critical decisions

### 2. Planning Agent
**Role**: Schedule optimization and resource allocation

**Specialization**: Historical data analysis for realistic estimates

**Capabilities**:
- Schedule optimization algorithms
- Resource allocation logic
- Dependency analysis and critical path calculation
- Video Fact benchmark integration
- Historical productivity data analysis
- Estimate refinement based on actual performance

**Data Sources**:
- Video Fact module (actual work hours vs. planned)
- GPS/GLONASS data (equipment movement patterns)
- Historical project data
- 1С:PM system (project structure)

### 3. Monitoring Agent
**Role**: Real-time progress tracking and deviation detection

**Key Function**: Early warning system for project issues

**Capabilities**:
- Real-time KPI monitoring
- Deviation detection algorithms
- Alert generation with severity levels
- GPS/GLONASS data stream processing
- Video Fact stream processing
- Dashboard metrics calculation

**Data Sources**:
- Video Fact module (real-time work status)
- GPS/GLONASS module (equipment location)
- 1С:PM system (actual vs. planned progress)
- Financial data (actual costs)

### 4. Financial Control Agent
**Role**: Budget management and financial oversight

**Specialization**: EVM (Earned Value Management) methodology

**Capabilities**:
- Real-time budget monitoring
- EVM implementation and reporting
- Variance analysis (schedule variance, cost variance)
- Forecast-at-completion (EAC) calculations
- Cost verification against Video Fact
- Budget forecasting and contingency recommendations

**Key Metrics**:
- SPI (Schedule Performance Index) > 0.95
- CPI (Cost Performance Index) > 0.95
- Budget variance tracking
- Trend analysis

### 5. Knowledge Management Agent
**Role**: Learning system and best practices repository

**Technology**: RAG (Retrieval-Augmented Generation) system

**Capabilities**:
- Benchmark database queries
- Best practices recommendations
- Historical performance analysis
- Risk alert system (RAW - Risk Alert Warnings)
- Telegram bot interface for alerts
- Document search and knowledge retrieval

**Data Sources**:
- Historical project data
- Industry benchmarks
- Best practices database
- Risk registry

## Core Modules

### Video Fact Module (VFV)

**Purpose**: Automatic verification of equipment work status

**Detection Capabilities**:
- Equipment Status: Working / Idle / Moving
- Work Hours: Actual vs. Planned tracking
- Activity Classification: Task identification
- Confidence Scoring: Accuracy metrics

**Technical Architecture**:
- Camera-based video analysis
- Edge computing for real-time processing
- Cloud sync for results storage
- API for agent access

**Integration Points**:
- Planning Agent (benchmark data for estimates)
- Monitoring Agent (real-time work tracking)
- Financial Agent (actual work hours for cost allocation)

### GPS/GLONASS Module

**Purpose**: Real-time equipment tracking and verification

**Capabilities**:
- Real-time position tracking (sub-meter accuracy)
- Movement history and patterns
- Geofencing logic and unauthorized use alerts
- Cross-verification with Video Fact data
- Confidence scoring for location data

**Recommended Equipment**: Teltonika FMC640
- Dual GPS/GLONASS system
- Operates in extreme conditions
- 80K RUB per unit
- Cloud connectivity via Teltonika IoT

**Integration Points**:
- Monitoring Agent (real-time tracking)
- Planning Agent (historical movement patterns)
- Financial Agent (equipment allocation tracking)

### Video Fact + GPS Integration

**Confidence Scoring Algorithm**:
1. Video Fact provides: Work status + confidence
2. GPS provides: Equipment location + accuracy
3. Fusion: Cross-verify status with location
4. Final Score: Combined confidence based on consistency

**Anomaly Detection**:
- Equipment working in unauthorized location
- Equipment idle at unexpected location
- GPS signal loss during work period
- Video quality degradation alerts

## Communication Protocols

### Agent-to-Agent Communication

**Protocol**: RESTful APIs over HTTPS

**Format**: JSON request/response

**Latency Requirements**:
- Planning Agent: <2 seconds
- Monitoring Agent: <1 second (real-time)
- Financial Agent: <2 seconds
- Knowledge Agent: <5 seconds

### External System Integration

**1С:PM Integration**:
- Real-time project data sync (30-second updates)
- Task status synchronization
- Schedule updates

**1С:ERP Integration**:
- Financial data sync (hourly batches)
- Budget tracking
- Cost allocation updates

## Data Flow Architecture

```
Video Fact Module
    ↓ (work status, hours)
├─→ Planning Agent (for benchmarking)
├─→ Monitoring Agent (for tracking)
└─→ Financial Agent (for costing)

GPS/GLONASS Module
    ↓ (location, movement)
├─→ Monitoring Agent (for alerts)
├─→ Planning Agent (for patterns)
└─→ Financial Agent (for allocation)

1С:PM System
    ↓ (project structure, tasks)
├─→ Planning Agent (for scheduling)
├─→ Monitoring Agent (for progress)
└─→ Knowledge Agent (for history)

1С:ERP System
    ↓ (financial data, costs)
├─→ Financial Agent (for budgeting)
├─→ Monitoring Agent (for variance)
└─→ Knowledge Agent (for benchmarks)

All Agents
    ↑ ↓
PMO Director Agent (Orchestrator)
    ↓
User Interface / API
```

## Technology Stack

### AI & LLM
- **Claude** (Opus for orchestration, Sonnet/Haiku for workers)
- **LangChain** or **CrewAI** for agent orchestration
- **Vector DB** for RAG system

### Backend & API
- **FastAPI** for high-performance REST APIs
- **Unified API Design** across all agents
- **WebSocket** for real-time monitoring

### Data & Storage
- **PostgreSQL** for primary data storage
- **Redis** for caching and real-time data
- **Vector DB** (e.g., Pinecone) for embeddings and semantic search

### Deployment & Infrastructure
- **Docker** for containerization
- **Kubernetes** for orchestration
- **Cloud-native** (AWS/GCP/Azure compatible)
- **CI/CD pipeline** for automated deployment

## API Standards

All agent APIs follow unified standards:

### Request Format
```json
{
  "agent_id": "unique_identifier",
  "operation": "operation_name",
  "project_id": "project_identifier",
  "parameters": {
    "key": "value"
  },
  "metadata": {
    "timestamp": "ISO-8601",
    "user_id": "user_identifier",
    "priority": "normal"
  }
}
```

### Response Format
```json
{
  "success": true,
  "agent_id": "agent_identifier",
  "operation": "operation_name",
  "data": {
    "result": "actual_result"
  },
  "metadata": {
    "timestamp": "ISO-8601",
    "execution_time_ms": 1234,
    "confidence": 0.95
  }
}
```

## Performance Requirements

| Aspect | Target |
|--------|--------|
| System Uptime | >99.5% |
| Average Response Time | <500ms |
| Real-time Data Latency | <5 seconds |
| Planning Agent Response | <2 seconds |
| Monitoring Agent Response | <1 second |
| Financial Agent Response | <2 seconds |
| Prediction Accuracy | >90% |
| API Availability | 99.9% |

## Security Architecture

- **Authentication**: OAuth 2.0 for API access
- **Authorization**: Role-based access control (RBAC)
- **Data Encryption**: TLS 1.3 for transport, AES-256 for storage
- **Audit Logging**: All agent actions logged
- **Rate Limiting**: Per-agent and per-user limits

## Scalability Considerations

- **Horizontal Scaling**: Worker agents can be replicated
- **Load Balancing**: Round-robin across agent instances
- **Caching Strategy**: Redis for frequently accessed data
- **Database Partitioning**: By project or time period
- **Async Processing**: Long-running operations queued

---

**Last Updated**: January 29, 2026
**Architecture Version**: 1.0
**Status**: Ready for implementation
