# Petals Chat

HTTP endpoint for [BLOOM-176B](https://huggingface.co/bigscience/bloom) inference with the [Petals](https://petals.ml) client

## Interactive Chat

Check out [http://chat.petals.ml](http://chat.petals.ml). It uses the HTTP API described below under the hood (see [static/chat.js](static/chat.js)).

## HTTP API Methods

### POST /api/v1/generate

Parameters:

- **inputs** (optional) - New user inputs. May be omitted if you continue generation in an inference session (see below).
- **do_sample** (optional) - If `0` (default), runs greedy generation. If `1`, performs sampling with parameters below.
- **temperature** (optional)
- **top_k** (optional)
- **top_p** (optional)
- **max_length** - Max length of generated text (including prefix) in tokens.
- **max_new_tokens** - Max number of newly generated tokens (excluding prefix).
- **session_id** (optional) - UUID of an inference session opened earlier (see methods below). This allows you to continue generation later without processing prefix from scratch.

Notes:

- You need to specify either `max_length` or `max_new_tokens`.
- If you'd like to solve downstream tasks in the zero-shot mode, start with `do_sample=0` (default).
- If you'd like to make a chat bot or write a long text, start with `do_sample=1, temperature=0.75, top_p=0.9` (tuning these params may help).

Returns (JSON):

- **ok** (bool)
- **outputs** (str)
- **traceback** (str) - the Python traceback if `ok == False`

### GET /api/v1/open_inference_session

Parameters:

- **max_length** (required)

Returns (JSON):

- **ok** (bool)
- **session_id** (str) - UUID of the opened session
- **traceback** (str) - the Python traceback if `ok == False`

### GET /api/v1/close_inference_session

Parameters:

- **session_id** (required)

Returns (JSON):

- **ok** (bool)
- **traceback** (str) - the Python traceback if `ok == False`