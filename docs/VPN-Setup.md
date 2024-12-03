## Guia de Configuração do OpenVPN

>**Este documento explica o processo de configuração de uma VPN utilizando OpenVPN, sem o uso de certificados, para conectar um cliente a uma instância EC2 configurada como servidor VPN. O guia inclui etapas para instalação, configuração e teste da VPN.**

## ❗ Pré-requisitos

1. **Instância EC2** configurada e acessível via SSH (consulte o <a href="/docs/EC2-Setup.md">**Guia de Configuração do EC2**</a>).
2. Cliente OpenVPN instalado na sua máquina local:
    - Donwload para Windows: <a href="https://openvpn.net/community-downloads/" target="_blank">**OpenVPN GUI**</a>
3. Instância EC2 FrontEnd na AWS com sistema Ubuntu.
4. Elastic IP associado à instância EC2.
5. Permissões de rede configuradas no grupo de segurança da EC2:
    - Porta **1194/UDP** para o tráfego OpenVPN (consulte o <a href="/docs/EC2-Setup.md">**Guia de Configuração do EC2**</a>)


## ⏩ Etapas

### 1. Instalar o OpenVPN na EC2
Após conectar-se à sua instância EC2 via SSH, instale o OpenVPN com os comandos abaixo:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install openvpn -y
```

---

### 2. Configurar o Servidor OpenVPN

**Criar o arquivo** ```servidor.conf```
1. Crie o arquivo de configuração do servidor OpenVPN na pasta /etc/openvpn/:
    ```bash
    sudo vim /etc/openvpn/server.conf
    ```
2. Exemplo de configuração básica do servidor:
    ```bash
    dev tun
    ifconfig 192.168.0.1    192.168.0.2 #Exemplo de IPs para utilizar
    secret /etc/openvpn/chave
    port 1194
    proto udp
    comp-lzo
    verb 4
    keepalive 10 120
    persist-key
    persist-tun
    float
    cipher AES-256-CBC
    ```
    - **ifconfig** define os IPs do servidor e do cliente na VPN.
    - **secret aponta** para o arquivo da chave compartilhada.
3. Gerar a chave compartilhada
    - Crie a chave compartilhada para criptografia entre cliente e servidor:
        ```bash
        sudo openvpn --genkey --secret /etc/openvpn/server.key
        ```

---

### 3. Configurar o Servidor OpenVPN

**Criar o arquivo** ```cliente.conf```
1. Crie um arquivo de configuração do cliente chamado client.conf:
    ```bash
    dev tun
    ifconfig 192.168.0.2 192.168.0.1 #Exemplo de IPs para utilizar
    remote <Elastic-IP-do-Servidor-FronEnd>
    secret "<Caminho para a chave na sua maquina local>"
    port 1194
    proto udp
    comp-lzo
    verb 4
    keepalive 10 120
    persist-key
    persist-tun
    float
    cipher AES-256-CBC
    ```   
    - Substitua "***Elastic-IP-do-Servidor-Frontend***" pelo IP elástico associado à sua EC2.

---

### 4. Transferir Arquivos para o Cliente
>Utilize o cliente Bitvise (ou outro cliente SFTP) para transferir os seguintes arquivos da instância EC2 para sua máquina local:

1. ```server.key``` (renomeie para *chave* no lado do cliente).
2. ```client.conf``` (este será transformado em *.ovpn* no próximo passo).

**Procedimento no Bitvise:**

1. Abra o Bitvise e conecte-se à EC2.
2. Na aba **SFTP**, localize e baixe os arquivos necessários para uma pasta local (exemplo: C:\VPN)
    - Localização do ```server.key```: ```/etc/openvpn/server.key```.
    
---

### 5. Configurar o Cliente no OpenVPN GUI

1. Renomeie **client.conf** para **cliente.ovpn** e mova-o para a pasta ```C:\Program Files\OpenVPN\config.```
2. Coloque o arquivo *chave* na mesma pasta.
3. Abra o **OpenVPN GUI** e clique com o botão direito no ícone da bandeja.
4. Selecione Conectar e insira as credenciais, se necessário.

---

### 6. Testar a Conexão VPN

1. Abra um terminal na máquina local.
2. Teste o ping para o IP do servidor:
    ```bash
    ping 192.168.0.1
    ```
    - Se você receber respostas, a conexão está funcionando corretamente.


---

### 7. Configurar o Nginx para Utilizar a VPN
>Edite o arquivo de configuração do Nginx (```/etc/nginx/sites-available/default```) na EC2 FrontEnd para redirecionar requisições através da VPN:
```bash
    upstream backend {
            server <ip da EC2 "BackEnd"> : <Porta EC2>;     #Server 1
            server <ip da EC2 "BackEnd"> : <Porta EC2>;     #Server 2    Load Balancer
            server <ip da EC2 "BackEnd"> : <Porta EC2>;     #Server 3
    }

    server {
            listen 80;
            server_name <IP DA VPN AQUI> ;

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
Reinicie o Nginx em sua EC2 para aplicar as mudanças:
```bash
sudo systemctl restart nginx
```

## 🎯 Próximos Passos

1. Finalize o teste da aplicação acessando o Elastic IP via navegador e validando as rotas configuradas.
2. Consulte a documentação <a href="/docs/Nginx-Configuration.md">**Nginx-Configuration.md**</a> para otimizar o proxy reverso.
3. Utilize o guia <a href="/docs/EC2-Setup.md">**Configuração do EC2**</a> para criar novas instâncias, se necessário.
4. Certifique-se de configurar as politicas de CORS de acordo com a tecnologia que você escolheu, veja um exemplo com FASTAPI em: <a href="/docs/CORS.md">**CORS.md**</a>

## 🌟 Opcional: Configurar Certificados

>Para aumentar a segurança, você pode configurar certificados TLS no OpenVPN. Consulte a documentação oficial do <a href="https://openvpn.net/community-resources/reference-manual-for-openvpn-2-4/" target="_blank">**OpenVPN**</a> para detalhes.

