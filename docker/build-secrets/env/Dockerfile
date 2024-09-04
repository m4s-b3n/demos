# builder
FROM python:3.11-buster AS builder
RUN pip install poetry==1.4.2
ENV POETRY_NO_INTERACTION=1 \
    POETRY_VIRTUALENVS_IN_PROJECT=1 \
    POETRY_VIRTUALENVS_CREATE=1 \
    POETRY_CACHE_DIR=/tmp/poetry_cache
WORKDIR /app
# copy project files
#COPY pyproject.toml poetry.lock ./
RUN --mount=type=secret,id=username \
    --mount=type=secret,id=password \
    poetry config http-basic.myprivaterepository \
    $(cat /run/secrets/username) \
    $(cat /run/secrets/password)
# do the install here
# RUN poetry install --without dev --no-root && rm -rf $POETRY_CACHE_DIR

# final
FROM python:3.11-slim-buster
ENV VIRTUAL_ENV=/app/.venv \
    PATH="/app/.venv/bin:$PATH"
# copy the virtual environment from the builder    
# COPY --from=builder ${VIRTUAL_ENV} ${VIRTUAL_ENV}
# copy the project files
# COPY myproject ./myproject
ENTRYPOINT ["python", "-m", "myproject.main"]