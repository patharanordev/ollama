# **ollama**

- [ollama's API](https://ollama.com/library/llama3.2/blobs/dde5aa3fc5ff).

## Prerequisite

1. [Download ollama CLI](https://ollama.com/download).
2. [Select your target model](https://ollama.com/library).
3. Load the model, example I would like to use `llava` to inference my image (this model has 4.1GB):

    ```sh
    ollama run llava
    ```

## Ex. Single model

### llama3.2 (small model: 1B, 3B)

Chat streaming (no streaming is an optional):

```sh
curl --location 'http://localhost:11434/api/chat' \
--header 'Content-Type: application/json' \
--data '{
  "model": "llama3.2",
  "messages": [
    { "role": "user", "content": "why is the sky blue?" }
  ]
}'
```

ex. result :

```json
{
    "model": "llama3.2",
    "created_at": "2024-10-13T06:58:50.99629301Z",
    "message": {
        "role": "assistant",
        "content": "The"
    },
    "done": false
}
{
    "model": "llama3.2",
    "created_at": "2024-10-13T06:58:50.996301466Z",
    "message": {
        "role": "assistant",
        "content": " sky"
    },
    "done": false
}

...

{
    "model": "llama3.2",
    "created_at": "2024-10-13T06:58:55.760420812Z",
    "message": {
        "role": "assistant",
        "content": "."
    },
    "done": false
}
{
    "model": "llama3.2",
    "created_at": "2024-10-13T06:58:55.776495846Z",
    "message": {
        "role": "assistant",
        "content": ""
    },
    "done_reason": "stop",
    "done": true,
    "total_duration": 4930880472,
    "load_duration": 56823000,
    "prompt_eval_count": 31,
    "prompt_eval_duration": 19696000,
    "eval_count": 311,
    "eval_duration": 4808869000
}
```

## Ex. Multi-models (8B+)

Chat with image:

```sh
curl --location 'http://localhost:11434/api/generate' \
--header 'Content-Type: application/json' \
--data '{
  "model": "llava",
  "prompt":"What is in this picture?",
  "stream": false,
  "images": ["iVBORw..."]
}'
```

ex. result:

```json
{
    "model": "llava",
    "created_at": "2024-10-13T07:30:18.544483365Z",
    "response": " The image is of an animated character resembling a cute, happy cartoon pig. It has rosy cheeks, big eyes, and a small tail. ",
    "done": true,
    "done_reason": "stop",
    "context": [733, 16289, ..., 28705],
    "total_duration": 1363666837,
    "load_duration": 13070641,
    "prompt_eval_count": 1,
    "prompt_eval_duration": 407302000,
    "eval_count": 34,
    "eval_duration": 892914000
}
```
