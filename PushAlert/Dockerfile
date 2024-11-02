# Use a imagem oficial do .NET 8 como base para o build
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
EXPOSE 80
WORKDIR /app

# restore
COPY . ./
RUN dotnet restore "PushAlert/PushAlert/PushAlert.csproj"

# build
COPY . ./
RUN dotnet build "PushAlert/PushAlert/PushAlert.csproj" -c Release -o /app/build


# Stage 2: Publish
FROM build as publish
RUN dotnet publish "PushAlert/PushAlert/PushAlert.csproj" -c Release -o /app/publish

# State 3: Run State
FROM nginx:1.23.0-alpine
WORKDIR /usr/share/nginx/html
COPY --from=publish /app/publish/wwwroot /usr/share/nginx/html
COPY "nginx.conf" "/etc/nginx/nginx.conf"
CMD ["nginx", "-g", "daemon off;"]