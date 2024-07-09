FROM  node:18.20.2-alpine AS builder

WORKDIR /app

COPY package.json .

ENV NODE_OPTIONS="--max-old-space-size=4096"

RUN npm install

RUN npm i -g serve

COPY . .

RUN npm run build

EXPOSE 3000

CMD ["serve", "-s", "dist"]

