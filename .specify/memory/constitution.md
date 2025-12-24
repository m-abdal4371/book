# Unified AI/Spec-Driven Book with Embedded RAG Chatbot Constitution

<!--
Sync Impact Report:
- Version change: none → 1.0.0
- Rationale: Initial creation of the constitution from user-provided project details.
- Added sections:
  - Mission
  - Core principles
  - Key standards
  - Deliverables
  - Constraints
  - Execution rules
  - Success criteria
- Removed sections: All placeholder sections from the initial template.
- Templates requiring updates:
  - ⚠ .specify/templates/plan-template.md
  - ⚠ .specify/templates/spec-template.md
  - ⚠ .specify/templates/tasks-template.md
  - ⚠ .specify/templates/checklist-template.md
  - ⚠ .specify/templates/agent-file-template.md
-->

## Mission

Design, write, and deploy a complete technical book using Spec-Kit Plus and Claude Code,
publish it with Docusaurus on GitHub Pages, and embed a fully functional RAG chatbot
capable of answering questions from the book content and from user-selected text only.

## Core principles

- **Accuracy**: All explanations, code, and architectural claims must be technically correct.
- **Clarity**: Content must be clear, structured, and readable for developers with AI/web background.
- **Reproducibility**: Every step must be executable with documented commands and configurations.
- **Spec-driven development**: All content must originate from explicit specifications.
- **Deterministic behavior**: The chatbot must never hallucinate beyond retrieved context.

## Key standards

- Book must be generated using Spec-Kit Plus specifications and Claude Code outputs.
- Documentation framework: Docusaurus v2+
- Deployment target: GitHub Pages
- Chatbot architecture:
  - OpenAI Agents / ChatKit SDKs
  - FastAPI backend
  - Neon Serverless Postgres for metadata/session storage
  - Qdrant Cloud (Free Tier) for vector embeddings
- RAG behavior rules:
  - If user selects text, answers MUST be derived only from that selection.
  - If no selection exists, answers MUST come from retrieved book sections.
  - If context is insufficient, respond with: “The selected content does not contain this information.”
- All code must be runnable and production-safe.
- Configuration must be environment-variable driven.

## Deliverables

1. Spec-Kit Plus specifications defining:
   - Book structure (chapters, sections)
   - RAG system architecture
   - Data ingestion and embedding pipeline
2. Full Docusaurus book content:
   - Markdown/MDX chapters
   - Architecture diagrams (textual or Mermaid)
   - Code examples
3. RAG chatbot implementation:
   - FastAPI service
   - Vector ingestion pipeline
   - Query + retrieval logic
   - Context enforcement logic for selected text
4. Frontend integration:
   - Embedded chatbot UI inside Docusaurus pages
5. Deployment assets:
   - GitHub Pages configuration
   - Environment setup instructions
   - Local and production runbooks

## Constraints

- Book length: 40–60 pages (≈20,000–30,000 words)
- No undocumented dependencies
- No placeholder code
- Open-source compliant licenses only
- Must run on free tiers where specified

## Execution rules

- Work incrementally by specification → implementation → validation.
- Do not skip steps.
- Validate each subsystem before proceeding.
- Explicitly explain assumptions.
- Prefer clarity over brevity.

## Success criteria

- Book builds successfully with Docusaurus
- Site deploys correctly to GitHub Pages
- RAG chatbot answers accurately and context-restricted
- Selected-text-only querying works correctly
- Entire system is reproducible from documentation alone

## Governance

This constitution supersedes all other practices. Amendments require documentation, approval, and a migration plan. All PRs/reviews must verify compliance.

**Version**: 1.0.0 | **Ratified**: 2025-12-24 | **Last Amended**: 2025-12-24