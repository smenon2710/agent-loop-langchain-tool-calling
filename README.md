# LangChain Tool-Calling Agent

A small Python example of a manual agent loop using LangChain's `.bind_tools()` and a local Ollama model (`qwen3:1.7b`). The model selects tools, receives their results, and generates an answer, with a limit of 10 iterations.

The demo asks for the price of a laptop after a gold discount. Its tools look up fixed catalog prices and apply bronze (5%), silver (12%), or gold (23%) discounts. The expected laptop total is **$1,000.99**.

## Run locally

You need Python 3.12.2 or newer, `uv`, and Ollama installed and running.

```bash
git clone https://github.com/smenon2710/agent-loop-langchain-tool-calling.git
cd agent-loop-langchain-tool-calling
ollama pull qwen3:1.7b
uv sync --locked
uv run main.py
```

The script prints each iteration, tool call, tool result, and the final response. Edit the question at the bottom of `main.py` to try another request, or change `MODEL` to use another Ollama model that supports tool calling.

## Optional tracing

To send traces to LangSmith, create a local `.env` file:

```dotenv
LANGSMITH_TRACING=true
LANGSMITH_API_KEY=your_langsmith_api_key
LANGSMITH_PROJECT=agent-loop-langchain-tool-calling
```

Tracing is optional; set `LANGSMITH_TRACING=false` to disable it. The script flushes pending traces before exiting. `.env` and `.venv` are excluded from Git; keep credentials out of source files.

## Demo limits

The loop executes only the first tool call in each model response. Responses containing multiple tool calls are not fully handled. Tool order is requested through the prompt, so results depend on the model following those instructions.
