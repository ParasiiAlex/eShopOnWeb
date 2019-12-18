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

#FROM build AS publish
WORKDIR /app/src/Web
RUN dotnet publish -c Release -o out


WORKDIR /app/src/Web
RUN dotnet build

FROM build AS testrunner
WORKDIR /app/tests/UnitTests
CMD ["dotnet", "test", "--logger:trx"]

# run the unit tests
FROM build AS test
WORKDIR /app/tests/UnitTests
RUN dotnet test --logger:trx


FROM mcr.microsoft.com/dotnet/core/aspnet:3.0 AS runtime
WORKDIR /app
COPY --from=build /app/src/Web/out ./

ENTRYPOINT ["dotnet", "Web.dll"]


