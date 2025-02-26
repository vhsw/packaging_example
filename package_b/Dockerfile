FROM ghcr.io/astral-sh/uv:python3.12-bookworm-slim AS builder
ENV UV_COMPILE_BYTECODE=1 UV_LINK_MODE=copy
ENV UV_PYTHON_DOWNLOADS=0

WORKDIR /app

ADD helpers /helpers

RUN --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=package_b/uv.lock,target=uv.lock \
    --mount=type=bind,source=package_b/pyproject.toml,target=pyproject.toml \
    uv sync --frozen --no-install-project --no-dev

ADD package_b /app
RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync --frozen --no-dev


FROM python:3.12-slim-bookworm
COPY --from=builder --chown=app:app /app /app
ENV PATH="/app/.venv/bin:$PATH"
CMD ["python", "/app/src/main.py"]
