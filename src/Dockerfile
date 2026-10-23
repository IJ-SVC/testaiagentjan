# syntax=docker/dockerfile:1

FROM node:22-bookworm-slim AS frontend
WORKDIR /code/frontend
RUN corepack enable && corepack prepare pnpm@10.4.1 --activate
COPY frontend/ ./
RUN pnpm install --frozen-lockfile && pnpm build

FROM python:3.13.8-slim-bookworm

WORKDIR /code

COPY . .

RUN pip install --no-cache-dir --upgrade -r requirements.txt

COPY --from=frontend /code/frontend/dist ./frontend/dist

EXPOSE 50505

CMD ["gunicorn", "api.main:create_app"]
