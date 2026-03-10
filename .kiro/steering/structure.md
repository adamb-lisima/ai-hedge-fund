# Project Structure

```
ai-hedge-fund/
├── src/                          # Core Python library (CLI + shared logic)
│   ├── agents/                   # AI analyst agents (one file per persona)
│   ├── backtesting/              # Backtesting engine, portfolio, metrics
│   ├── cli/                      # CLI input handling
│   ├── data/                     # Data models and caching
│   ├── graph/                    # LangGraph agent graph state
│   ├── llm/                      # LLM provider configs and model definitions
│   ├── tools/                    # Financial data API tools
│   ├── utils/                    # Display, progress, API key helpers
│   ├── main.py                   # CLI entry point for hedge fund
│   └── backtester.py             # CLI entry point for backtester
│
├── app/                          # Full-stack web application
│   ├── backend/                  # FastAPI backend
│   │   ├── main.py               # FastAPI app entry point
│   │   ├── routes/               # API route handlers (one file per domain)
│   │   ├── services/             # Business logic (agent, backtest, graph, portfolio)
│   │   ├── models/               # Pydantic schemas and event models
│   │   ├── repositories/         # Database access layer (SQLAlchemy queries)
│   │   ├── database/             # DB connection, SQLAlchemy ORM models
│   │   └── alembic/              # Database migrations
│   │
│   └── frontend/                 # React + Vite frontend
│       └── src/
│           ├── components/       # React components
│           │   ├── ui/           # shadcn/ui primitives (button, dialog, etc.)
│           │   ├── panels/       # Left sidebar, right sidebar, bottom panel
│           │   ├── settings/     # Settings dialogs (API keys, models, appearance)
│           │   ├── tabs/         # Tab bar and tab content
│           │   └── layout/       # App layout shell
│           ├── nodes/            # React Flow node types and components
│           │   └── components/   # Individual node UIs (agent, portfolio, output)
│           ├── edges/            # React Flow edge types
│           ├── contexts/         # React contexts (flow, node, tabs, layout)
│           ├── hooks/            # Custom React hooks
│           ├── services/         # API client and service layers
│           ├── providers/        # Theme provider
│           ├── data/             # Static data (agent defs, model defs, node mappings)
│           ├── types/            # TypeScript type definitions
│           ├── utils/            # Utility functions
│           └── lib/              # Shared lib (cn utility from shadcn)
│
├── tests/                        # Python test suite (pytest)
│   ├── backtesting/              # Backtesting unit tests
│   └── fixtures/                 # Test fixtures
│
├── docker/                       # Docker deployment files
├── pyproject.toml                # Python project config (Poetry)
└── .env.example                  # Environment variable template
```

## Architecture Patterns

### Backend
- Layered architecture: routes → services → repositories → database
- Routes define FastAPI endpoints and delegate to services
- Services contain business logic
- Repositories handle SQLAlchemy queries
- Pydantic models in `models/schemas.py` for all request/response types
- SSE streaming for real-time run progress updates

### Frontend
- Context-based state management (React Context API, no Redux)
- Custom hooks encapsulate reusable logic (`hooks/`)
- Node-based flow editor using @xyflow/react
- Component composition: NodeShell wraps all node types
- Services layer for API communication (`services/`)
- Path alias `@/` resolves to `src/`

### Agent System
- Each agent in `src/agents/` is a self-contained module
- Agents are composed into a LangGraph directed graph
- The web app lets users visually compose agent graphs via React Flow
- Agent graphs are serialized as nodes + edges and sent to the backend for execution
