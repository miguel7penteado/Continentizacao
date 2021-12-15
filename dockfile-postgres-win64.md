Dockerfile
```cmd
http://www.macoratti.net/19/02/dock_imgfile1.htm
```

```dockfile
FROM mcr.microsoft.com/windows/nanoserver:20H2 AS download

#SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

#ENV PG_VERSION 14.1-1
RUN curl -O https://get.enterprisedb.com/postgresql/postgresql-14.1-1-windows-x64-binaries.zip && tar -xf postgresql-14.1-1-windows-x64-binaries.zip -C "C:\." && del /q postgresql-14.1-1-windows-x64-binaries.zip
RUN curl -O https://download.visualstudio.microsoft.com/download/pr/3b070396-b7fb-4eee-aa8b-102a23c3e4f4/40EA2955391C9EAE3E35619C4C24B5AAF3D17AEAA6D09424EE9672AA9372AEED/VC_redist.x64.exe && VC_redist.x64.exe /install /passive /norestart
FROM mcr.microsoft.com/windows/nanoserver:20H2

COPY --from=download /pgsql /pgsql
COPY --from=download /windows/system32/msvcp120.dll /pgsql/bin/msvcp120.dll
COPY --from=download /windows/system32/msvcr120.dll /pgsql/bin/msvcr120.dll

RUN setx /M PATH "C:\pgsql\bin;%PATH%"

EXPOSE 5432
CMD ["postgres"]
```

Criar Volume
```
docker volume create --name=pgdata
```

DockerBuild
```yaml
version: '3'
services:
  postgres:
	container_name: postgres
	restart: always
	build:
		context: ./postgres
		dockerfile: Dockerfile
	volumes:
		- pgdata:/var/lib/postgresql/data
	ports:
		- "5432:5432"
	environment:
		- POSTGRES_USER=${POSTGRES_USER}
		- POSTGRES_PASSWORD=${POSTGRES_PASS}
	networks:
		- code-network
networks:
  code-network:
     driver: bridge
volumes:
  pgdata:
     external: true
```
