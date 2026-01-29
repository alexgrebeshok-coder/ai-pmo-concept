# AI-PMO Agents Specification

## Overview

AI-PMO consists of 5 specialized AI agents that work in orchestration to provide intelligent project management. Each agent has specific responsibilities, capabilities, and integration points.

---

## 1. PMO Director Agent

### Type
**Orchestrator** - Central coordinator and decision maker

### Technology
- **LLM**: Claude Opus (for complex reasoning)
- **Framework**: LangChain or CrewAI
- **Role**: Smart task router based on project state

### Primary Responsibilities

1. **Request Routing**
   - Analyze incoming request
   - Determine which agents need to be involved
   - Create execution plan

2. **Result Synthesis**
   - Collect responses from worker agents
   - Resolve conflicts between recommendations
   - Generate unified response

3. **Error Handling**
   - Implement fallback mechanisms
   - Escalate critical issues
   - Maintain system resilience

4. **Context Management**
   - Maintain project state awareness
   - Track agent availability
   - Monitor system health

### Integration Points
- Direct interface with all 4 worker agents
- External API gateway
- User interface layer

### Key Metrics
- **Response Time**: <500ms average
- **Uptime**: >99.5%
- **Routing Accuracy**: >99%

### Example Workflow
```
User Request: "How are we doing on Project X?"
    ↓
PMO Director analyzes request:
- Need current status → Monitoring Agent
- Need financial forecast → Financial Agent
- Need schedule prediction → Planning Agent
    ↓
Sends parallel requests to 3 agents
    ↓
Receives responses with confidence scores
    ↓
Synthesizes into single executive summary
    ↓
Returns comprehensive status to user
```

---

## 2. Planning Agent

### Type
**Specialist Worker** - Schedule optimization and resource allocation

### Technology
- **LLM**: Claude Sonnet (balanced cost/performance)
- **Specialization**: ML algorithms for optimization
- **Framework**: LangChain or CrewAI

### Primary Responsibilities

1. **Schedule Optimization**
   - Analyze task dependencies
   - Optimize task sequencing
   - Calculate critical path
   - Level resource usage

2. **Resource Allocation**
   - Assign equipment to tasks
   - Allocate team members
   - Balance workload distribution
   - Prevent over-allocation

3. **Historical Analysis**
   - Extract productivity benchmarks from past projects
   - Identify patterns in similar work
   - Improve estimate accuracy

4. **Video Fact Integration**
   - Use actual work hours for benchmarking
   - Refine future estimates based on real performance
   - Identify efficiency trends

### Data Sources

| Source | Data Type | Update Frequency |
|--------|-----------|------------------|
| Video Fact Module | Actual work hours, equipment status | Real-time |
| GPS/GLONASS | Equipment movement patterns, idle time | Real-time |
| 1С:PM | Project structure, tasks, deadlines | Hourly |
| Historical Database | Past project metrics, benchmarks | Batch (nightly) |

### Key Algorithms

**Schedule Optimization**:
- Critical Path Method (CPM)
- Precedence Diagramming Method (PDM)
- Resource Leveling
- Monte Carlo simulation for risk

**Resource Allocation**:
- Linear optimization for equipment
- Constraint satisfaction for team skills
- Cost minimization algorithms

**Benchmarking**:
- Historical data matching (by task type, complexity)
- Confidence scoring based on similarity
- Weighted averaging for estimates

### Key Metrics
- **Estimate Accuracy**: >90%
- **Schedule Variance**: <5%
- **Resource Utilization**: 80-85%
- **Response Time**: <2 seconds

### Integration Points
- Planning API (receives requests)
- 1С:PM API (reads project data)
- Video Fact API (reads actual work data)
- GPS/GLONASS API (reads movement patterns)
- Knowledge Management Agent (reads benchmarks)

---

## 3. Monitoring Agent

### Type
**Specialist Worker** - Real-time progress tracking and deviation detection

### Technology
- **LLM**: Claude Haiku (optimized for real-time)
- **Specialization**: Stream processing and anomaly detection
- **Framework**: LangChain or CrewAI

### Primary Responsibilities

1. **Real-Time KPI Monitoring**
   - Track schedule performance (SPI)
   - Track cost performance (CPI)
   - Monitor work quality metrics
   - Calculate variance metrics

2. **Deviation Detection**
   - Identify schedule slips
   - Detect cost overruns
   - Flag quality issues
   - Spot anomalous equipment behavior

3. **Alert Generation**
   - Classify severity levels (Critical, High, Medium, Low)
   - Route alerts to appropriate stakeholders
   - Aggregate related alerts
   - Provide recommended actions

4. **Data Stream Processing**
   - Process Video Fact real-time streams
   - Process GPS/GLONASS position updates
   - Aggregate 1С data updates
   - Calculate composite metrics

### Data Sources

| Source | Data Type | Latency |
|--------|-----------|---------|
| Video Fact Module | Equipment status updates | <5 seconds |
| GPS/GLONASS | Position updates | <5 seconds |
| Financial System | Cost entries | <30 minutes |
| 1С:PM | Progress updates | <1 hour |

### Key Algorithms

**Deviation Detection**:
- Threshold-based alerting
- Trend analysis (SMA, EMA)
- Z-score anomaly detection
- Rule-based logic for complex conditions

**Severity Calculation**:
- Impact assessment
- Urgency evaluation
- Confidence scoring
- Stakeholder assignment

### Key Metrics
- **Detection Latency**: <5 seconds
- **Alert Accuracy**: >95%
- **False Positive Rate**: <5%
- **Response Time**: <1 second

### Alert Types

| Alert Type | Threshold | Action |
|------------|-----------|--------|
| Schedule Slip | >7 days delay | Notify PM + Director |
| Cost Overrun | >10% over budget | Notify Finance + Director |
| Equipment Issue | Video status anomaly | Notify Ops Lead |
| Quality Problem | >2 defects per 100 units | Notify QA Lead |
| Productivity Drop | <60% efficiency | Notify Crew Lead |

### Integration Points
- Monitoring API (receives queries)
- Video Fact API (real-time stream)
- GPS/GLONASS API (position stream)
- Financial System API (cost data)
- 1С:PM API (progress data)
- Alert Service (sends notifications)

---

## 4. Financial Control Agent

### Type
**Specialist Worker** - Budget management and financial oversight

### Technology
- **LLM**: Claude Sonnet (for financial reasoning)
- **Specialization**: EVM (Earned Value Management)
- **Framework**: LangChain or CrewAI

### Primary Responsibilities

1. **Budget Monitoring**
   - Track actual costs vs. budgeted costs
   - Monitor budget allocation by task
   - Flag budget overruns
   - Manage contingency reserves

2. **EVM Implementation**
   - Calculate Planned Value (PV)
   - Calculate Earned Value (EV)
   - Calculate Actual Cost (AC)
   - Compute Schedule Variance (SV = EV - PV)
   - Compute Cost Variance (CV = EV - AC)

3. **Forecasting**
   - Calculate Estimate at Completion (EAC)
   - Calculate Budget at Completion (BAC)
   - Calculate To-Complete Performance Index (TCPI)
   - Provide trend forecasts

4. **Cost Verification**
   - Cross-verify work hours with Video Fact
   - Validate equipment costs against GPS data
   - Reconcile 1С:ERP financial data
   - Identify cost anomalies

### Key Metrics

| Metric | Formula | Target |
|--------|---------|--------|
| CPI | EV / AC | >0.95 |
| SPI | EV / PV | >0.95 |
| Budget Variance | EV - AC | <10% |
| Schedule Variance | EV - PV | <10% |
| Forecast Accuracy | \|EAC - Actual\| / Actual | <5% |

### Data Sources

| Source | Data Type | Update Frequency |
|--------|-----------|------------------|
| 1С:ERP | Cost entries, invoices | Real-time |
| Video Fact | Actual work hours | Real-time |
| 1С:PM | Task completion status | Hourly |
| Planning Agent | Budgeted values | Daily |

### Integration Points
- Financial API (receives queries)
- 1С:ERP API (reads financial data)
- 1С:PM API (reads task status)
- Video Fact API (reads actual hours)
- Monitoring Agent (sends variance alerts)
- Knowledge Agent (accesses historical benchmarks)

### Example EVM Report
```
Project: Road Construction Project X

Planned Value (PV): 5,000,000 RUB
Earned Value (EV): 4,700,000 RUB
Actual Cost (AC):  4,800,000 RUB

Schedule Variance (SV): -300,000 RUB (6% behind schedule)
Cost Variance (CV):     -100,000 RUB (2% over budget)

CPI: 0.98 (slightly over budget)
SPI: 0.94 (behind schedule)

EAC: 5,210,000 RUB
BAC: 5,000,000 RUB
Forecast Variance: +210,000 RUB (+4.2% overrun expected)

Status: YELLOW - Monitor closely, may need corrective action
```

---

## 5. Knowledge Management Agent

### Type
**Specialist Worker** - Learning system and best practices repository

### Technology
- **LLM**: Claude Sonnet (for semantic understanding)
- **Specialization**: RAG (Retrieval-Augmented Generation)
- **Vector DB**: Pinecone or similar for embeddings
- **Framework**: LangChain for RAG implementation

### Primary Responsibilities

1. **Knowledge Retrieval**
   - Search benchmark database
   - Find similar historical projects
   - Retrieve best practices
   - Access risk registry

2. **Recommendation Generation**
   - Suggest optimal approaches
   - Recommend equipment/team composition
   - Provide risk mitigation strategies
   - Offer scheduling alternatives

3. **Risk Alert System (RAW)**
   - Identify potential risks
   - Generate risk alerts based on project state
   - Suggest mitigation actions
   - Track risk status

4. **Interface & Notifications**
   - Telegram bot for alerts and queries
   - Dashboard queries
   - Scheduled reports

### Knowledge Base

| Category | Content | Source |
|----------|---------|--------|
| Benchmarks | Historical productivity data | Past projects |
| Best Practices | Proven methodologies | Industry standards |
| Risk Registry | Known risks and mitigations | Historical issues |
| Equipment Specs | Performance characteristics | Equipment manuals |
| Team Capabilities | Skill profiles and availability | HR system |

### RAG Implementation

**Process**:
1. Query receives user question
2. Question converted to embedding
3. Vector DB searched for similar content
4. Top-K most relevant documents retrieved
5. Retrieved context + question sent to LLM
6. LLM generates contextual response

**Example**:
```
User Query: "What's the best approach for concrete pouring in cold weather?"
    ↓
Vector search finds:
- 3 past projects with winter concrete work
- 2 industry best practice documents
- 5 risk alerts from similar conditions
    ↓
Retrieved context fed to Claude with question
    ↓
Response includes:
- Recommended approach (with confidence)
- Historical precedent (similar projects)
- Potential risks (with mitigations)
- Temperature thresholds and precautions
```

### Key Metrics
- **Search Accuracy**: >90%
- **Recommendation Quality**: >4/5 user rating
- **Response Time**: <5 seconds
- **Knowledge Base Coverage**: >95% of common scenarios

### Integration Points
- Knowledge API (receives queries)
- Vector DB (searches embeddings)
- Telegram Bot API (sends alerts and receives queries)
- Financial Agent (historical cost data)
- Monitoring Agent (risk event data)
- Planning Agent (historical scheduling data)

### Telegram Bot Interface

**Features**:
- `/status` - Current project status
- `/risks` - Active risk alerts
- `/recommend` - Get recommendations
- `/search` - Search knowledge base
- `/history` - View similar past projects

**Alert Notifications**:
- Critical alerts in real-time
- Daily summary reports
- Weekly trend analysis

---

## Agent Communication Matrix

| From | To | Message Type | Frequency |
|------|----|--------------:|-----------|
| PMO Director | Planning | Request/Response | On-demand |
| PMO Director | Monitoring | Request/Response | On-demand |
| PMO Director | Financial | Request/Response | On-demand |
| PMO Director | Knowledge | Request/Response | On-demand |
| Planning | Knowledge | Get benchmarks | Real-time |
| Monitoring | Financial | Send variance alerts | Real-time |
| Financial | Knowledge | Get historical data | Hourly |
| Knowledge | All Agents | Recommendations | On-demand |

---

## Agent Deployment

### Development
- Each agent developed in isolation
- Unit tests >90% coverage
- Integration tests with mock APIs

### Testing
- Individual agent testing
- Inter-agent communication testing
- End-to-end system testing
- Load testing for scalability

### Deployment
- Docker containerization
- Kubernetes orchestration
- Blue-green deployment for updates
- Automatic rollback on failure

### Monitoring
- Agent health checks
- Response time monitoring
- Error rate tracking
- Resource utilization alerts

---

**Last Updated**: January 29, 2026
**Agent Specification Version**: 1.0
**Status**: Ready for implementation
