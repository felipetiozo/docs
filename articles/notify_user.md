
# Enviar notificação pelo Layers

### Introdução

É possível enviar notificações personalizadas através da API do Layers para dispositivos android, IOS e também na plataforma web. Para enviar uma notificação, siga os passos abaixo.

> **Atenção**: para realizar esse procedimento é necessário ter permissão ```notification:send```

### 1 - Cadastre seu app e crie token para o seu app de sincronização

Todas as rotas da API do Layers Education exigem autenticação para que seja garantida sua segurança. Desse modo, para realizar uma requisição para a rota de sync será necessário que sua aplicação passe nos headers a chave "Authorization" com seu token. Caso você ainda não tenha um app cadastrado, entre em contato conosco para realizar esse cadastro. 

Uma vez que você tem seu app cadastrado no Layers, para gerar seu token de autenticação você deve fazer uma requisição como mostrado abaixo:

##### **GET** `/appmaker/apps/:appId/token`
###### resposta
```js
{
    "token": 'Bearer ' + seuToken
}

Esse token deve ser passado na chave ```Authorization``` nos headers para realizar a requisição para a rota de notificação.

### 2 - Faça a requisição para a rota de notificação

Existem duas formas de escolher o público que deve receber uma notificação: passar a chave ```userIds``` com os ids dos usuários que devem ser notificados em um array ou passar a chave ```targets``` contendo um lista de objetos e a chave ```roles``` contendo quais permissões um usuário que se encaixe nos targets deve ter para receber aquela notificação.

##### POST `/communication/notify`
###### Payload usando o array de userIds
```js
{
    "title": "título da notifícação",
    "body": "texto do corpo da notificação",
    "userIds": ["id", "id", "id"], //IDs dos usuários a serem notificados
    "data": {
        "action": "goTo" //ação quando a notificação for clicada
        "payload": {
            "location": "groups"
        }
    }
}
```

##### POST `/communication/notify`
###### Payload usando o array de userIds
```js
{
    "title": "título da notificação",
    "body": "texto do corpo da notificação"
    "targets": [{
        "_id": "id" //id do membro, grupo, usuário ou tag
        "kind": "member" //tipo do documento cujo id está na chave _id
    }]
    "roles": ["guardian"], //permissões que serão notificadas
    "data": {
        "action": "goTo" //ação quando a notificação for clicada
        "payload": {
            "location": "groups"
        }
    }
}
```


###### Resposta

```js
{
    "success": 10, //Número de dispositivos notificados
    "failure": 0 //Número de dispositivos para os quais a notificação falhou
}
```