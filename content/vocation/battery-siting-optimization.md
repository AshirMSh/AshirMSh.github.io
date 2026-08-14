---
title: "Optimal Placement and Sizing of Batteries in Power Networks"
date: 2023-05-01
draft: false
tags: ["python", "optimization", "pulp", "battery-storage"]
summary: "A linear programming model in Python/PuLP for optimal PV and battery sizing, raising renewable share to 90.86% and cutting costs by 56%."
showTableOfContents: false
---

## Overview

A linear programming model built in Python with [PuLP](https://coin-or.github.io/pulp/) to jointly size and place photovoltaic generation and battery storage across a power network.

## Approach

- Formulated the siting and sizing problem as a mixed-integer linear program (MILP).
- Modeled network constraints, PV generation profiles, and battery charge/discharge limits.
- Solved for the configuration that minimizes total cost while maximizing renewable self‑consumption.

## Outcomes

- Raised the renewable energy share to **90.86%**.
- Cut overall system costs by **56%** versus the baseline configuration.

## Links

- Repository: <https://github.com/AshirMSh>
