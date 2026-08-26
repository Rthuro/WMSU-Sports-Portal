FROM node:22-slim AS builder

WORKDIR /app

COPY package*.json ./
COPY apps/backend/package*.json ./apps/backend/
COPY packages ./packages

RUN npm ci

COPY apps/backend ./apps/backend

RUN npm run build --workspace=backend


FROM node:22-slim AS runner

WORKDIR /app

ENV NODE_ENV=production
ENV PORT=10000

COPY package*.json ./
COPY apps/backend/package*.json ./apps/backend/
COPY packages ./packages

RUN npm ci --omit=dev

COPY --from=builder /app/apps/backend/dist ./apps/backend/dist

EXPOSE 10000

USER node

CMD ["node", "apps/backend/dist/server.js"]