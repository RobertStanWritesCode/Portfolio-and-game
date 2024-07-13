#build stage
FROM node:22-slim AS dependencies
WORKDIR /app
#copy only package.json and package-lock.json to leverage docker build cache
COPY package.json package-lock.json ./
#install dependencies
RUN npm install --frozen-lockfile

FROM node:22-slim AS builder
WORKDIR /app
#copy dependencies
COPY --from=dependencies /app/node_modules ./node_modules
#copy whole of app
COPY . .

#create production image
FROM node:22-slim AS runner
WORKDIR /app
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/package.json ./
COPY --from=builder /app/public ./public

#install production dependencies
RUN npm install --production --frozen-lockfile
EXPOSE 3000

