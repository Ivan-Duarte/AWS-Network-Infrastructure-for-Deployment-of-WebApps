## Infraestrutura de rede AWS para implantação de WebApps

Este repositório fornece um guia completo para configurar uma infraestrutura de rede segura e escalável para implantar aplicativos da web na AWS. Seja você um desenvolvedor, um engenheiro de DevOps ou alguém novo na AWS, este repositório serve como uma referência reutilizável e bem documentada para construir infraestrutura de nuvem.

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
        Clone o repositório:
        <br>

        git clone https://github.com/Ivan-Duarte/AWS-Network-Infrastructure-for-Deployment-of-WebApps.git
        
        cd AWS-Network-Infrastructure-for-Deployment-of-WebApps
</li>
    <li>
    Configure seu ambiente:
    <br>
        <ul>
            <li>
                Prepare uma conta AWS com as permissões de IAM necessárias.
            </li>
            <li>
                Instale as ferramentas necessárias como AWS CLI, OpenVPNe Nginx.
            </li>
        </ul>
    </li>
    <li>
    </li>
    <li>
    </li>
    <li>
    </li>
</ol>    