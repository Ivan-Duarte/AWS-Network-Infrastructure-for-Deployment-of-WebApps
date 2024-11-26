## Infraestrutura de rede AWS para implantação de WebApps

> Este repositório fornece um guia completo para configurar uma infraestrutura de rede segura e escalável para implantar aplicativos da web na AWS. Seja você um desenvolvedor, um engenheiro de DevOps ou alguém novo na AWS, este repositório serve como uma referência reutilizável e bem documentada para construir infraestrutura de nuvem.

## 📌 Visão geral

O projeto demonstra:

* Configurando e configurando instâncias do AWS EC2.
* Estabelecendo uma VPN segura para comunicação de rede privada.
* Configurando o Nginx como um proxy reverso e balanceador de carga.
* Implantação de aplicativos web (frontend e backend) em uma infraestrutura segura.
* Automatizando tarefas comuns de configuração com scripts Bash.
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
│   ├── Troubleshooting.md         # Problemas comuns e como resolvê-los
│   └── Network-Diagram.md         # Representação visual da infraestrutura de Rede
│
├── examples/
│   ├── nginx/
│   │   ├── default.conf           # Exemplo do arquivo de configuração NginX
│   │   └── load-balancer.conf     # Exemplo do arquivo de configuração do Load Balancer
│   ├── vpn/
│   │   ├── server.conf            # Exemplo do arquivo de configuração do Server OpenVPN
│   │   └── client.ovpn            # Exemplo do arquivo de configuração do Cliente OpenVPN
│   ├── .env.example               # Amostra de exemplo para variáveis de ambiente adotadas
│   └── README.md                  # Como utilizar os exemplos
│
├── scripts/
│   ├── setup-ec2.sh               # Script para automatizar a configuração de instancias EC2
│   ├── setup-vpn.sh               # Script para automatizar configuração da VPN
│   ├── deploy-nginx.sh            # Script para automatizar a configuração de implementação do NginX
│   └── update-cors.sh             # Script para automatizar politicas de CORS na aplicação
│
├── diagrams/
│   └── network-topology.drawio     # Diagrama editável de topologia de rede
│
├── LICENSE                        # Lisença do projeto
├── CONTRIBUTING.md                # Guia para contribuições
└── README.md                      # Você está aqui!
```

## 🔰 Começando

Siga estas etapas para configurar a infraestrutura:

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
            Prepare uma conta <a href="https://aws.amazon.com/pt/free/?gclid=CjwKCAiA3ZC6BhBaEiwAeqfvyrw2rtc1HfyzoWWtt6mO1fQiXwAkCV2UEng6-62EzV1e2EXr3u5uvxoCS-sQAvD_BwE&trk=2ee11bb2-bc40-4546-9852-2c4ad8e8f646&sc_channel=ps&ef_id=CjwKCAiA3ZC6BhBaEiwAeqfvyrw2rtc1HfyzoWWtt6mO1fQiXwAkCV2UEng6-62EzV1e2EXr3u5uvxoCS-sQAvD_BwE:G:s&s_kwcid=AL!4422!3!696214219374!e!!g!!aws!15278604629!130587771740&all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc&awsf.Free%20Tier%20Types=*all&awsf.Free%20Tier%20Categories=*all" target="_blank">AWS</a> com as permissões de IAM necessárias.
        </li>
        <li>
            Instale as ferramentas necessárias como  <a href="https://openvpn.net/community-downloads/" target="_blank">OpenVPNe</a>,  <a href="https://nginx.org/en/download.html" target="_blank">Nginx</a>.
        </li>
    </ul>
</li>
<li>
Siga os guias na pasta <a href="docs">docs</a> para obter instruções detalhadas.
</li>
<li>
Use os scripts na pasta <a href="scripts">scripts</a> para automatizar o processo de configuração.
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
Um diagrama detalhado da topologia de rede está incluído na pasta <a href="diagrams">diagrams</a> . Abaixo está uma visão geral da arquitetura:

- **Camada Pública** : Nginx como proxy reverso e balanceador de carga.
- **Camada de Aplicação** : instâncias EC2 executando serviços de backend.
- **Camada privada** : comunicação protegida por VPN entre o cliente e o backend.

## 🧾 Exemplos
Use os arquivos da pasta <a href="examples">examples</a> como modelos para suas configurações.

- Configurações Nginx para proxy reverso e balanceamento de carga.
- Configurações de servidor e cliente OpenVPN.
- Amostras de variáveis ​​de ambiente.

## 💡 Licença
Este projeto está licenciado sob a <a href="https://opensource.org/license/mit" target="_blank">Licença MIT</a>.