FROM node:18

WORKDIR /app

COPY package.json package-lock.json ./

RUN npm config set registry https://registry.npmmirror.com
RUN npm config set audit false
RUN npm config set fund false
RUN npm config set fetch-retries 10
RUN npm config set fetch-retry-mintimeout 30000
RUN npm config set fetch-retry-maxtimeout 180000

RUN npm install --no-audit --no-fund

COPY . .

RUN npm run build
RUN npm install --global http-server

CMD ["npx", "http-server", "-p", "8000", "/app/build"]