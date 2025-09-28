# Configurar domínio na VPS

Guia de como usar um domínio próprio para acessar os serviços rodando na VPS, como Vaultwarden, Portainer e outros.

1. Registrar um domínio:
   - Pode ser em Registro.br, Namecheap, GoDaddy, Cloudflare ou outro.
   - Exemplo: `meuservidor.com`.

2. Criar subdomínios no painel DNS:
   - Criar registros do tipo **A** apontando para o IP público da VPS.
   - Exemplos:
     - `cofre.meuservidor.com` → IP da VPS (Vaultwarden)
     - `portainer.meuservidor.com` → IP da VPS (Portainer)

3. Subir o Nginx Proxy Manager (NPM):
```
docker volume create npm_data
docker volume create npm_letsencrypt

docker run -d   -p 80:80   -p 81:81   -p 443:443   --name nginx-proxy-manager   --restart=always   -v npm_data:/data   -v npm_letsencrypt:/etc/letsencrypt   jc21/nginx-proxy-manager:latest
```
   - Painel web acessível em `http://IP-da-VPS:81`
   - Usuário: `admin@example.com`
   - Senha: `changeme`

4. Criar Proxy Hosts no NPM:
   - No painel do NPM, criar um novo Proxy Host.
   - Exemplo:
     - Domain Names: `cofre.meuservidor.com`
     - Forward Hostname/IP: `vaultwarden`
     - Forward Port: `80`
   - Marcar **Request a new SSL certificate** e **Force SSL**.

5. Acessar os serviços pelo domínio:
   - Vaultwarden em `https://cofre.meuservidor.com`
   - Portainer em `https://portainer.meuservidor.com`

Resumo:
- Domínio registrado e apontado para o IP da VPS
- Subdomínios criados para cada serviço
- Nginx Proxy Manager gerencia os certificados SSL automaticamente
- Agora os serviços ficam acessíveis por endereços HTTPS válidos
