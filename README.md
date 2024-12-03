# Infraestrutura de rede AWS para implantação de WebApps

## 🌍 Índice Principal

1. **Introdução**
    - [**Visão Geral**](#-visão-geral) do projeto e propósito.
2. **Guia de Configuração**
    - <a href="/docs/EC2-Setup.md">**EC2 Setup**</a>: Como criar e configurar instâncias EC2 na AWS.
    - <a href="/docs/VPN-Setup.md">**VPN Setup**</a>: Configuração de VPN utilizando OpenVPN, incluindo cliente e servidor.
    - <a href="/docs/Nginx-Configuration.md">**Nginx Configuration**</a>: Configuração do Nginx como proxy reverso e load balancer.
    - <a href="/docs/CORS.md">**CORS**</a>: Exemplo de configuração para Cross Origins no BackEnd da aplicação.
3. **Diagramas e Referências**
    - Topologias de Rede - <a href="/docs/img/Network-Topology.png">**Clique Aqui**</a>
    - Template NginX - <a href="/examples/nginx/">**Clique Aqui**</a>
    - Template VPN - <a href="/examples/vpn/">**Clique Aqui**</a>

## 📌 Visão geral

> Este repositório fornece um guia completo para configurar uma infraestrutura de rede segura e escalável para implantar aplicativos da web na AWS. Seja você um desenvolvedor, um engenheiro de DevOps ou alguém novo na AWS, este repositório serve como uma referência reutilizável e bem documentada para construir infraestrutura de nuvem.

O projeto demonstra:

* Configurando e configurando instâncias do AWS EC2.
* Estabelecendo uma VPN segura para comunicação de rede privada.
* Configurando o Nginx como um proxy reverso e balanceador de carga.
* Implantação de aplicativos web (frontend e backend) em uma infraestrutura segura.
* Manipulando políticas CORS e comunicação segura entre origens.

## 📦 Estrutura do Repositório

```bash
AWS-Network-Infrastructure-for-Deployment-of-WebApps/
│
├── docs/
│   ├── EC2-Setup.md               # Guia para configuração de instancias EC2
│   ├── VPN-Setup.md               # Instruções para configuração do OpenVPN
│   ├── Nginx-Configuration.md     # Configurações detalhadas do NginX + configurações do Proxy Reverso
│   ├── CORS-Setup.md              # Explicação e implementação das politicas de CORS
│   └── Network-Diagram.md         # Representação visual da infraestrutura de Rede
│
├── examples/
│   ├── nginx/
│   │   ├── default.conf           # Exemplo do arquivo de configuração NginX
│   │   └── load-balancer.conf     # Exemplo do arquivo de configuração do Load Balancer
│   ├── vpn/
│   │   ├── server.conf            # Exemplo do arquivo de configuração do Server OpenVPN
│   │   └── client.ovpn            # Exemplo do arquivo de configuração do Cliente OpenVPN
│
├── LICENSE                        # Lisença do projeto
└── README.md                      # Você está aqui!
```

## 🔰 Começando

>Siga estas etapas para configurar a infraestrutura:

<ol>
<li>
Clone o repositório:<br>

    git clone https://github.com/yourusername/AWS-Network-Infrastructure-for-Deployment-of-WebApps.git

    cd AWS-Network-Infrastructure-for-Deployment-of-WebApps
</li>
<li>
Configure seu ambiente:
<br>
    <ul>
        <li>
            Prepare uma conta <a href="https://aws.amazon.com/pt/free/?gclid=CjwKCAiA3ZC6BhBaEiwAeqfvyrw2rtc1HfyzoWWtt6mO1fQiXwAkCV2UEng6-62EzV1e2EXr3u5uvxoCS-sQAvD_BwE&trk=2ee11bb2-bc40-4546-9852-2c4ad8e8f646&sc_channel=ps&ef_id=CjwKCAiA3ZC6BhBaEiwAeqfvyrw2rtc1HfyzoWWtt6mO1fQiXwAkCV2UEng6-62EzV1e2EXr3u5uvxoCS-sQAvD_BwE:G:s&s_kwcid=AL!4422!3!696214219374!e!!g!!aws!15278604629!130587771740&all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc&awsf.Free%20Tier%20Types=*all&awsf.Free%20Tier%20Categories=*all" target="_blank"><b>AWS</b></a> com as permissões de IAM necessárias.
        </li>
        <li>
            Instale as ferramentas necessárias como  <a href="https://openvpn.net/community-downloads/" target="_blank"><b>OpenVPN</b></a>,  <a href="https://nginx.org/en/download.html" target="_blank"><b>Nginx</b></a>.
        </li>
    </ul>
</li>
<li>
Siga os guias na pasta <a href="/docs/"><b>Docs</b></a> para obter instruções detalhadas.
</li>
<li>
Teste sua implantação e garanta que todos os componentes funcionem conforme o esperado.
</li>
</ol>

---

## 🔠 Características
- **Escalabilidade** : dimensione facilmente aplicativos da web com balanceamento de carga.
- **Segurança** : aproveite as políticas de VPN e CORS para um ambiente seguro.
- **Reutilização** : A estrutura modular permite fácil adaptação em outros projetos.
- **Documentação** : Guias claros e passo a passo para cada componente.
- **Automação** : scripts pré-criados para tarefas repetitivas, economizando tempo e esforço.

## 🌐 Topologia de rede
Um diagrama detalhado da topologia de rede está incluído em: <a href="/docs/img/Network-Topology.png">**Network-Topology**</a> . Abaixo está uma visão geral da arquitetura:

- **Camada Pública** : Nginx como proxy reverso e balanceador de carga.
- **Camada de Aplicação** : instâncias EC2 executando serviços de backend.
- **Camada privada** : comunicação protegida por VPN entre o cliente e o backend.

## 🧾 Exemplos
Use os arquivos da pasta <a href="/examples/">**Examples**</a> como modelos para suas configurações.

- Configurações Nginx para proxy reverso e balanceamento de carga.
- Configurações de servidor e cliente OpenVPN.

## 💡 Licença
Este projeto está licenciado sob a <a href="https://opensource.org/license/mit" target="_blank"><b>Licença MIT</b></a>.