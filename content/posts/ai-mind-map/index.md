+++
date = '2025-09-03T20:51:05-06:00'
draft = true
title = "Visualize podcasts with mind maps"
tags = ["ai"]
+++

In this short tutorial, I'll present how to use Notebook.LM to disect the podcast episode to obtain a structural view of the covered topics - as a mind map, with detailed information about each "leaf" topic.

### Example

To illustrate where this is useful, imagine an almost 2h podcast of [Huberman Labs interviewing Alex Honnold](https://www.hubermanlab.com/episode/how-to-set-and-achieve-massive-goals-alex-honnold). It would be cool to listen to it, but we don't always have the full 2 hours to do so. I'm going to show how to use ready-made AI products to extract the whole content from it, and being able to jump into interesting sections, with very detailed summaries. 

### Required tools

To do this we will only need 2 tools:

  1. Whisper from OpenAI (https://github.com/openai/whisper). Their GitHub page has extensive installation guide. You'd use it in command line as a tool. In my example, I'm using Ubuntu Linux and Python with a virtual environment. 
  1. Access to Notebook.LM.

### Step one - transcribe

You should download the audio of the media that you want to process. How to do it for "Huberman Lab"? You can find the content eg. on [podbean](https://www.podbean.com/media/share/dir-imjm2-27448c76), which allows you to download the audio.

Now with this, you can simply launch 'whisper' to get the transcript of the whole episode.

```bash
$ cd whisper                  # go to your installation directory
$ source .venv/bin/activate   # activate your python venv
$ whisper [audio.mp3] --output_format txt  
```

Whisper can generate multiple text formats, including the most commonly used subtitles like SRT, which could be useful if you are trying to transcribe videos. It can even translate between certain languages (which I found useful when watching Korean videos and trying to understand them). But that's outside the scope of our goal here.

### Step two - upload to Notebook.LM

Go to [Notebook.LM by Google](https://notebooklm.google.com/) and create a new notebook.

Upload your txt file just as is. Your notebook will get updated title, and you should see some basic summary.

You can ask questions to the assistant, and it will be able to answer them according to what happened in the podcast even quoting specific fragments where it happened.

### Step three - generate mindmap

Move over to the right panel. You should see an option to generate a mind map. Wait a moment for the tree to be generated.

### Step four - explore the mind map and profit.

Mind map is a tree representing various concepts covered in the podcast, that are grouped logically. I find this representation superior to the general overview of the podcast, or the audio overview.

General overview, won't be able to go into details and focus on specific sub-topic easily. And those topics are not easily discoverable without deeply reading the transcript or listening to 2 hours of podcast.

Listening to audio summary would reduce listening time to ~15-20 minutes, but that summary often is very superficial and doesn't go deeply into the topics.

Mind map offers much more rich and structured overview of what was covered. For example, I can explore the area of "is climbing an intrisic or extrinsically motivated activity". I find the leaf node covering this. If I click on that leaf node, Notebook.LM automatically creates a prompt that outputs a pretty detailed essay of everything spoken on this.

This is super useful, because podcasts are linear - you'd need to listen to the whole 2 hours, messy - the topic often is regurgitated across those 2 hours, and hard to remember - often after listening for 2 hours, you barely remember anything, not to mention such detailed hierarchical outline of topics.

For me this is my favorite hack where Notebook.LM is a mighty tool that is simply awesome.

### Appendix - other useful features



