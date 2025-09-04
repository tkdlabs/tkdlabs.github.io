+++
date = '2025-08-25T16:51:05-06:00'
draft = true
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

We will be using Prefect to orchestrate the actions. [Prefect](http://prefect.io) is an orchestration software that you can use for free eg. if you want to run the workflows and automations on your local machine.

I'll cover later how to create the workflows to put all this together.

### Transcribing the media

For transcribing the media I'll be using Whisper from OpenAI. You can install it using the [instructions here](http://github.com/openai/whisper).
Basically, what you need is the binary that will be used to transcribe the media. Whisper can output subtitle formats like SRT, but I only use TXT
for this use case as I don't need timestamps.

Here is the sketch of the Python code the client.py that I use to interact with Whisper.

I'm using Python virtual environment to manage the whisper module.

```python

from pathlib import Path
import subprocess

WHISPER_ROOT="/path/to/whisper"
WHISPER_BIN=WHISPER_ROOT + "/.venv/bin/whisper"

def transcribe(path : Path) -> Path:
    # --output_format
    output = path.with_suffix(".txt")
    if output.exists():
        return output
    p = subprocess.run([WHISPER_BIN, "--output_format", "txt", 
                        "--output_dir", str(path.parent), str(path)],
                       stdout=subprocess.PIPE, stderr=subprocess.STDOUT)
    output = path.with_suffix(".txt")
    if output.exists():
        return output
    raise Exception(f"Expected transcript at {output} but found none! {p.stdout.decode()}")
```

### Summarizing the transcript

We will connect to a local LLM using KoboldCpp API to ask a local LLM to summarize the transcript. You can alternatively use the API of your favorite LLM provider to do the same.

I use Kobold, because it has built in support for image recognition, if your LLM supports that. Honestly, I started out by using KoboldCpp, and it just stuck with me.

Kobold supports OpenAI style API, but it has some significant limitation. The biggest one is lack of prompt caching and some problems with handling the continuations of the response prompts. The API reference can be found [here](https://koboldai-koboldcpp-tiefighter.hf.space/api) - note that Completions API is not recommended.

Instead I'm using the standard KoboldAI United [/api/v1/generate](https://lite.koboldai.net/koboldcpp_api#/api/v1). There is a lot of parameters to set, but I just copied it from the UI debug logs, when I was running it using the web UI.

```python
@dataclass(kw_only=True)
class GenerationInput:
    """See https://lite.koboldai.net/koboldcpp_api#/api%2Fv1/post_api_v1_generate."""
    max_context_length: int
    max_length: int
    prompt: str
    memory: str # for system prompt
    genkey: str # unique id set by the user
    # Some LLM tweaks
    rep_pen: float = 1.07
    rep_pen_range: int = 360
    rep_pen_slope: float = 0.7
    temperature: float = 1.00
    min_p: float = 0.0
    top_p: float = 0.95
    tok_k: int = 64
    top_a: float = 0.0
    typical: float = 1.0
    tfs: float = 1.0
    dynatemp_range: float = 0.0
    dynatemp_exponent: float = 1.0
    smoothing_factor: float = 0.0
    nsigma: float = 0.0
    sampler_order: list[int] #= field(default_factory=lambda: [6, 0, 1, 3, 4, 2, 5])
    # Token config
    banned_tokens: list[str] #= field(default_factory=lambda: [])
    render_special: bool = False
    trim_stop: bool = True  # removes detected stop_sequences and truncate all text afterwars
    bypass_eos: bool = False
    use_default_badwordsids: bool = False
    stop_sequence: list[str] #= field(default_factory=lambda: ["### Instruction:", "### Response:"]),
    logit_bias: dict[int, float] #= field(default_factory=lambda: {})    # preference for specific token-ids, up to 16
    # Stats
    logprobs: bool = False
    # Undocumented but in the request
    quiet: bool = True # not sure what it is but it's in sample api request.
    presence_penalty: float = 0.0 
    n : int = 1   # investigate if this is a counter
```

There was a bit of a PITA about setting the default for dicts, which if set using the comments, would break toJSON() which we need to send the request to the API.

But basically, the sketch of sending the API request to KoboldCPP using API is as follows:

```python
def ask_kobold_api(genkey, text=None, context_length=4096, max_tokens=300):
    resp = ""
    if text:
        parts = add_p([], text=text)
        messages.append(create_user_content(parts))
    api_request = GenerationInput(
            max_context_length=context_length,   # take it from config
            max_length=max_tokens,
            prompt=generate_kobold_api_prompt(messages),
            memory=generate_kobold_api_memory(messages),
            genkey=genkey,
            # because dataclasses' default factory can't
            # be converted to dict
            sampler_order=[6,0,1,3,4,2,5],
            banned_tokens=[],
            stop_sequence=["### Instruction:", "### Response:"],
            logit_bias={})
    r = requests.post(KOBOLD_API_GENERATE, json=asdict(api_request))
    # TODO: error handling, 503 etc
    resp_obj = r.json()
    for r in resp_obj['results']:
        messages.append(create_ai_content(add(text=r['text'])))
        logging.info(f"Received {r['text']}.")
        resp += r['text']
    return resp
```







### Storing the summary

The summary will be stored locally in the MD file.


### Prefect: Putting it all together
