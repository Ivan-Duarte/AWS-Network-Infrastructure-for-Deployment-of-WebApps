## Guia de Configuração do EC2
> **Este guia fornece um passo a passo detalhado para configurar instâncias EC2 na AWS e preparar o ambiente para a implantação de aplicações web.**

## ❗ Pré-requisitos
1. Conta <a href="https://aws.amazon.com/pt/free/?gclid=CjwKCAiA3ZC6BhBaEiwAeqfvyrw2rtc1HfyzoWWtt6mO1fQiXwAkCV2UEng6-62EzV1e2EXr3u5uvxoCS-sQAvD_BwE&trk=2ee11bb2-bc40-4546-9852-2c4ad8e8f646&sc_channel=ps&ef_id=CjwKCAiA3ZC6BhBaEiwAeqfvyrw2rtc1HfyzoWWtt6mO1fQiXwAkCV2UEng6-62EzV1e2EXr3u5uvxoCS-sQAvD_BwE:G:s&s_kwcid=AL!4422!3!696214219374!e!!g!!aws!15278604629!130587771740&all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc&awsf.Free%20Tier%20Types=*all&awsf.Free%20Tier%20Categories=*all" target="_blank">**AWS**</a> com permissões específicas (IAM).
2. AWS CLI configurada localmente (opcional para automação).
3. Cliente SSH instalado em sua máquina (como <a href="https://bitvise.com/download-area" target="_blank">**Bitvise SSH**</a> ou <a href="https://www.putty.org/" target="_blank">**PuTTY**</a>).
4. Chave de acesso ***.pem*** para conexão com a instância.

## ⏩ Etapas

### 1. Criar uma Instância EC2

**Passos no Console AWS:**

1. Acesse o console da AWS e navegue até a seção EC2.
![EC2-Setup-001](/docs/img/EC2-Setup/EC2-Setup-001.png)
2. Clique em ***Launch Instance*** (Iniciar Instância).
![EC2-Setup-002](/docs/img/EC2-Setup/EC2-Setup-002.png)
3. Escolha a imagem do sistema operacional ( Ubuntu Server 22.04 LTS é recomendado).
![EC2-Setup-01](/docs/img/EC2-Setup/EC2-Setup-01.png)
4. Configurar:
    - **Tipo de Instância** : ***t2.micro*** (elegível para o Free Tier).
    ![EC2-Setup-02](/docs/img/EC2-Setup/EC2-Setup-02.PNG)
    - Crie o par de chave **".pem"** para poder acessar sua EC2 via SSH
    ![EC2-Setup-03](/docs/img/EC2-Setup/EC2-Setup-03.PNG)
    - De um nome para sua chave e selecione o formato **".pem"**, e então clique em criar par de chaves.
    ![EC2-Setup-04](/docs/img/EC2-Setup/EC2-Setup-04.png)
    - **Grupos de Segurança** : Adicione regras para permitir:
        - SSH (porta 22) - Para conexões remotas
        - HTTP (porta 80) - Para tráfego da Internet
        - HTTPS (443) - Para tráfego da Internet
        - Basta selecionar os itens destacados na imagem
        ![EC2-Setup-05](/docs/img/EC2-Setup/EC2-Setup-05.png)
    - Configure o armazenamento desejado (recomendado 10Gb - Gib gp3)
    ![EC2-Setup-06](/docs/img/EC2-Setup/EC2-Setup-06.png)
    - Clique em "Launch Instance"
    ![EC2-Setup-07](/docs/img/EC2-Setup/EC2-Setup-07.png)
    - **Elastic IP** : Associa um Elastic IP à instância para um endereço fixo.
        - Clique em "Elastic IPs"
        ![EC2-Setup-08](/docs/img/EC2-Setup/EC2-Setup-08.PNG)
        - Vá até "Allocate Elastic IP address"
        ![EC2-Setup-09](/docs/img/EC2-Setup/EC2-Setup-09.PNG)
        - Clique em "Allocate"
        ![EC2-Setup-10](/docs/img/EC2-Setup/EC2-Setup-10.PNG)
        - Selecione o IP elástico que você acabou de criar e clique em "Actions"
        ![EC2-Setup-11](/docs/img/EC2-Setup/EC2-Setup-11.png)
        - Clique em Associate Elastic IP address
        ![EC2-Setup-12](/docs/img/EC2-Setup/EC2-Setup-12.png)
        - Selecione a instancia que deseja manter o IP elástico e clique em Associate
        ![EC2-Setup-13](/docs/img/EC2-Setup/EC2-Setup-13.png)
            > Esse processo garante que a instancia não mudará de IP toda vez que for reiniciada, garantindo que as configurações baseadas em IPs funcionem de forma constante, sem necessitar refatoração dos arquivos de configuração.

---

### 2. Conecte-se à Instância
Depois que a instância criada e o Elastic IP foram configurados, você pode acessar sua maquina EC2 das seguintes maneiras:

#### **Para usuários de Linux/Mac:**

1. Sem terminal, execute:

```bash
ssh -i caminho-para-sua-chave.pem ubuntu@<Elastic-IP>
```

2. Substitua caminho-para-sua-chave.pempelo caminho do arquivo "**.pem**" e <Elastic-IP>pelo IP elástico associado.



#### **Para usuários do Windows:**

1. Utilize o cliente Bitvise:
    - Configure uma conexão no Bitvise com o IP da instância, a porta 22 e o usuário "ubuntu"<br>
    ![EC2-Setup-14](/docs/img/EC2-Setup/EC2-Setup-14.png)
2. Será necessário definir o método de autenticação, no nosso caso precisaremos configurar uma "***Global Key***", para isso vamos em "**Client key Manager**"<br>
![EC2-Setup-15](/docs/img/EC2-Setup/EC2-Setup-15.png)
3. Clique em "**import**" e selecione o seu arquivo "**.pem**" que foi salvo no momento em que você criou o Key Pair ao criar a sua EC2<br>
![EC2-Setup-16](/docs/img/EC2-Setup/EC2-Setup-16.png)
4. Agora você precisa selecionar o "**Client Key**" para utilizar a chave Global que você importou<br>
![EC2-Setup-17](/docs/img/EC2-Setup/EC2-Setup-17.png)
5. Clique em "**Log in**" e verifique se apareceu uma nova aba na sidebar chamada "**New terminal console**", caso tenha aparecido, basta clicar nela para abrir o terminal da sua máquina EC2.<br>
![EC2-Setup-18](/docs/img/EC2-Setup/EC2-Setup-18.png)

---

### 3. Configurar Grupos de Seguranças
>Os Grupos de Segurança são essenciais para controlar o tráfego que entra e sai de sua instância EC2. Eles funcionam como firewalls virtuais.

**Passos para Configurar Grupos de Segurança:**

1. Acesse a Seção de Grupos de Segurança:
    - No painel da AWS, acesse a seção **Security Groups** (Grupos de Segurança) no menu lateral da página EC2.<br>
    ![EC2-Setup-](/docs/img/EC2-Setup/EC2-Setup-20.png)
2. Crie ou Edite um Grupo de Segurança:
    - Clique em Create Security Group para criar um novo grupo ou selecione um grupo existente para editar.
    ![EC2-Setup-](/docs/img/EC2-Setup/EC2-Setup-21.png)
3. Configurações Básicas do Grupo de Segurança:
    - Nome do grupo: Frontend (ou outro nome que descreva sua função).
    - Descrição: Grupo de segurança para a instância Frontend.
    ![EC2-Setup-](/docs/img/EC2-Setup/EC2-Setup-22.png)
4. Adicione Regras de Entrada:
    - Clique em Add Rule e configure as portas abaixo:
        - SSH (porta 22): Permite acesso remoto via terminal.
            - Tipo: SSH
            - Protocolo: TCP
            - Porta: 22
            - Origem: Seu IP (recomendado) ou Anywhere para acesso aberto.
        - HTTP (porta 80): Permite tráfego da web não seguro.
            - Tipo: HTTP
            - Protocolo: TCP
            - Porta: 80
            - Origem: Anywhere.
        - HTTPS (porta 443): Permite tráfego da web seguro.
            - Tipo: HTTPS
            - Protocolo: TCP
            - Porta: 443
            - Origem: Anywhere.
        - VPN (porta 1194): Permite conexões do OpenVPN.
            - Tipo: Custom UDP Rule
            - Protocolo: UDP
            - Porta: 1194
            - Origem: Anywhere (ou Seu IP, caso queira limitar acessos).
<br>

        ![EC2-Setup-](/docs/img/EC2-Setup/EC2-Setup-23.png)

> Por padrão, todas as regras de saída são permitidas. Não é necessário configurar manualmente, a menos que queira restringir tráfego específico.
5. Associar o Grupo de Segurança à Instância EC2:
    - Acesse a página da instância EC2 criada.
    - Clique em Actions > Security > Change Security Groups.<br>
    ![EC2-Setup-](/docs/img/EC2-Setup/EC2-Setup-24.png)
    - Adicione o grupo de segurança configurado (e remova outros, se necessário).
    ![EC2-Setup-](/docs/img/EC2-Setup/EC2-Setup-25.png)

>Salve as alterações e Confirme que o grupo de segurança está aplicado corretamente e teste as conexões para garantir que as portas configuradas estejam funcionando como esperado.

---

### 4. Instalar Pacotes Básicos
Depois de conectar-se à instância, atualize o diretório de pacotes com o comando abaixo:

```bash
sudo apt update && sudo apt upgrade -y
```

![EC2-Setup-](/docs/img/EC2-Setup/EC2-Setup-19.png)

> Agora sua instância EC2 está pronta para as próximas configurações.

## 🎯 Próximos Passos

**Após configurar sua instância EC2 e instalar os pacotes básicos:**

1. Determine funções das instâncias EC2:
    - **Frontend**: Hospedagem de arquivos estáticos, proxy reverso e VPN.
    - **Backend**: Hospedagem do servidor backend e banco de dados.
2. Consulte o <a href="./img/Network-Topology.png">**Diagrama de Topoligia de Rede**</a>.
3. Configure: 
    - VPN: Siga o guia <a href="/docs/VPN-Setup.md">**VPN-Setup.md**</a>
    - Nginx como proxy reverso: Veja <a href="/docs/Nginx-Configuration.md">**Nginx-Configuration.md**</a>
