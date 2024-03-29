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

### Opcional - Tendo acesso a pasta de aplicativos do Windows 10
Abra uma janela do terminal como administrador e execute os seguintes comandos
```cmd
takeown /F "%ProgramFiles%\WindowsApps"
takeown /F "%ProgramFiles%\WindowsApps" /r /d y
icacls "%ProgramFiles%\WindowsApps" /grant Administradores:F
icacls "%ProgramFiles%\WindowsApps" /grant Administradores:F /t
icacls "%ProgramFiles%\WindowsApps" /setowner "NT Service\TrustedInstaller"
```


### Método nutella
```cmd
curl.exe -L -o Debian.appx  https://aka.ms/wsl-debian-gnulinux
```
Descompacte o arquivo baixado em alguma pasta que você escolhe para armazenar a Debian
```cmd
powershell Add-AppxPackage .\Debian.appx
```

Atualize os componentes do Subsistema Linux do Windows antes de iniciar
```cmd
wsl --update
;Verificando atualizações...
;Baixando atualizações...
;Instalando atualizações...
;Esta alteração entrará em vigor na próxima reinicialização completa do WSL. Para forçar uma reinicialização, execute "wsl --shutdown".
;Versão do kernel: 5.10.60.1
```

Reinicie o Subsistema Linux do Windows
```cmd
wsl --shutdown
```

Finalmente execute a primeira inicialização da sua imagem Debian baixada
```cmd
Debian.exe
;Installing, this may take a few minutes...
;Please create a default UNIX user account. The username does not need to match your Windows username.
;For more information visit: https://aka.ms/wslusers
;Enter new UNIX username: (coloque aqui um nome de usuário que pode ser o seu do windows mesmo)
;New password:
;Retype new password:
;passwd: password updated successfully
;Installation successful!
```
Você irá se deparar com o terminal do Linux. Use-o. Para sair, basta digitar exit

#### Observação - Instaladores criados pelo pacote que você baixou

Só para você saber: neste método padrão, sua o pacote do linux irá gerar instaladores
na pasta **C:\Program Files\WindowsApps\ **.
O pacote instala imagens para várias arquiteturas, então são criadas pastas começando
com o nome **TheDebianProject.DebianGNULinux_alguma_coisa** acrescentado da versão e da arquitetura e um
hash no final. Por exemplo, a minha instalação ficou em:
```cmd
C:\Program Files\WindowsApps\TheDebianProject.DebianGNULinux_1.11.1.0_x64__76v4gfsz19hv4\
```

#### Observação - O local de sua instalação:
O local de uma instalção normalmente fica no diretório **%userprofile%\AppData\Local\Packages**. O caminho completo de cada instalação você encontra executando o seguinte comando dentro do powershell:
```powershell
(Get-ChildItem HKCU:\Software\Microsoft\Windows\CurrentVersion\Lxss | ForEach-Object {Get-ItemProperty $_.PSPath}) | select DistributionName, @{n="Path";e={$_.BasePath + "\rootfs"}}
```
Dentro do caminho você vai encontrar um disco VHDX chamado **ext4.vhdx**. 

#### Acessando o wsl via rede pelo explorer
```cmd
\\wsl$\Debian
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

```cmd
wsl.exe --import Meu_Debian %USERPROFILE%\images\ .\install.tar.gz
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


