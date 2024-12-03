## Configuração de CORS (Cross-Origin Resource Sharing)

>Este guia descreve como configurar o CORS para sua aplicação backend, garantindo que as requisições entre diferentes origens sejam permitidas quando necessário e configuradas corretamente seguindo a VPN.

## 📚 O que é CORS?
>CORS (Cross-Origin Resource Sharing) é um mecanismo de segurança que controla como os navegadores interagem com recursos de diferentes origens. Por padrão, os navegadores bloqueiam requisições de origens diferentes, e a configuração de CORS permite especificar quais origens têm permissão para acessar os recursos.

## 🔧 Configuração do CORS no FastAPI
A seguir está um passo a passo para configurar o ```CORS``` no backend da aplicação utilizando FastAPI.

### 1. Instalação do Middleware de CORS
- Antes de configurar o CORS, instale o pacote necessário:
    ```bash
    pip install fastapi[all]
    ```

---

### 2. Adicionando o Middleware de CORS no ```main.py```
>Atualize o arquivo main.py para incluir o middleware de CORS. Aqui está um exemplo de configuração que suporta múltiplas origens a partir de uma variável de ambiente:
```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware
from os import environ as env

app = FastAPI()

# Obtendo as origens permitidas da variável de ambiente
allowed_origins = env.get("ALLOWED_ORIGINS", "")
origins = allowed_origins.split(",") if allowed_origins else []

# Configurando o middleware de CORS
app.add_middleware(
    CORSMiddleware,
    allow_origins=origins,
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

---

### 3. Configurando a Variável de Ambiente
Adicione as origens permitidas no arquivo ```.env``` do backend:
```env
ALLOWED_ORIGINS=http://<SEU IP ELASTICO DO FRONTEND>,http://<SEU IP DA VPN>
```

---

### 4. Testando as Configurações
**Cenário de Sucesso**
>Certifique-se de que o backend responda corretamente para uma origem permitida. Faça uma requisição usando o frontend hospedado em um dos domínios configurados, como:
- ```http://155.155.155.155```
- ```http://192.168.0.1```

**Cenário de Erro**
>Se uma origem não configurada tentar acessar o backend, a requisição será bloqueada e o navegador exibirá um erro como:
```csharp
Access to XMLHttpRequest at 'http://backend/api/items' from origin 'http://example.com' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.
```

---

### 5. Debugging de Problemas Comuns
1. **Erro**: *Origens não estão sendo permitidas*
    - Certifique-se de que a variável ```ALLOWED_ORIGINS``` está configurada corretamente no arquivo .env e que a aplicação está lendo o valor após reiniciar.
2. **Erro**: *Middleware não aplicado*
    - Verifique se o middleware de ```CORS``` foi adicionado ao main.py antes da inicialização das rotas.
3. **Erro**: *Backend não responde para todas as origens*
    - Teste o valor de ```allowed_origins``` no terminal:
        ```python
        print(origins)
        ```
    - Verifique se todas as origens configuradas no .env estão aparecendo corretamente.

---

### 6. Opções Adicionais de Configuração
Você pode personalizar as permissões do middleware de ```CORS``` para atender necessidades específicas:

- Restringir métodos permitidos:
    ```python
    allow_methods=["GET", "POST"]
    ```
- Restringir cabeçalhos permitidos:
    ```python
    allow_headers=["Authorization", "Content-Type"]
    ```

---

### 7. Exemplo de Configuração Completa

```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware
from os import environ as env

app = FastAPI()

# Origens permitidas (configuradas no .env)
allowed_origins = env.get("ALLOWED_ORIGINS", "")
origins = allowed_origins.split(",") if allowed_origins else []

# Middleware de CORS
app.add_middleware(
    CORSMiddleware,
    allow_origins=origins,
    allow_credentials=True,
    allow_methods=["GET", "POST", "PUT", "DELETE"],
    allow_headers=["Authorization", "Content-Type"],
)
```

## 🎯 Próximos Passos
Depois de configurar o ```CORS```:
1. Certifique-se de que as origens permitidas atendam ao ambiente de produção e desenvolvimento.
2. Atualize a documentação do backend para incluir a configuração de CORS.
3. Teste o fluxo entre frontend e backend para verificar se os problemas de CORS foram resolvidos.