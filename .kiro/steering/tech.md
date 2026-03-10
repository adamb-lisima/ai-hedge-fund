# Tech Stack

## Backend (Python)
- Python 3.11+
- FastAPI for the REST API (`app/backend/`)
- Uvicorn as the ASGI server
- SQLAlchemy ORM with SQLite (`hedge_fund.db`)
- Alembic for database migrations
- Pydantic v2 for request/response schemas
- LangChain + LangGraph for agent orchestration
- Poetry for dependency management (root `pyproject.toml`)

## Frontend (TypeScript/React)
- React 18 with TypeScript
- Vite as the build tool and dev server
- @xyflow/react (React Flow) for the node-graph editor
- Tailwind CSS 3 for styling
- shadcn/ui (New York style) with Radix UI primitives
- lucide-react for icons
- react-resizable-panels for layout panels
- sonner for toast notifications
- next-themes for dark/light mode
- ESLint with TypeScript and React hooks plugins

## CLI Core (`src/`)
- LangChain + LangGraph agent graph
- pandas/numpy for data processing
- Rich + Colorama for terminal output
- Questionary for interactive CLI prompts

## Infrastructure
- Docker support via `docker/` directory
- CORS configured for localhost:5173 (frontend dev server)
- SSE (Server-Sent Events) for streaming run progress to the frontend

## Common Commands

### Python / Backend
```bash
# Install all Python dependencies (from root)
poetry install

# Run the CLI hedge fund
poetry run python src/main.py --ticker AAPL,MSFT,NVDA

# Run the CLI backtester
poetry run python src/backtester.py --ticker AAPL,MSFT,NVDA

# Start the backend dev server
cd app/backend
poetry run uvicorn main:app --reload

# Run tests
poetry run pytest

# Format code
poetry run black .
poetry run isort .

# Lint
poetry run flake8
```

### Frontend
```bash
cd app/frontend

# Install dependencies
npm install

# Start dev server
npm run dev

# Build for production
tsc && npm run build

# Lint
npm run lint
```

### Quick Start (both frontend + backend)
```bash
# From repo root
./run.sh
# Or on Windows: run.bat
# Or: cd app && npm install && npm run setup
```

## Code Style
- Python: black (line-length 420), isort (black profile)
- TypeScript: ESLint with @typescript-eslint, react-hooks, react-refresh plugins
- Path aliases in frontend: `@/` maps to `src/` (e.g. `@/components`, `@/hooks`, `@/lib`)
