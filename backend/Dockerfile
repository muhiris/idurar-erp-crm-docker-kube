FROM node:20-alpine as build

WORKDIR /app/backend
COPY . .

RUN npm install

FROM node:20-alpine as production

WORKDIR /app/backend

COPY --from=build /app/backend .

EXPOSE 8888

CMD ["npm", "run", "dev"]
