# Uso de una imagen completa (no Alpine), lo que aumenta la superficie de ataque
FROM node:18

WORKDIR /app

COPY . .

# PARCHE: Forzamos la versión 8.3.2 de uuid para evitar el error ERR_REQUIRE_ESM
# Esto demuestra la falta de control de versiones en la línea base
RUN yarn add uuid@8.3.2

RUN yarn install

# Exponemos el puerto
EXPOSE 3000

# Por defecto, esto se ejecutará como el usuario 'root'
CMD ["node", "src/index.js"]