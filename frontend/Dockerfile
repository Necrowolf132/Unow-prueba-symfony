FROM node:18

WORKDIR /usr/src/app

# Instala dependencias
COPY package.json yarn.lock ./
RUN yarn install

# Copia el código del proyecto
COPY . .

# Inicia el servidor de desarrollo
CMD ["yarn", "start"]

EXPOSE 3000
