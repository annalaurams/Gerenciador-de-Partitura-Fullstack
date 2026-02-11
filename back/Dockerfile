FROM node:18-alpine

WORKDIR /app
# Criar pasta de trabalho - tudo agora acontece dentro de /app

COPY package*.json ./

RUN npm install

# Copiar resto do código
COPY . . 

RUN npx prisma generate

# Compilar TypeScript
RUN npm run build

# Porta que a API usa
EXPOSE 3333

# Comando que inicia o servidor
CMD ["node", "dist/server.js"]
