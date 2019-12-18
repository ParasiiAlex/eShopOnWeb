# RUN ALL CONTAINERS FROM ROOT (folder with .sln file):
# docker-compose build
# docker-compose up
#
# RUN JUST THIS CONTAINER FROM ROOT (folder with .sln file):
# docker build --pull -t web -f src/Web/Dockerfile .
#
# RUN COMMAND
#  docker run --name eshopweb --rm -it -p 5106:5106 web
FROM mcr.microsoft.com/dotnet/core/sdk:3.0 AS build
WORKDIR /app

COPY *.sln .
COPY . .
WORKDIR /app/src/Web
RUN dotnet restore

#COPY . .
#RUN dotnet build eShopOnWeb.sln

FROM build AS testrunner
WORKDIR /app/tests/UnitTests
CMD ["dotnet", "test", "--logger:trx"]

# run the unit tests
FROM build AS test
# set the directory to be within the unit test project
WORKDIR /app/tests/UnitTests
# run the unit tests
RUN dotnet test --logger:trx

RUN dotnet publish -c Release -o out

FROM mcr.microsoft.com/dotnet/core/aspnet:3.0 AS runtime
WORKDIR /app
COPY --from=build /app/src/Web/out ./

# Optional: Set this here if not setting it from docker-compose.yml
# ENV ASPNETCORE_ENVIRONMENT Development

ENTRYPOINT ["dotnet", "Web.dll"]


