---
title: "CI/CD & HIL Verification Framework for 800V DC Microgrids"
date: 2025-07-01
draft: false
tags: ["ci-cd", "model-based-design", "hil-testing", "data-centers"]
summary: "A scalable CI/CD and Hardware-in-the-Loop validation framework for 200K+ LOC PLC/controller projects, cutting validation time by 80%."
showTableOfContents: false
---

## Overview

While architecting next‑generation 800V DC microgrid and DC UPS solutions at Schneider Electric, I designed and implemented a CI/CD pipeline (GitHub Actions, Docker, RaaS) alongside a scalable verification and validation framework for 200K+ lines‑of‑code controller projects.

## What it does

- Automates PLC code generation and static/dynamic verification on every commit.
- Deploys controller software to **Speedgoat** real‑time hardware for Hardware‑in‑the‑Loop (HIL) testing — timing analysis, fault injection, and requirements‑based verification.
- Provides cross‑functional teams with a repeatable, documented HIL testing methodology.

## Outcomes

- Reduced PLC code generation and validation time by **up to 80%**.
- Helped secure **€50M** in R&D funding for the underlying 800V DC microgrid and DC UPS program.
