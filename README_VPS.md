# Servidor VPS com Ubuntu, Docker e Portainer

Guia simples para configurar uma VPS com Docker e Portainer.  
A ideia é instalar apenas o necessário via linha de comando (Docker e Portainer).  
Os outros containers serão criados e gerenciados pelo Portainer.

1. Atualizar o sistema:
```
sudo apt update && sudo apt upgrade -y
```

2. Instalar o Docker:
```
sudo apt install docker.io -y
sudo systemctl enable docker
sudo systemctl start docker
```

3. Instalar o Portainer:
```
docker volume create portainer_data

docker run -d   -p 9443:9443   --name portainer   --restart=always   -v /var/run/docker.sock:/var/run/docker.sock   -v portainer_data:/data   portainer/portainer-ce:latest
```

4. Acessar o Portainer:
- Entrar no navegador: `https://IP-da-VPS:9443`
- Criar o usuário administrador
- A partir daqui, todos os outros containers podem ser criados pelo painel do Portainer.

Resumo:
- VPS atualizada
- Docker instalado
- Portainer rodando e acessível via HTTPS na porta 9443
- Todos os outros containers serão configurados pelo Portainer
