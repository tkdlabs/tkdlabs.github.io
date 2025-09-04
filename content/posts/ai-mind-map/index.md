+++
date = '2025-09-03T20:51:05-06:00'
draft = false
title = "AI: Visualize podcasts with mind maps"
tags = ["ai"]
+++

{{< figure src=mind_map_title.png >}}

I'll present how to use Google Notebook.LM and a local media-to-transcript tool to scan the podcast episode to generate a structural view of the topics - a mind map - with detailed notes for each "leaf" topic.

### Example

Imagine a ~2h podcast of [Huberman Lab featuring Alex Honnold](https://www.hubermanlab.com/episode/how-to-set-and-achieve-massive-goals-alex-honnold). It would be cool to listen to it, but we don't always have the full 2 hours. I'll show how to use free AI tools to create a tree of logical parts you can expand with auto-generated notes for everything said on each specific topic. 

### Required tools

To do this we will only need 2 tools:

  1. [Whisper from OpenAI](https://github.com/openai/whisper). Their GitHub page has extensive installation guide. You'd use it in command line as a tool. In my example, I'm using Ubuntu Linux and Python with a virtual environment. 
  1. [Notebook.LM from Google](https://notebooklm.google.com/).

### Step 1 - Transcribe

First, download the audio/video that you want to process. How to do it for "Huberman Lab"? You can find the episode e.g., on [PodBean](https://www.podbean.com/media/share/dir-imjm2-27448c76), which allows you to download the audio.

Now with this, you can simply launch 'whisper' to get the transcript of the whole episode. It accepts audio or video formats. You can do this in Terminal roughly in this way:
(I run a Pythonic equivalent via Prefect, but that's a separate post):

```bash
$ cd whisper                  # go to your installation directory
$ source .venv/bin/activate   # activate your python venv
$ whisper [audio.mp3] --output_format txt  
```

In addition to TXT, Whisper can generate several other text formats - e.g., SRT subtitles, which can be useful if you are trying to transcribe videos. It can even translate between certain languages (which I found useful when watching Korean videos and trying to understand them). That's beyond today's scope, but good to know.

### Step 2 - Upload to Notebook.LM

Go to [Notebook.LM by Google](https://notebooklm.google.com/) and create a new notebook.

Upload your txt file just as is. Notebook.LM will update the notebook title, and produce a basic summary. It should roughly like this:

{{< figure src=after_upload_notebook_lm.png >}}

You can ask questions about the episode and it will answer them based on transcript often quoting exact fragments where it happened.

### Step 3 - Generate the mind map

Move over to the right panel. You should see an option to generate a mind map. Click on this box and wait a moment for the tree to be generated.

{{< figure src=create_mind_map.png >}}

### Step 4 - Explore the mind map (and profit).

The mind map is a tree of concepts from the podcast, grouped logically. I find this representation superior to a generic summary or a linear audio overview:

{{< figure src=mind_map.png >}}

  * A general summary can't easily zoom into specific subtopics.
  * An audio summary (known as NotebookLM "podcast") may cut listening down to ~15-20 minutes, but it often treats topics superficially.
  * The mind map gives a rich and structured overview. For example, I can explore "Is climbing an intrinsic or extrinsically motivated?". I can locate the leaf node covering this and click on it - Notebook.LM automatically creates a prompt that outputs a pretty detailed essay of everything said on that point.

{{< figure src=expand_map_leaf.png >}}

This is super useful, because podcasts are linear - you'd need to listen to the whole 2 hours, messy - the topic often is regurgitated across those 2 hours, and hard to remember - often after listening for 2 hours, you barely remember anything, not to mention such detailed hierarchical outline of topics.

For me this is my favorite hack where Notebook.LM is a mighty tool that is simply awesome.

