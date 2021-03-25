# Microsoft SQL Server in Mac

## Installing & Starting through Docker

```sh
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=Maari@2021" \
   -p 1433:1433 --name sql1 -h sql1 \
   -d mcr.microsoft.com/mssql/server:2019-latest
```


## Check if Docker container running

```sh
docker ps
```

## Run query inside SQL Server Docker Container

```sh
sudo docker exec -it sql1 "bash"
```

```sh
/opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P "Maari@2021"
```