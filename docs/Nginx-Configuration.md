## Guia de Configuração do Nginx

>**Este guia detalha como instalar, configurar e gerenciar o Nginx em uma instância EC2 na AWS para servir como servidor de arquivos estáticos, proxy reverso e balanceador de carga.**

## ❗ Pré-requisitos

1. **Instância EC2** configurada e acessível via SSH (consulte o <a href="/docs/EC2-Setup.md">**Guia de Configuração do EC2**</a>).
2. Sistema operacional **Ubuntu 22.04 LTS** ou compatível.
3. Pacotes básicos atualizados:
    ```bash
    sudo apt update && sudo apt upgrade -y
    ```
4. **Elastic IP** associado à instância (recomendado).

## ⏩ Etapas

### 1. Instalar o Nginx

1. Conecte-se à sua instância EC2 via SSH.
2. Instale o Nginx com o comando:
    ```bash
    sudo apt install nginx -y
    ```
3. Verifique se o serviço foi iniciado:
    ```bash
    sudo systemctl status nginx
    ```
4. Confirme que o Nginx está funcionando acessando o Elastic IP no navegador. Você deve ver a página padrão do Nginx.

---

### 2. Configuração Básica do Nginx
**Localização dos Arquivos de Configuração:**

- **Arquivo principal**: */etc/nginx/nginx.conf*
- **Configurações específicas do site**: */etc/nginx/sites-available/default*

>**Nota**: As configurações de sites podem ser gerenciadas via links simbólicos em /etc/nginx/sites-enabled.

---

### 3. Configurar o Proxy Reverso

**Modifique o arquivo** */etc/nginx/sites-available/default*:

1. Edite o arquivo com seu editor preferido:
    ```bash
    sudo vim /etc/nginx/sites-available/default
    ```
2. Substitua o conteúdo pelo exemplo abaixo:
    ```bash
    upstream backend {
            server <ip da EC2 "BackEnd"> : <Porta EC2>;     #Server 1
            server <ip da EC2 "BackEnd"> : <Porta EC2>;     #Server 2    Load Balancer
            server <ip da EC2 "BackEnd"> : <Porta EC2>;     #Server 3
    }

    server {
            listen 80;
            server_name <IP da EC2 "FrontEnd" OU IP do Tunel VPN configurado> ;

            #Proxy FrontEnd
            location / {
                    root /var/www/html/frontend; # Sugestão de caminho para Build do Projeto
                    try_files $uri $uri/ /index.html;        
            }

            #Proxy BackEnd
            location /api/ {
                    proxy_pass http://backend;
                    proxy_set_header Host $host;
                    proxy_set_header X-Real-IP $remote_addr;
                    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                    proxy_set_header X-Forwarded-Proto $scheme;
            }
    }
    ```
    Também disponível em: <a href="/examples/nginx/defaut.conf">**Exemplo - defaut.conf**</a>

3. Salve e saia do editor (no vim, pressione ESC, :, w + q, ENTER).

---

### 4. Configurar o Balanceador de Carga
**Atualize o bloco** upstream **no arquivo** */etc/nginx/sites-available/default*:

>O Nginx pode ser configurado para distribuir o tráfego entre várias instâncias backend para balanceamento de carga.

1. No bloco upstream backend, adicione as instâncias backend com seus IPs privados e portas, como no exemplo abaixo:
    ```bash
    upstream backend {
        # Instância Exemplo 1
        server 192.168.0.2:8001;

        # Instância Exemplo 2
        server 192.168.0.3:8002;

        # Instância Exemplo 3
        server 192.168.0.4:8003;

        # Adicione mais instâncias, se necessário
    }
    ```
2. Certifique-se de que o bloco location /api/ está apontando para o grupo backend:
    ```bash
    location /api/ {
        proxy_pass http://backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
    ```

---

### 5. Testar e Reiniciar o Nginx

1. Teste a configuração para garantir que não há erros:
    ```bash
    sudo nginx -t
    ```
2. Reinicie o serviço do Nginx para aplicar as alterações:
    ```bash
    sudo systemctl restart nginx
    ```


## 🎯 Próximos Passos

1. Configure o **VPN** para segurança adicional (consulte o <a href="/docs/VPN-Setup.md">**Guia de Configuração do OpenVPN**</a>).
2. Configure políticas de **CORS** no backend, se necessário.
