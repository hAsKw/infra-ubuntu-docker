# Servidor Pessoal com Ubuntu, Docker, Portainer, Vaultwarden e Tailscale

Este documento é um guia para configurar um servidor pessoal utilizando Ubuntu Server em um notebook, acessado via Tailscale, com Docker, Portainer e Vaultwarden.

# Outros guias

- [Configuração em VPS](README_VPS.md)  
- [Como configurar domínio e HTTPS](DOMINIO.md)  


1. Atualizar o sistema:
```
sudo apt update && sudo apt upgrade -y
```

2. Conectar ao Tailscale:
```
sudo tailscale up
tailscale status
tailscale ip
```
O Tailscale cria uma rede privada para acessar o servidor de qualquer lugar.

3. Instalar o Docker:
```
sudo apt install docker.io -y
sudo systemctl enable docker
sudo systemctl start docker
```

4. Instalar o Portainer:
```
docker volume create portainer_data

docker run -d   -p 8000:8000   -p 9443:9443   --name portainer   --restart=always   -v /var/run/docker.sock:/var/run/docker.sock   -v portainer_data:/data   portainer/portainer-ce:latest
```
Acessar no navegador:
```
https://<tailscale-ip>:9443
```

5. Instalar o Vaultwarden pelo Portainer:
- Criar volume de dados (ex: vaultwarden_data)
- Configurar o container apontando esse volume para /data
- Definir a porta de acesso (ex: 8080)

6. Ativar HTTPS com Tailscale Funnel (OBRIGATÓRIO para o Vaultwarden funcionar):
```
tailscale funnel 8080
```
Isso cria um túnel seguro com certificado SSL automático.  
O Funnel é essencial porque o Vaultwarden exige HTTPS.  
Como não há um domínio válido, o Tailscale fornece uma URL segura para acessar o Vaultwarden de fora.

Resumo:
- Ubuntu Server atualizado
- Tailscale para VPN
- Docker para containers
- Portainer para gerenciar via web
- Vaultwarden como cofre de senhas
- Tailscale Funnel garantindo HTTPS sem precisar de domínio
