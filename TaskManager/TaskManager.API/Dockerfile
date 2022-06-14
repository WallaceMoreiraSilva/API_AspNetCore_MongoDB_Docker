#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

# Estagio 1 
	#Esta puchando a imagem do aspnet 5. 
	#Workdir é nosso diretorio de trabalho que ele chamou de /app. 
	#Vai expor o container na porta 80
FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80

# Estagio 2 
	# Puxa uma nova imagem maior apontando para o sdk. 
	# Repare que ele cria um outro diretorio de trabalho /src. Copia o arquivo do projeto para a pasta que ele ta criando
	# run dotnet restore para restaurar os pacotes e referencias. Copy .. copia do diretorio atual para o outro
	# WORKDIR /src/TaskManager.API => define um novo diretorio que é uma subpasta dentro do /src
	# em resumo aqui ele copia de um lugar para outro, restaura os pacotes e compila
FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["TaskManager.API/TaskManager.API.csproj", "TaskManager.API/"]
RUN dotnet restore "TaskManager.API/TaskManager.API.csproj"
COPY . .
WORKDIR "/src/TaskManager.API"
RUN dotnet build "TaskManager.API.csproj" -c Release -o /app/build

# Estagio 3
	# Faz o publish
FROM build AS publish
RUN dotnet publish "TaskManager.API.csproj" -c Release -o /app/publish

# Estagio 4
	# Vai copiar todos os arquivos publish do estagio 3 para /app/publish .
	# depois que é copiado os arquivos, ele vai colocar um entrypoint para executar o comando dotnet. 
	# Isso serve para executar o executavel
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "TaskManager.API.dll"]