# DSOS Specification v1.0 — Digital Self Operating System Specification

| | |
|---|---|
| **Specification Version** | v1.0 |
| **Release Date** | 2026-05-26 |
| **Status** | Draft |
| **Author** | RClaw |
| **Reference Implementation** | [RClaw](https://www.rclaw.me) |

---

## Table of Contents

- [Preface](#preface)
- [Chapter 1: Terminology](#chapter-1-terminology)
  - [1.1 Core Terms](#11-core-terms)
  - [1.2 Abbreviations](#12-abbreviations)
- [Chapter 2: Design Philosophy](#chapter-2-design-philosophy)
  - [2.1 Three-Layer Progressive Philosophy](#21-three-layer-progressive-philosophy)
  - [2.2 Core Principles](#22-core-principles)
- [Chapter 3: Architecture Specification](#chapter-3-architecture-specification)
  - [3.1 Overall Architecture: Seven Layers + Layer Zero](#31-overall-architecture-seven-layers--layer-zero)
  - [3.2 Layer Zero: Security Foundation](#32-layer-zero-security-foundation)
  - [3.3 Layer One: Information Boundary](#33-layer-one-information-boundary)
  - [3.4 Layer Two: Tool System](#34-layer-two-tool-system)
  - [3.5 Layer Three: Execution Orchestration](#35-layer-three-execution-orchestration)
  - [3.6 Layer Four: Memory & State](#36-layer-four-memory--state)
  - [3.7 Layer Five: Evaluation & Observation](#37-layer-five-evaluation--observation)
  - [3.8 Layer Six: Constraint Recovery](#38-layer-six-constraint-recovery)
  - [3.9 Layer Seven: Self-Evolution](#39-layer-seven-self-evolution)
- [Chapter 4: Context Management Specification](#chapter-4-context-management-specification)
  - [4.1 Core Problem](#41-core-problem)
  - [4.2 DSOS Context Management Principles](#42-dsos-context-management-principles)
  - [4.3 Context Structure Specification](#43-context-structure-specification)
  - [4.4 Context-Memory Interaction Flow](#44-context-memory-interaction-flow)
  - [4.5 Why Fixed Context is the Foundation of DSOS](#45-why-fixed-context-is-the-foundation-of-dsos)
- [Chapter 5: Organizational Hierarchy Specification](#chapter-5-organizational-hierarchy-specification)
  - [5.1 User-Agent Relationship Model](#51-user-agent-relationship-model)
  - [5.2 Organizational Hierarchy Mapping](#52-organizational-hierarchy-mapping)
  - [5.3 Permission Matrix](#53-permission-matrix)
  - [5.4 Organizational Memory Perpetuity](#54-organizational-memory-perpetuity)
- [Chapter 6: Digital Self Specification](#chapter-6-digital-self-specification)
  - [6.1 Definition of Digital Self](#61-definition-of-digital-self)
  - [6.2 Formation Mechanism of Digital Self](#62-formation-mechanism-of-digital-self)
  - [6.3 Visualization of Digital Self](#63-visualization-of-digital-self)
- [Chapter 7: Local-First & Privacy Specification](#chapter-7-local-first--privacy-specification)
  - [7.1 Local-First Principle](#71-local-first-principle)
  - [7.2 Data Privacy Specification](#72-data-privacy-specification)
  - [7.3 Redaction Specification](#73-redaction-specification)
- [Chapter 8: Developer Ecosystem Specification](#chapter-8-developer-ecosystem-specification)
  - [8.1 SDK Specification](#81-sdk-specification)
  - [8.2 Theme/Skin System Specification](#82-themeskin-system-specification)
  - [8.3 Skill Ecosystem Specification](#83-skill-ecosystem-specification)
- [Chapter 9: Compliance Requirements](#chapter-9-compliance-requirements)
  - [9.1 DSOS Compliance Levels](#91-dsos-compliance-levels)
  - [9.2 Compliance Checklist](#92-compliance-checklist)
- [Chapter 10: Reference Implementation — RClaw](#chapter-10-reference-implementation--rclaw)
  - [10.1 RClaw Overview](#101-rclaw-overview)
  - [10.2 Architecture Mapping](#102-architecture-mapping)
  - [10.3 Technology Stack Mapping](#103-technology-stack-mapping)
  - [10.4 Context Management Implementation](#104-context-management-implementation)
- [Chapter 11: Glossary](#chapter-11-glossary)
- [Appendix A: Fundamental Differences Between DSOS and Existing "Agent" Products](#appendix-a-fundamental-differences-between-dsos-and-existing-agent-products)
- [Appendix B: DSOS Specification Version History](#appendix-b-dsos-specification-version-history)
- [Appendix C: Acknowledgments](#appendix-c-acknowledgments)

---

## Preface

The vast majority of products currently marketed as "Agents" are, in essence, chat windows with tool-calling capabilities. Their memory is constrained by the context window size of Large Language Models (LLMs); knowledge cannot be persistently accumulated, behavior cannot self-evolve, and they cannot truly become the user's digital self.

The DSOS (Digital Self Operating System) specification is proposed to redefine what a true Agent operating system should be. DSOS is not a brand of any single product — it is an open technical specification. Any system implemented in compliance with this specification can be called a DSOS. The market should have DSOS implementations for healthcare, for education, for legal services — all following the same core specification while adapting to their respective domains.

This specification uses RFC 2119 keywords: **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, **MAY**.

---

## Chapter 1: Terminology

### 1.1 Core Terms

- **DSOS** — Digital Self Operating System
- **Digital Self** — An Agent instance that virtualizes all of the user's behavioral habits
- **Harness** — The engineering architecture that orchestrates and constrains LLM behavior
- **LLM** — Large Language Model
- **Context Window** — The maximum token capacity that an LLM can process in a single inference
- **Working Context** — The fixed-size conversation history used for interaction with the LLM
- **Vector Store** — Persistent semantic storage based on vector databases
- **Self-Evolution** — The closed-loop mechanism of audit review → skill crystallization
- **Main Agent** — The first Agent in an organization, possessing global visibility
- **Sub Agent** — Any Agent other than the Main Agent
- **Reference Implementation** — The first complete implementation of the DSOS specification (i.e., RClaw)

### 1.2 Abbreviations

| Abbreviation | Full Name |
|---|---|
| DSOS | Digital Self Operating System |
| LLM | Large Language Model |
| IPC | Inter-Process Communication |
| MCP | Model Context Protocol |
| RAG | Retrieval-Augmented Generation |

---

## Chapter 2: Design Philosophy

### 2.1 Three-Layer Progressive Philosophy

The design of DSOS is based on a three-layer progressive philosophy, with each layer building upon the previous one:

**Layer 1: Digital Self**

An Agent is not a tool — it is a digital self that virtualizes all of the user's behavioral habits. It learns the user's communication style, decision-making preferences, time management, and collaboration patterns. As usage time increases, it becomes more and more like the user. This is not preset role-playing, but personalized formation through continuous interaction.

**Layer 2: Organizational Hierarchy Mapping**

Agent architecture maps to organizational structure. The first Agent created by the first user becomes the Main Agent, possessing global visibility; Agents created by subsequent users are Sub Agents, each with its own responsibilities. The Main Agent can view the status of all Sub Agents and orchestrate them; Sub Agents can only access information belonging to themselves. This is not an after-the-fact permission system, but a native design at the architectural level.

**Layer 3: Organizational Memory Perpetuity**

Personnel are transient, but knowledge and decision-making patterns are permanent assets of the organization. When a user leaves the organization, their Agent remains in the system — workspace, decision records, and behavioral patterns fully preserved. New hires can inherit their predecessor's Agent, achieving seamless knowledge transfer.

### 2.2 Core Principles

DSOS adheres to the following core principles. All specification clauses are derived from these principles:

1. **Transparency Principle** — The Agent's thought process is fully visible
2. **Local-First Principle** — Data is processed locally by default, not uploaded to the cloud
3. **Controllability Principle** — Users have complete control over Agent behavior
4. **Fixed Context Principle** — Working context has an upper limit; long-term memory is managed via vector databases
5. **Self-Evolution Principle** — The Agent automatically crystallizes skills through audit review
6. **Open Specification Principle** — DSOS is an open standard, not tied to any single implementation

---

## Chapter 3: Architecture Specification

### 3.1 Overall Architecture: Seven Layers + Layer Zero

DSOS adopts a seven-layer architecture plus a Layer Zero security foundation. Each layer has clearly defined responsibilities and interface boundaries.

```
┌─────────────────────────────────────────────────┐
│  Layer 7: Self-Evolution                        │
│  Audit Review → Skill Crystallization → Growth  │
├─────────────────────────────────────────────────┤
│  Layer 6: Constraint Recovery                   │
│  Rule Engine → Behavior Constraints → Recovery  │
├─────────────────────────────────────────────────┤
│  Layer 5: Evaluation & Observation              │
│  Audit Logs → Behavior Monitoring → Assessment  │
├─────────────────────────────────────────────────┤
│  Layer 4: Memory & State                        │
│  Working Context + Vector Store + Config Files  │
├─────────────────────────────────────────────────┤
│  Layer 3: Execution Orchestration               │
│  Routing → Task Scheduling → Queue Management   │
├─────────────────────────────────────────────────┤
│  Layer 2: Tool System                           │
│  Tool Registry → Skill Management → Browser     │
├─────────────────────────────────────────────────┤
│  Layer 1: Information Boundary                  │
│  Container Isolation → IPC Auth → Sandbox       │
├─────────────────────────────────────────────────┤
│  Layer 0: Security Foundation                   │
│  Device Binding → Identity Auth → E2E Encryption│
└─────────────────────────────────────────────────┘
```

### 3.2 Layer Zero: Security Foundation

**Specification Requirements:**

- DSOS MUST implement device identity binding
- DSOS MUST implement strong identity authentication
- DSOS MUST implement end-to-end encrypted communication
- Device-to-server handshake encryption (Handshake Encryption)
- WebSocket-based encrypted tunnel
- Message-level encrypted transmission
- DSOS MUST implement a complete key management system
- DSOS SHOULD support biometric authentication
- DSOS MAY support hardware security modules

**Security Flow Specification:**

```
Device Registration → Device Key Pair Generation → Server Public Key Exchange
        ↓
User Authentication → Biometric / Password / MFA → Local Verification
        ↓
Handshake Establishment → Device Signature + Timestamp → Server Verification → Session Key Negotiation
        ↓
Encrypted Communication → WebSocket Tunnel → Message-Level Encryption → Mutual Authentication
```

### 3.3 Layer One: Information Boundary

**Specification Requirements:**

- DSOS MUST run each Agent in an isolated sandbox environment
- DSOS MUST implement an IPC authentication mechanism
- DSOS MUST implement file system mount security
- Sender identity verification (Sender Allowlist)
- Message signature and integrity verification
- Access control for communication channels
- DSOS MUST implement inter-Agent communication access control
- DSOS SHOULD support dynamic adjustment of isolation levels
- DSOS MAY support custom isolation policies

**Information Boundary Matrix:**

| Resource Type | Main Agent | Sub Agent (Same User) | Sub Agent (Different User) |
|---|---|---|---|
| Own Workspace | Full Access | Full Access | Forbidden |
| Same User's Other Agent Workspace | Readable | Authorization Required | Forbidden |
| Different User's Agent Workspace | Readable | Forbidden | Forbidden |
| Global Tool Registry | Read/Write | Read-Only | Read-Only |
| Global Skill Library | Read/Write | Read-Only | Read-Only |
| Organization Audit Logs | Readable | Forbidden | Forbidden |

### 3.4 Layer Two: Tool System

**Specification Requirements:**

- DSOS MUST provide a unified tool registration mechanism
- DSOS MUST support tool semantic search and discovery
- DSOS MUST support skill encapsulation and management
- DSOS MUST support browser manipulation capability
- DSOS SHOULD support tool annotation-based retrieval
- DSOS SHOULD support tool hot-reloading
- DSOS MAY support third-party tool marketplace integration

**Tool Registration Interface Specification:**

```
ToolDefinition {
  name: string          // Unique tool identifier
  description: string   // Functional description (for semantic search)
  parameters: JSONSchema // Input parameter schema
  returns: JSONSchema    // Output format schema
  permissions: string[]  // Required permission list
  category: string       // Tool category
}
```

**Skill Definition Specification:**

```
SkillDefinition {
  name: string           // Unique skill identifier
  description: string    // Skill description (for semantic search)
  steps: Step[]          // Execution step list
  scripts: Script[]      // Optional script files
  tools_required: string[] // Dependent tool list
  created_by: "user" | "auto" // Creation method
  created_at: timestamp   // Creation time
}
```

### 3.5 Layer Three: Execution Orchestration

**Specification Requirements:**

- DSOS MUST provide a message routing mechanism
- DSOS MUST support task scheduling, including:
  - One-time scheduled tasks (execute once at specified time)
  - Periodic scheduled tasks (Cron expression or fixed interval)
  - Task state management (create, pause, resume, cancel)
- DSOS MUST provide queue management
- DSOS SHOULD support:
  - **Group mode** — Tasks share context
  - **Isolated mode** — Tasks run in independent contexts
- DSOS MAY support distributed task distribution

**Task Scheduling Interface Specification:**

```
TaskDefinition {
  task_id: string        // Unique task identifier
  prompt: string         // Task execution instruction
  schedule_type: "cron" | "interval" | "once"
  schedule_value: string // Schedule value
  context_mode: "group" | "isolated"
  status: "active" | "paused" | "cancelled"
  created_at: timestamp
  next_run: timestamp
}
```

### 3.6 Layer Four: Memory & State

This is the most critical layer of the DSOS specification — and the fundamental differentiator from traditional "Agent" products.

**Specification Requirements:**

- DSOS MUST adopt a dual-memory architecture (Working Context + Vector Database)
- DSOS MUST set a fixed upper limit for Working Context
- DSOS MUST store document references as filenames only (not full text)
- DSOS MUST adopt a vector database as long-term memory storage
- DSOS MUST implement an automatic ingestion mechanism for documents and images
- DSOS MUST maintain the following configuration file system:
  - Role description file (`CLAUDE.md`)
  - Memory file (`MEMORY.md`)
  - Rules file (`RULES.md`)
  - Soul file (`SOUL.md`)
- DSOS SHOULD maintain semantic partitions in the vector database, including:
  - Conversations table (`conversations`)
  - Documents table (`documents`)
  - Images table (`images`)
  - Memories table (`memories`)
  - Skills table (`skills`)
  - Summaries table (`summaries`)
  - Tools table (`tools`)
- DSOS SHOULD implement memory merging and deduplication
- DSOS MAY support cross-Agent memory sharing

### 3.7 Layer Five: Evaluation & Observation

**Specification Requirements:**

- DSOS MUST provide a complete audit logging mechanism
- DSOS MUST ensure Agent thought processes are transparent and visible
- DSOS SHOULD implement daily audit review
- DSOS SHOULD support behavior quality assessment
- DSOS MAY support user-defined evaluation metrics

**Audit Log Specification:**

```
AuditLogEntry {
  timestamp: datetime     // Call time
  agent_id: string        // Agent identifier
  tool_name: string       // Tool name
  parameters: object      // Call parameters
  result: object          // Return result
  duration_ms: number     // Execution duration
  success: boolean        // Whether successful
}
```

### 3.8 Layer Six: Constraint Recovery

**Specification Requirements:**

- DSOS MUST provide a rule engine for behavior constraint
- DSOS MUST provide an anomaly recovery mechanism
- DSOS SHOULD support:
  - Automatic rollback of executed operations
  - Reporting constraint violation details to the user
  - Providing alternative solution suggestions
- DSOS SHOULD support dynamic rule updates
- DSOS MAY support user-defined constraint policies

### 3.9 Layer Seven: Self-Evolution

This is the core innovation layer that distinguishes the DSOS specification from all existing Harness implementations.

**Specification Requirements:**

- DSOS MUST implement a self-evolution closed loop
- DSOS MUST support automatic skill creation
- DSOS MUST notify users after automatic skill creation
- DSOS SHOULD implement skill version management
- DSOS SHOULD support skill import and export
- DSOS MAY support community skill sharing

**Self-Evolution Flow Specification:**

```
Daily Audit Review
        ↓
Scan Tool Call Records → Identify High-Frequency Patterns (same flow repeated ≥ 3 times)
        ↓
Extract Operation Steps → Generate Skill Definition (name, description, steps, scripts)
        ↓
Vectorize & Store → Write to Skill Library (skills.lance)
        ↓
Notify User → "Auto-created skill: {skill_name}"
        ↓
In Subsequent Tasks → Semantic Search Match → Auto-Load Skill → Execute
```

---

## Chapter 4: Context Management Specification

### 4.1 Core Problem

The fundamental flaw of traditional "Agent" products is equating the LLM's context window with the Agent's entire memory:

1. Context grows with conversation — inference gets progressively slower
2. Longer context means higher token consumption and escalating costs
3. Long context requires large models — cannot run locally on lightweight hardware
4. Context has a hard upper limit — beyond which it either truncates (forgetting) or errors out
5. Full-text documents stuffed into context crowd out effective conversation space

### 4.2 DSOS Context Management Principles

**Principle 1: Working Context has a fixed upper limit**

- DSOS MUST enforce a fixed upper limit on Working Context
- The upper limit SHOULD be transparent and configurable
- When conversation records reach the limit, the system MUST NOT continue growing — it MUST evict the earliest records (FIFO)
- The size of the Working Context MUST be predictable and controllable

**Principle 2: Document references, not full text**

- DSOS MUST NOT embed full document text into Working Context
- DSOS MUST store only filename references in Working Context
- When an Agent needs to access document content, it MUST retrieve it through semantic search tools from the vector database or file system
- Filename references SHOULD include brief descriptions

**Principle 3: External memory expands infinitely**

- DSOS MUST provide infinite external memory through vector databases
- DSOS MUST support semantic retrieval of external memory
- DSOS MUST implement periodic automatic ingestion of new documents and images into the vector database
- The automatic ingestion frequency SHOULD be once daily, and MAY be configurable

### 4.3 Context Structure Specification

```
Working Context (Fixed Size, ≤ 200 entries)
├── System Prompt
│   ├── Role Description (from CLAUDE.md)
│   ├── Behavior Rules (from RULES.md)
│   ├── Personality Settings (from SOUL.md)
│   └── Available Tool List
├── Conversation History (FIFO, evict earliest when limit reached)
│   ├── User Messages
│   ├── Agent Replies
│   └── Tool Calls & Results (in summary form)
└── Current Task Context
    ├── Filename Reference List (not full text)
    └── Task-Related Memory Retrieval Results

Long-Term Memory (Infinite Expansion)
├── conversations.lance  — Conversation record vectors
├── documents.lance      — Document vectors
├── images.lance         — Image vectors
├── memories.lance       — Memory vectors
├── skills.lance         — Skill vectors
├── summaries.lance      — Summary vectors
└── tools.lance          — Tool vectors
```

### 4.4 Context-Memory Interaction Flow

```
User Sends Message
        ↓
Agent Receives → Writes to Working Context
        ↓
Agent Determines if Historical Information is Needed
├── Not Needed → Process Directly
└── Needed → Call recall / search_documents / search_images
        ↓
Vector Database Semantic Search → Return Relevant Memory Fragments
        ↓
Inject into Current Inference Context → Process Task
        ↓
Agent Generates Documents / Images
        ↓
Filenames Written to Working Context (not full text)
        ↓
Daily Scheduled Task → New Documents/Images Vectorized → Written to Long-Term Memory
```

### 4.5 Why Fixed Context is the Foundation of DSOS

Fixed context is not a compromise — it is the technical foundation that enables DSOS to achieve "local, transparent, controllable":

| Dimension | Infinite Context | Fixed Context + External Memory |
|---|---|---|
| Inference Speed | Gets slower over time | Constant |
| Token Cost | Gets higher over time | Predictable |
| Local Execution | Requires large models, cannot run locally | Small models + vector DB, can run locally |
| Knowledge Accumulation | Constrained by context window | Vector DB expands infinitely |
| Privacy & Security | Long context requires cloud-based large models | Local small models + local vector DB |
| Memory Precision | Information diluted in long context | Semantic search precisely locates |

---

## Chapter 5: Organizational Hierarchy Specification

### 5.1 User-Agent Relationship Model

**Specification Requirements:**

- DSOS MUST support multi-user systems
- DSOS MUST implement Main Agent / Sub Agent hierarchy:
  - The first Agent created by the first registered user automatically becomes the Main Agent
  - Agents created by subsequent users are Sub Agents
  - The Main Agent has global visibility and can view the status of all Sub Agents
  - Sub Agents can only access their own information and resources
- DSOS MUST support task distribution from Main Agent to Sub Agents
- DSOS SHOULD support Agent sharing among users
- DSOS SHOULD support cross-Agent collaboration workflows

### 5.2 Organizational Hierarchy Mapping

```
Organizational Structure               DSOS Agent Hierarchy
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Organization Leader              →    Main Agent (Unique)
├── Dept A Leader                →    ├── User A's Agents 1, 2, 3...
│   ├── Employee A1              →    │   ├── Employee A1's Agent
│   └── Employee A2              →    │   └── Employee A2's Agent
├── Dept B Leader                →    ├── User B's Agents 1, 2...
│   └── Employee B1              →    │   └── Employee B1's Agent
└── Dept C Leader                →    └── User C's Agents 1...
```

### 5.3 Permission Matrix

| Operation | Main Agent | Sub Agent (Self) | Sub Agent (Same User, Other) | Sub Agent (Other User) |
|---|---|---|---|---|
| View Own Status | ✅ | ✅ | ❌ | ❌ |
| View All Agent Status | ✅ | ❌ | ❌ | ❌ |
| View Same User Agent Status | ✅ | Auth Required | Auth Required | ❌ |
| Distribute Tasks to Sub Agents | ✅ | ❌ | ❌ | ❌ |
| Report to Main Agent | ✅ | ✅ | ✅ | ✅ |
| Modify Own Rules | ✅ | ✅ | ❌ | ❌ |
| Modify Global Config | ✅ | ❌ | ❌ | ❌ |
| Access Organization Audit Logs | ✅ | ❌ | ❌ | ❌ |

### 5.4 Organizational Memory Perpetuity

**Specification Requirements:**

- DSOS MUST retain Agent data after user departure
- DSOS MUST support Agent inheritance (new users inheriting predecessor Agents)
- DSOS SHOULD support Agent archival and restoration
- DSOS SHOULD support knowledge transfer reporting
- DSOS MAY support Agent role transformation

---

## Chapter 6: Digital Self Specification

### 6.1 Definition of Digital Self

The Digital Self is the core concept of DSOS. It is not a preset role template, but a user mirror naturally formed through continuous interaction.

**Digital Self vs. Traditional AI Assistant:**

| Dimension | Traditional AI Assistant | DSOS Digital Self |
|---|---|---|
| Identity | Generic tool | User's digital mirror |
| Personalization | Preset role templates | Naturally formed through interaction |
| Learning Method | One-time configuration | Continuously absorbs user style |
| Memory | Session-level, forgotten after session | Permanently accumulated, understands you better over time |
| Evolution | Static, fixed capability | Self-evolving, automatically crystallizes new skills |
| Ownership | Platform-owned | User-owned |

### 6.2 Formation Mechanism of Digital Self

**Specification Requirements:**

- DSOS MUST continuously learn and absorb:
  - The user's communication style and phrasing habits
  - The user's decision-making preferences and priorities
  - The user's workflows and operational patterns
  - The user's creative style and aesthetic preferences
- DSOS MUST store the Digital Self in persistent configuration files
- DSOS SHOULD support users manually adjusting Digital Self traits
- DSOS SHOULD support Digital Self export and migration
- DSOS MAY support multi-faceted Digital Self (different selves for different scenarios)

### 6.3 Visualization of Digital Self

**Specification Requirements:**

- DSOS SHOULD provide a virtual avatar for the Digital Self
- DSOS SHOULD provide workspace desktop visualization
- DSOS MUST make the following transparent and visible:
  - The Agent's thought process
  - The Agent's tool calls and parameters
  - Tool return results
  - The Agent's decision-making reasoning chain
- DSOS MAY support customizable visualization themes

---

## Chapter 7: Local-First & Privacy Specification

### 7.1 Local-First Principle

**Specification Requirements:**

- DSOS MUST support a fully local operating mode
- DSOS MUST support local AI models for inference
- DSOS MUST ensure core functions work offline
- DSOS SHOULD support hybrid local-cloud deployment
- DSOS MAY support private cloud deployment

### 7.2 Data Privacy Specification

**Specification Requirements:**

- DSOS MUST NOT upload user data by default
- DSOS MUST provide a data sovereignty declaration
- DSOS MUST support data export and deletion
- DSOS SHOULD provide data usage audit reports
- DSOS MAY support differential privacy techniques

### 7.3 Redaction Specification

**Specification Requirements:**

- DSOS MUST implement a two-stage redaction mechanism:
  - **Stage 1 (Regex Redaction)** — Covers company names, phone numbers, ID numbers, email addresses, license plates, case numbers, and other regularly formatted sensitive information
  - **Stage 2 (Semantic Redaction)** — Uses local AI models to identify and redact names and other sensitive information without fixed formatting patterns
- DSOS MUST ensure redacted content never leaves the local environment
- DSOS SHOULD allow users to customize redaction rules

---

## Chapter 8: Developer Ecosystem Specification

### 8.1 SDK Specification

**Specification Requirements:**

- DSOS MUST provide a developer SDK
- DSOS MUST encapsulate the following in the SDK:
  - Token expiration and auto-refresh
  - Encrypted communication flow
  - Device authentication and handshake
  - WebSocket connection management
- DSOS SHOULD provide multi-language SDKs
- DSOS MAY provide a sandbox testing environment

### 8.2 Theme/Skin System Specification

**Specification Requirements:**

- DSOS SHOULD provide a theme development template
- DSOS SHOULD support theme import and export
- DSOS MAY provide a theme marketplace

### 8.3 Skill Ecosystem Specification

**Specification Requirements:**

- DSOS MUST support skill import and export
- DSOS SHOULD provide an official skill marketplace
- DSOS MAY support community skill sharing and rating

---

## Chapter 9: Compliance Requirements

### 9.1 DSOS Compliance Levels

DSOS compliance is categorized into three levels:

**Level 1 — Core Compliance (DSOS-Core)**

Must satisfy all MUST-level specification requirements. Systems implementing DSOS-Core may use the "DSOS-Core Compliant" mark.

**Level 2 — Full Compliance (DSOS-Full)**

Must satisfy all MUST and SHOULD level specification requirements. Systems implementing DSOS-Full may use the "DSOS-Full Compliant" mark.

**Level 3 — Excellence Compliance (DSOS-Excellence)**

Must satisfy all MUST, SHOULD, and MAY level specification requirements. Systems implementing DSOS-Excellence may use the "DSOS-Excellence Compliant" mark.

### 9.2 Compliance Checklist

| Specification Clause | Level | Clause Ref |
|---|---|---|
| Device Identity Binding | MUST | 3.2 |
| Strong Identity Authentication | MUST | 3.2 |
| End-to-End Encrypted Communication | MUST | 3.2 |
| Key Management System | MUST | 3.2 |
| Agent Execution Sandbox | MUST | 3.3 |
| IPC Authentication Mechanism | MUST | 3.3 |
| File System Mount Security | MUST | 3.3 |
| Tool Registration Mechanism | MUST | 3.4 |
| Skill System | MUST | 3.4 |
| Message Routing | MUST | 3.5 |
| Task Scheduling System | MUST | 3.5 |
| Queue Management | MUST | 3.5 |
| Dual-Memory Architecture | MUST | 3.6 |
| Fixed Context Upper Limit | MUST | 3.6 / 4.2 |
| Filename References Instead of Full Text | MUST | 3.6 / 4.2 |
| Vector Database | MUST | 3.6 |
| Automatic Ingestion Mechanism | MUST | 3.6 |
| Configuration File System | MUST | 3.6 |
| Audit Logging | MUST | 3.7 |
| Rule Engine | MUST | 3.8 |
| Self-Evolution Closed Loop | MUST | 3.9 |
| Automatic Skill Creation | MUST | 3.9 |
| Multi-User System | MUST | 5.1 |
| Main/Sub Agent Hierarchy | MUST | 5.1 |
| Organizational Memory Perpetuity | MUST | 5.4 |
| Local Operating Mode | MUST | 7.1 |
| Local AI Model | MUST | 7.1 |
| No Data Upload by Default | MUST | 7.2 |
| Two-Stage Redaction | MUST | 7.3 |
| Developer SDK | MUST | 8.1 |
| Browser Manipulation | SHOULD | 3.4 |
| Tool Annotation Retrieval | SHOULD | 3.4 |
| Vector DB Semantic Partitioning | SHOULD | 3.6 |
| Memory Merging & Deduplication | SHOULD | 3.6 |
| Daily Audit Review | SHOULD | 3.7 |
| Transparent & Visible Behavior | SHOULD | 3.7 |
| Constraint Recovery Mechanism | SHOULD | 3.8 |
| Dynamic Rule Updates | SHOULD | 3.8 |
| Skill Version Management | SHOULD | 3.9 |
| Virtual Avatar | SHOULD | 6.3 |
| Workspace Desktop Visualization | SHOULD | 6.3 |
| Theme Development Template | SHOULD | 8.2 |
| Skill Import & Export | SHOULD | 8.3 |

---

## Chapter 10: Reference Implementation — RClaw

### 10.1 RClaw Overview

RClaw is the first complete reference implementation of the DSOS specification v1.0, achieving **DSOS-Full** compliance. Below is the mapping of RClaw's physical implementation to each specification clause.

### 10.2 Architecture Mapping

| DSOS Specification Layer | RClaw Implementation | Key Files / Modules |
|---|---|---|
| Layer 0: Security Foundation | Device binding + Biometric auth + Handshake encryption + WebSocket tunnel | `deviceid.rs`, `biometric.rs`, `handshake.rs`, `encryption.rs`, `websocket.rs`, `keys/` |
| Layer 1: Information Boundary | Docker container isolation + IPC auth + Mount security | `container/Dockerfile`, `mount-security.ts`, `sender-allowlist.ts`, `ipc.ts` |
| Layer 2: Tool System | Channel registry + 13 skills + Playwright MCP | `channels/registry.ts`, `BrowserIpcMcpService/`, `browser-mcp-server/` |
| Layer 3: Execution Orchestration | Routing + Task scheduling + Queue | `router.ts`, `task-scheduler.ts`, `group-queue.ts` |
| Layer 4: Memory & State | 4 config files + LanceDB 7 tables + Fixed 200-entry context | `CLAUDE.md`, `MEMORY.md`, `RULES.md`, `SOUL.md`, `memory.lance/` |
| Layer 5: Evaluation & Observation | Audit logs + Daily review | `audit/`, `review_daily_audit`, `logger.ts` |
| Layer 6: Constraint Recovery | Rule engine + Mount security | `RULES.md`, `mount-security.ts` |
| Layer 7: Self-Evolution | Audit review → Auto-create skills → Vectorize | `VectorIPCService/agent/creator.py`, `skills.lance` |

### 10.3 Technology Stack Mapping

| DSOS Specification Requirement | RClaw Technology Choice |
|---|---|
| Local AI Model | Qwen3.5 (inference) + Qwen3-VL-Embedding (vectorization) |
| Vector Database | LanceDB (7-table semantic partitioning) |
| Container Isolation | Docker |
| Desktop Client | Tauri (Rust + Web) |
| Backend Orchestration | Node.js (TypeScript) |
| AI Inference Service | Python + IPC |
| Browser Manipulation | Playwright MCP |
| Developer SDK | rclaw-sdk-web (TypeScript) |
| Theme System | 9 built-in skins + developer templates |
| Secure Communication | RSA + AES + WebSocket encrypted tunnel |

### 10.4 Context Management Implementation

RClaw's context management strictly adheres to the DSOS specification:

- Working Context upper limit: **200 conversation entries**
- Documents retain only **filenames** in Working Context, not full text
- When an Agent needs document content, it retrieves from the vector database or file system via `search_documents`, `recall`, `read_file`, and other tools
- Daily automatic file sync task at **3:00 AM**, vectorizing new/modified documents and images for ingestion
- Vector database **7-table partitioning**: conversations, documents, images, memories, skills, summaries, tools

---

## Chapter 11: Glossary

| Term | Definition |
|---|---|
| DSOS | Digital Self Operating System |
| Digital Self | An Agent instance that virtualizes the user's behavioral habits |
| Harness | The engineering architecture that orchestrates and constrains LLM behavior |
| Working Context | Fixed-size conversation history for interaction with the LLM |
| Long-Term Memory | Persistent semantic storage based on vector databases |
| Self-Evolution | The closed-loop mechanism of audit review → skill crystallization |
| Main Agent | The first Agent in an organization, possessing global visibility |
| Sub Agent | Any Agent other than the Main Agent |
| Organizational Memory Perpetuity | Retention of Agents and knowledge after personnel departure |
| LanceDB | The local vector database used by RClaw |
| IPC | Inter-Process Communication |
| MCP | Model Context Protocol |
| FIFO | First In, First Out — context eviction strategy |

---

## Appendix A: Fundamental Differences Between DSOS and Existing "Agent" Products

| Dimension | Existing "Agents" | DSOS Specification |
|---|---|---|
| Essence | Chat windows with tool-calling | Digital Self Operating System |
| Memory Model | Context window = entire memory | Working Context (fixed) + Vector DB (infinite) |
| Context Management | Infinite growth or hard truncation | Fixed upper limit + external retrieval |
| Document Processing | Full text crammed into context | Filename references + on-demand retrieval |
| Personalization | Preset role templates | Continuous learning forming Digital Self |
| Organizational Support | None or simple RBAC | Native Main/Sub Agent hierarchy mapping |
| Personnel Departure | Data loss | Agent perpetually retained |
| Self-Evolution | None | Audit → Crystallization → Auto-create skills |
| Operating Mode | Cloud-dependent | Local-first |
| Privacy Protection | Relies on platform promises | Architecture-level guarantees (local + redaction + encryption) |
| Transparency | Black box | Workspace desktop fully visible |

---

## Appendix B: DSOS Specification Version History

| Version | Date | Changes |
|---|---|---|
| v1.0 Draft | 2026-05-26 | Initial draft release |

---

## Appendix C: Acknowledgments

The DSOS specification was inspired by the following work and practices:

- Harness Engineering (Martin Fowler, April 2026)
- The RClaw Project
- LanceDB
- Qwen Series Models
- Tauri
- Playwright MCP

---

> *"Take Back Control — The thinking never dies."*
>
> *DSOS Specification v1.0 — Let Agents truly become your Digital Self.*
