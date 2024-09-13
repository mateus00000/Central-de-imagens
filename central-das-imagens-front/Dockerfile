# Primeira etapa de build, utilizando uma imagem base do Node.js versão 18 no Alpine Linux, uma distribuição leve
FROM node:18-alpine AS build

# Define o diretório de trabalho dentro do contêiner
WORKDIR /app

# Copia os arquivos de configuração do npm (package.json e package-lock.json) para o diretório de trabalho no contêiner
COPY package*.json ./

# Executa o comando para instalar as dependências definidas no package.json
RUN npm install

# Copia todo o conteúdo do diretório atual para o diretório de trabalho no contêiner
COPY . .

# Executa o comando de build da aplicação React (ou outra aplicação frontend), gerando os arquivos estáticos para produção
RUN npm run build

# Segunda etapa, para servir os arquivos gerados utilizando o Nginx, uma imagem leve do Nginx no Alpine
FROM nginx:alpine

# Copia o resultado do build (arquivos estáticos) da etapa anterior para o diretório onde o Nginx serve os arquivos HTML
COPY --from=build /app/dist /usr/share/nginx/html

# Copia o arquivo de configuração personalizado do Nginx para a configuração padrão
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expõe a porta 80, que é a porta padrão do Nginx para servir o conteúdo HTTP
EXPOSE 80

# Comando de inicialização do Nginx, que executa o servidor em modo não daemon (foreground)
CMD ["nginx", "-g", "daemon off;"]
