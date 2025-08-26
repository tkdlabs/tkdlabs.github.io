+++
date = '2025-08-25T16:51:05-06:00'
draft = false
title = "Summarize with local AI"
tags = ["ai"]
+++

{{< figure src=workflow.png >}}

In this tutorial, I'll present a simple way to auto-transcribe and summarize your media using local AI.

### Example use

Create a summary of a podcast episode and a transcript to view before listening. Figure out if you want to spend next N hours on this episode and if it delivers what you want.

### Required tools
  1. Python3
  1. Whisper
  1. KoboldCpp
  1. Local LLM, eg. Gemma 27b.
  1. Prefect

### Architecture

We will be using Prefect to orchestrate the actions.


### Transcribing the media

Whisper is used to generate .txt files with the transcript from audio/video file.


### Summarizing the transcript

We will connect to a local LLM using KoboldCpp API to ask a local LLM to summarize the transcript. You can alternatively use the API of your favorite LLM provider to do the same.

### Storing the summary

The summary will be stored locally in the MD file.


### Prefect: Putting it all together
