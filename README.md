# YouTube Content Creation Pipeline

**An autonomous multi-agent system that takes a YouTube video from raw idea to edit-ready draft.**

This is a production content system I designed and run for my gaming channel, *Alkenmist Online*. A team of specialized AI agents research a topic, write the script, plan the edit, package the upload, and quality-check the result — coordinating through a shared file-based state store, with a dedicated repair agent that keeps the whole thing healthy.

> **Status: In active production.** The pipeline has produced real, published videos. It's continuously being hardened and extended.

## Why I built this

Running a YouTube channel solo means every video is a pipeline: research → script → voiceover → edit plan → packaging → QC. Doing all of that by hand is the bottleneck. So I built the pipeline as a multi-agent system where each stage is owned by a specialist agent — and, critically, a self-healing "medic" agent that detects when a stage stalls or produces a broken artifact and either fixes it or escalates.

The interesting engineering problem isn't any single agent — it's making a reliable multi-agent system that survives real-world failures (a missing file, a stalled step, an external tool going offline) without silently producing garbage or overrunning a human approval gate.

## The agents

- **Scout** — Researches the topic and produces a brief: the hook, key facts, community pulse, competitor gaps, and segment suggestions.
- **Writer** — Turns the brief into a full script and a clean voiceover script.
- **Editor** — Builds an edit plan timed against the real voiceover length.
- **Packager** — Generates titles, thumbnail concepts, description, tags, and an upload checklist.
- **Showrunner** — Orchestrates the run and produces the final QC verdict.
- **Medic** — Health-checks every stage against an expected-artifact matrix, auto-repairs the specific broken step when it's safe, and escalates when it isn't.

## How it works

The pipeline treats a shared file store as its state machine — a topic's current stage is fully determined by which artifacts exist on disk, their status headers, and their timestamps. No stage trusts a previous conversation's claim that something happened; it re-derives state from the files every time.

## Engineering principles

- **State lives in artifacts, not memory.** The folders *are* the state machine; any agent can restart and re-derive where things stand from disk.
- **Monitor → Detect → Diagnose → Recover → Verify.** The medic classifies the failure first (missing file vs. malformed output vs. offline external tool vs. pending human decision) and matches the recovery to the actual problem.
- **Bounded retries with a circuit breaker.** A broken step is auto-retried at most once; two consecutive failures stop and escalate instead of spinning.
- **Human gates are never bypassed.** Script approval, footage-sourcing decisions, and the final QC verdict always wait for a human. The system never fabricates an artifact (for example, faking voiceover audio if the text-to-speech tool is offline).
- **Append-only shared log** as the single source of truth for what's already been done.

## Tech stack

- **Agents:** Claude / Claude Code custom agents
- **State store:** File-based (Markdown artifacts + status headers + timestamps)
- **Integrations:** MCP tools; text-to-speech for voiceover generation
- **Orchestration:** Stage-gated workflow with a self-healing repair agent

## What it produces

End-to-end, the pipeline turns a single topic into: a research brief, a full script, a generated voiceover, a timed edit plan, complete upload packaging, and a QC verdict — leaving the creator to capture gameplay footage and do the final creative edit.

## About

Designed and operated by **Kenneth Henry** — AI Solutions Architect focused on building reliable, production-grade multi-agent AI systems.

- LinkedIn: https://www.linkedin.com/in/kenneth-henry-29b8b285/
- GitHub: https://github.com/Alkenmist-Grimwore
