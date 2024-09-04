# base
FROM node:17.9.0 AS base
WORKDIR /usr/src/app
# copy package files
#COPY package*.json ./    
# normally install would look like this
#RUN --mount=type=secret,id=npmrc,target=/usr/src/app/.npmrc # npm install
# for testing purpose we do it like this
RUN --mount=type=secret,id=npmrc,target=/usr/src/app/.npmrc cp /usr/src/app/.npmrc /root/.npmrc
# copy the rest of the files
#COPY . .

# for lint
FROM base AS linter
WORKDIR /usr/src/app
RUN npm run lint

# for build
FROM linter AS builder
WORKDIR /usr/src/app
RUN npm run build

# for production
FROM node:17.9.0-alpine3.15
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install --only=production
COPY --from=builder /usr/src/app/dist ./
EXPOSE 3000
ENTRYPOINT ["node","./app.js"]