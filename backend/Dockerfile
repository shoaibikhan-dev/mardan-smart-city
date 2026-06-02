
FROM node:20-alpine AS builder

WORKDIR /usr/src/app


COPY package*.json ./


RUN npm ci --omit=dev --ignore-scripts

COPY . .


FROM gcr.io/distroless/nodejs20-debian12 AS runner


WORKDIR /usr/src/app


COPY --from=builder /usr/src/app/node_modules ./node_modules


COPY --from=builder /usr/src/app/server.js ./server.js
COPY --from=builder /usr/src/app/src ./src


USER nonroot:nonroot

EXPOSE 5000

ENV NODE_NODE=production \
    PORT=5000


HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD node -e "require('http').get('http://localhost:5000/healthz', (r) => r.statusCode === 200 ? process.exit(0) : process.exit(1))"


CMD ["server.js"]
