FROM node:20 AS build

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

#### Multi stage Build ####

RUN npm run build

# Use nginx to serve the built app
FROM nginx:alpine

COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
