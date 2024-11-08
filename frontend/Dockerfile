# Build stage
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .

# This is the stage that frontend-test will use
FROM builder AS test
CMD ["npm", "test"]

# Development stage
FROM builder AS development
EXPOSE 3000
CMD ["npm", "run", "dev"] 