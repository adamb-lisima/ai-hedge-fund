# Product Overview

AI Hedge Fund is an educational proof-of-concept for AI-powered trading decisions. It is not intended for real trading or investment.

The system orchestrates multiple AI "analyst" agents (modeled after famous investors like Warren Buffett, Charlie Munger, Cathie Wood, etc.) alongside technical agents (fundamentals, sentiment, technicals, valuation) to analyze stocks. A Risk Manager sets position limits and a Portfolio Manager makes final trading decisions.

There are two interfaces:
- A CLI (`src/main.py`) for running the hedge fund and backtester directly from the terminal
- A full-stack web app (`app/`) with a React frontend and FastAPI backend, providing a visual node-graph editor for composing and running agent workflows

The web app uses a flow-based UI (React Flow) where users drag-and-drop agent nodes, connect them into a graph, configure LLM models per agent, and run simulations. Results stream back via SSE.

Key domain concepts:
- Flows: saved agent graph configurations (nodes, edges, viewport)
- Flow Runs: execution instances of a flow, tracking status and results
- Agents: AI analyst personas that analyze stocks and produce trading signals
- Backtesting: historical simulation of trading strategies over a date range
- Multiple LLM providers supported: OpenAI, Anthropic, Groq, DeepSeek, Google Gemini, xAI, GigaChat, Ollama (local)
