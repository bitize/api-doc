---
tags: [autenticação, auth, login, refresh token, token]
---

# Como obter o token de acesso

O Token de Acesso é a maneira como sua aplicação se autentica na API da plataforma Bitize e obtem todos os acessos necessários para a integração.<br>
Para iniciar esse processo é necessário criar sua chave de consumo (Consumer Key) para isso acesse a [plataforma](https://app.bitize.com.br/) como um administrador e, no menu, vá até Configurações > Integrações:
 <br>

![Cadastrar Integração](../../assets/images/cadastrar-integracao.jpg)

<!-- theme: danger -->
>### Atenção
>O Secret Key só é apresentado nesse momento, se você perder, será necessário criar outro cadastro de integração.

Agora que temos o consumer key e o secret key podemos fazer a requisição para obtermos o token.


<!-- theme: info -->
> ### API
> Método: POST
> 
> URL: https://api.bitize.com.io/auth
>

**Requisição:**

 ```json json_schema
{
    "type": "object",
    "properties": {
        "consumer_key": {
            "type": "string",
            "description":"Chave de Consumo"
        },
        "secret_key": {
            "type": "string",
            "description": "Chave Secreta"
        },
    },
    "required": [
        "consumer_key",
        "secret_key"
    ]
}
```


Você pode testar suas credenciais abaixo. Adicione as chaves e envie o post:

```json http
{
  "method": "post",
  "url": "https://api.bitize.io/auth",
  "body":{
    "consumer_key":"SUA CHAVE DE CONSUMO",
    "secret_key":"SUA CHAVE SECRETA"
  },
  "headers":{
    "Content-Type":"application/json"
  }
}
```


**Resposta:**

 ```json json_schema
{
    "type": "object",
    "properties": {
        "access_token": {
            "type": "string",
            "description":"Token"
        },
        "refresh_token": {
            "type": "string",
            "description": "Refresh Token"
        },
        "expiration_access_token": {
            "type": "string",
            "description":"Data e hora da expiração do Token"
        },
        "expiration_refresh_token": {
            "type": "string",
            "description": "Data e hora do Refresh Token"
        },
    }
}
```

Agora que você possue o token de acesso você deve incluí-lo no header de todas as suas requisições

<!-- theme: info -->
>Authorization: Bearer **SEU TOKEN**

Mas você também deve se atentar a validade do token e utilizar o Refresh Token para solicitar um novo Token sem que seja necessário reenviar sua Chave de Consumo e a Chave Secreta.

```json http
{
  "method": "get",
  "url": "https://api.bitize.io/auth",
  "query":{"refresh_token":"SEU REFRESH TOKEN"},
  "headers":{
    "Content-Type":"application/json"
  }
}