---
title: "Korean Flashcards"
date: 2025-12-30
draft: false
---

A web-based application for downloading Anki-packaged collections of Korean flashcards, containing almost 5000 cards with audio items.

[Launch Korean Flashcards →](https://korean.tkdlabs.com)

## Features

- **Downloadable Anki Package**: Complete .apkg3 collection ready to import into Anki
- **Extensive Content**: Nearly 5000 flashcards with audio files
- **Preview System**: Interactive card previews with audio playback, showing exactly how cards will appear in Anki
- **Comprehensive Learning Paths**:
  - TOPIK1 vocabulary
  - 500 most common Korean phrases
  - Hangul letters

## Technology

### Frontend
- Custom static web page generation for fast, lightweight delivery

### Backend
- **Python-based workflow system** with:
  - Custom reusable framework for generating Anki collections
  - Batching support for efficient processing
  - Resume capability for interrupted work
  - Error recovery mechanisms
- **Switchable AI backends** for content generation:
  - vLLM
  - ChatterBox
  - Gemini Cloud TTS
  - OpenAI TTS
  - OpenAI LLM
- **Custom Anki package editor** written in Python for managing and creating .apkg files
