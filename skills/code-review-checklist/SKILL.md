---
name: code-review-checklist
description: Comprehensive code review checklists covering security, performance, architecture, and testing. Loaded by the code-reviewer agent and its sub-agents as reference material. Use when performing detailed code reviews or when specific review areas need structured guidance.
---

# Code Review Checklist

Comprehensive review checklists used by the code-reviewer system. Each checklist is in a separate reference file for progressive disclosure.

## Available Checklists

- [Security Checklist](references/security-checklist.md) — OWASP-based security review
- [Performance Checklist](references/performance-checklist.md) — Performance analysis guide
- [Anti-Patterns Library](references/anti-patterns.md) — Common bad patterns with examples

## Quick Reference — Severity Guide

- **Critical (90-100)**: Security vulnerabilities, data loss, broken core functionality
- **Important (80-89)**: Performance issues, missing error handling, architecture problems
- **Minor (<80)**: NOT REPORTED — style, optimization, documentation

## Review Flow

1. Start with high-level architecture scan
2. Apply relevant checklists based on change type
3. Score each finding (confidence 0-100)
4. Filter: only report confidence >= 80
5. Produce actionable report

## Integration

**Consumed by agents:**
- `code-reviewer` — Orchestrator that dispatches the specialists below
- `security-reviewer` — Loads `references/security-checklist.md`
- `performance-reviewer` — Loads `references/performance-checklist.md`
- `architecture-reviewer` — Loads `references/anti-patterns.md`
