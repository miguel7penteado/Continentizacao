# Utilização de `Containers` em Sistemas Operacionais 

## Evoluindo a técnica de virtualização, segue o uso de containers

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



