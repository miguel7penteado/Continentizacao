# Utilização de `Containers` em Sistemas Operacionais 

# Evoluindo a técnica de virtualização, segue o uso de containers

# Utilizando Containers em Sistemas Operacionais Linux

## Requisitos do kernel do Sistema Operacional

## Isolamento ao nível de processos

### Instalação de requisitos para containers `docker` no Linux Debian, Ubunto e Devuan

```bash

# Removendo pacotes anteriores do gerenciador Docker
sudo apt-get remove docker docker-engine docker.io

# Adicionando pacotes dependências debian/ubuntu/devuan 
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common

# Obtendo certificado GPG do repositório docker
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

```
#### Se estiver usando o Debian ou o Devuan

```bash
# Adicionando no catálogo do APT o repositório do docker
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
```

#### Se estiver usando o Ubuntu

```bash
# Adicionando no catálogo do APT o repositório do docker
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

### Instalação do gestor de containers `docker` no Linux

```bash
sudo apt-get install docker-ce
```

## Painel WEB de gerência de Container no computador

### Instalando um painel web de gerência de containers: `Portainer`

#### Criando um volume de container para o painel web utilizar:

```bash
docker volume create portainer_data
```

#### Fazendo container contendo o painel web Portainer:
Vamos colocar o painel para ser acessado externamente na porta 2727.
```bash
docker run --name portainer -d \
-p 2727:9000 \
-v /var/run/docker.sock:/var/run/docker.sock \
-v portainer_data:/data portainer/portainer
```

# Utilizando Containers em Sistemas Operacionais Windows

## Isolamento ao nível de processos

## Requisitos do kernel do Sistema Operacional

## Ativando os requisitos nativos do windows 10 atualização 21h2

Precisamos ativar/ativar o subsistema Linux do Windows
```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```
Agora ative todo o recurso de virtualização presente no Windows 10
```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
Agora torne padrão a versão 2 do subsistema Linux do Windows para rodar qualquer imagem do Linux que você tenha instalado
```
wsl --set-default-version 2
```

## Baixando imagens "Linux" para uso de containers "Linux"

### Método nutella
```cmd
curl.exe -L -o Debian.appx  https://aka.ms/wsl-debian-gnulinux
```
Descompacte o arquivo baixado em alguma pasta que você escolhe para armazenar a Debian
```cmd
powershell Add-AppxPackage .\Debian.appx
```

### Método raiz
Baixando imagem do Debian

Baixando pelo DOS/CMD
```cmd
mkdir %USERPROFILE%\images
cd    %USERPROFILE%\images
mkdir %tmp%\download1
cd    %tmp%\download1
curl.exe -L -o Debian.appx  https://aka.ms/wsl-debian-gnulinux

```
Baixando pelo powershell
```powershell
mkdir %USERPROFILE%\images
cd    %USERPROFILE%\images
Invoke-WebRequest -Uri  https://aka.ms/wsl-debian-gnulinux -OutFile PacotaoDebian.appx -UseBasicParsing
```

Descompacte o arquivo baixado em alguma pasta que você escolhe para armazenar a Debian
```powershell
Rename-Item .\PacotaoDebian.appx .\PacotaoDebian.zip
Expand-Archive .\PacotaoDebian.zip .\PacotaoDebian
cd PacotaoDebian
# procure por DistroLauncher-Appx_1.11.1.0_x64.appx
Rename-Item .\Debian.appx .\Debian.zip
Expand-Archive .\Debian.zip .\Debian
move Debian %USERPROFILE%\images\
cd %USERPROFILE%\images\
```
Obtenha o PATH do usuário que está executando este procedimento
```powershell
$userenv = [System.Environment]::GetEnvironmentVariable("Path", [System.EnvironmentVariableTarget]::User)
```
Agora insira na variável de ambiente PATH o caminho de instalação da sua distribuição
```powershell
[System.Environment]::SetEnvironmentVariable("PATH", $userenv + ";%USERPROFILE%\images\Debian", [System.EnvironmentVariableTarget]::User)
```

## Baixando imagens "windows" para uso de containers "windows"

```cmd
ver
```

```cmd
docker pull mcr.microsoft.com/windows/nanoserver:10.0.19042.1348
```

```cmd
docker pull mcr.microsoft.com/windows/servercore:10.0.19042.1348
```


