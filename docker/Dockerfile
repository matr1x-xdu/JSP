FROM python:3.12-slim AS builder

WORKDIR /app

ENV PATH=/root/.local/bin:$PATH
RUN pip install pipx && \
    pipx ensurepath && \
    pipx install poetry
RUN apt update && apt install -y git

RUN apt-get update --fix-missing && \
    apt-get install -y --no-install-recommends \
        git build-essential cmake pkg-config libssl-dev && \
    apt-get clean && rm -rf /var/lib/apt/lists/*
    
COPY . .

RUN poetry lock && \
poetry self add poetry-dynamic-versioning && \
    poetry config virtualenvs.in-project true && \
    poetry install && \
    rm -rf /app/.git


FROM python:3.12-slim as runner

WORKDIR /app

COPY --from=builder /app/ /app/

ENTRYPOINT ["/app/.venv/bin/javsp"]
CMD ["-i", "/video"]
