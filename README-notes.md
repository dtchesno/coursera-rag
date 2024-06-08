# Setup

Coursera project - https://www.coursera.org/learn/introduction-to-rag.

## Python, pip, virtualenv

Assume you have Python & Virtualenv installed.

## Coursera examples repo

https://github.com/dtchesno/coursera-rag is forked from https://github.com/alfredodeza/learn-retrieval-augmented-generation.

Few changes done to support execution on WSL and for exploration reasons.

## Install dependencies

Go into repo root, create virtual env, install packages.

```
python3 -m venv .venv
source .venv/bin/activate
.venv/bin/pip install -r requirements.txt
```

NOTE: 'master' requirements.txt didn't work on WSL - there are older versions, which also required change in notebook. Please check notebook changes and comments. Out-of-box master worked fine on Mac.

Download LLM (LLama / LLaVA 1.5 in our case) to be used for generation phase.

```https://github.com/Mozilla-Ocho/llamafile```

# Models used

## Encoding / sentence transfomer

Builds vector(s) / embeddings for your documents and prompt.

```all-MiniLM-L6-v2```
For details and more options - https://www.sbert.net/

## Text Generation (for this project), and more

Provides text genaration / chat capabilities, including RAG scenarios when request includes context from customer data and/or chat history.

```LLama_CPP```
For details:
- LLama repo - https://github.com/Mozilla-Ocho/llamafile
- OpenAI API details - https://platform.openai.com/docs/guides/text-generation

# Execution

## Run LLama model

```./llava-v1.5-7b-q4.llamafile --host 0.0.0.0```

NOTES:
- check cmdline params, https://github.com/ggerganov/llama.cpp/tree/master/examples/server
- host option required when run on WSL to bind on 0.0.0.0 to be available from another WSL session - Jupyter notebook.
- works out-of-box on Mac
- you can chat with model at localhost:8080 from browser

## Run Jupyter Notebook

From repo root:
```
source .venv/bin/activate
python3 -m jupyter notebook
```
You will get URL to open in browser. Use ```examples/3-applied-rag/embeddings.ipynb```.

Default implementation works as expected on Mac with original packages from requirements.txt (see above).

Here are steps to make it work on WSL:
- packages versions are downgraged & python interfaces are a bit different - check cells & comments in notebook
- 127.0.0.1 or localhost is not working for OpenAI connection
    - find WSL 'bridge IP', ```ip route show | grep -i default | awk '{ print $3}'```
    - when run model, use ```--host 0.0.0.0``` (see above)
    - when create OpenAI client use address found from above

You can also use curl for quick test. Assume your bridge is ```172.17.128.1```:

``` curl --request POST --url http://172.17.128.1:8080/completion --data '{"prompt": "Building a website can be done in 10 simple steps:","n_predict": 128}'```


