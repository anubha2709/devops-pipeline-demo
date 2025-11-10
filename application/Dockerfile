# ============= Stage 1: Build ======================

FROM node:22-slim AS builder

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY src ./src

# ============= Stage 2: Runtime ====================

FROM node:22-alpine

WORKDIR /app

COPY --from=builder /app /app

EXPOSE 3000

HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
	CMD wget --no-verbose --tries=1 --spider http://localhost:3000/health

CMD ["node", "src/app.js"]

