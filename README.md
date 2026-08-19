# Lab 1: Requirements Engineering & UML Use-Case Modelling

**Problem Statement #44 | Developer Tools & IT OperationsProject: Database Query Performance Profiler**

## Overview

A database observability tool that ingests slow query logs, parses SQL execution plans, identifies missing index candidates, and generates weekly performance optimization digests.

## Actors

- **Database Administrator** — manages the database, reviews flagged queries, configures alerts
- **Backend Lead** — owns the application code, reviews/exports performance digests
- **System Scheduler** — automated actor that triggers the weekly digest job

## Use Cases

| ID | Use Case | Primary Actor |
| --- | --- | --- |
| UC-01 | Ingest Slow Query Logs | Database Administrator |
| UC-02 | Parse SQL Execution Plan | (system-internal, triggered via include) |
| UC-03 | Identify Missing Index Candidates | Database Administrator |
| UC-04 | Recommend Index Column Definitions | Database Administrator, Backend Lead |
| UC-05 | Generate Weekly Optimization Digest | Backend Lead, System Scheduler |
| UC-06 | Configure Alert Thresholds | Database Administrator |

## Relationships

- **«include»**: UC-03 (Identify Missing Index Candidates) includes UC-02 (Parse SQL Execution Plan) — a plan must always be parsed first.
- **«extend»**: UC-04 (Recommend Index Column Definitions) extends UC-03 (Identify Missing Index Candidates) — a recommendation is only generated when a missing-index condition is found.

## Deliverables in this Repo

- `requirements_table.pdf` / `.docx` — 5 FRs (FR-001–FR-005) + 2 NFRs (NFR-001, NFR-002)
- `usecase_diagram.png` / `.pdf` — UML use-case diagram
- `usecase_flow.pdf` — Use-case flow specification for UC-03 (Preconditions, Postconditions, Main Success Scenario, Alternate Flow)

## Core Use Case Detailed: UC-03 — Identify Missing Index Candidates

- **Preconditions**: A slow query log batch has been ingested (UC-01 complete)
- **Postconditions**: A missing composite index candidate is recorded, or the query is marked "reviewed — no gap found"
- **Main Flow**: Select query → retrieve execution plan (include UC-02) → detect scan on large table → cross-check existing indexes → flag missing index → estimate savings → display to DBA
- **Alternate Flow**: If an index already covers the query, mark as reviewed with no candidate raised
