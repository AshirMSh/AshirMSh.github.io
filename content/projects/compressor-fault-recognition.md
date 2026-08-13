---
title: "Machine Learning-Based Fault Recognition for Industrial Compressors"
date: 2023-01-01
draft: false
tags: ["machine-learning", "lstm", "embedded", "stm32"]
summary: "An LSTM model for compressor fault detection via acoustic signals, optimized for STM32F7xx microcontrollers at 98.7% accuracy."
showTableOfContents: false
---

## Overview

An LSTM-based classifier that detects developing faults in industrial compressors from acoustic signals, designed to run directly on embedded hardware.

## Approach

- Collected and labeled acoustic signatures from healthy and faulty compressor operating conditions.
- Trained an LSTM network to classify fault modes from time-series acoustic features.
- Optimized and deployed the model on **STM32F7xx** microcontrollers for on‑device inference.

## Outcomes

- Achieved **98.7%** classification accuracy on held-out test data.
- Enabled low-cost, real-time condition monitoring without cloud connectivity.
