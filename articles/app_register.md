# Registrando seu app no Layers Education

Todas as rotas da API do Layers Education exigem autenticação para que seja garantida sua segurança. Desse modo, para realizar uma requisições para a API, será necessário que sua aplicação passe nos headers a chave "Authorization" com seu token. Siga os passos abaixo para cadastrar seu app e gerar o seu token de autenticação. 

### 1 - Cadastre seu app

Antes de gerar seu token queremos conhecer seu app! Entre em contato conosco através do e-mail suporte@layers.education falando um pouco sobre seu app no Layers. A partir desse e-mail entraremos em contato com você para entendermos melhor como podemos ajudar para que o seu app tenha acesso a todas as funcionalidades que ele precisa. Além disso, será a partir dessa comunicação que cadastraremos seu app para que ele possa gerar um token.

### 2 - Gere um token de autenticação para o seu app

Uma vez que você tem seu app cadastrado no Layers, para gerar seu token de autenticação você deve fazer uma requisição como mostrado abaixo:

##### **GET** `/appmaker/apps/:appId/token`
###### resposta
```js
{
    "token": 'Bearer ' + seuToken
}
```

Passando esse token na chave ```Authorization``` nos headers seu app tem agora acesso às rotas da API do Layers para as quais ele tem permissão registrada! 
