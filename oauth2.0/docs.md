# Layers OAuth 2.0

### Escopos
| Escopo                    | Acesso                                                                                                                                                                               |
|---------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| identity:basic:read       | account.id <br/>account.createdAt <br/>account.updatedAt <br/>account.language <br/>account.timezone <br/>account.firstName <br/>account.lastName <br/>account.name                                                     |
| identity:&#8288;email&#8288;:read       | account.email                                                                                                                                                                        |
| identity:birth:read       | account.avatar                                                                                                                                                                       |
| identity:cpf:read         | account.birth                                                                                                                                                                        |
| identity:communities:read | community.community <br/>community.name <br/>community.icon <br/>community.logo <br/>community.language <br/>community.timezone <br/>community.geolocation <br/>community.color <br/>community.createdAt <br/>community.updatedAt |
| user:enrollments:read     | enrollment.id <br/>enrollment.kind <br/>enrollment.entity <br/>enrollment.group <br/>enrollment.createdAt <br/>enrollment.updatedAt                                                                                               |
| group:read                | <br/>group.id <br/>group.name <br/>group.alias <br/>group.createdAt <br/>group.updatedAt                                                                                                                                          |
| user:members:read         | member.id <br/>member.alias <br/>member.name <br/>member.createdAt <br/>member.updatedAt                                                                                                                                          |

### Client-side

O cliente deverá abrir a seguinte url `https://id.layers.digital`, passando os seguintes pârametros:

| Parâmetro     | Valor                                                               |
| ------------- | ------------------------------------------------------------------- |
| client_id     | Identificador do app                                                |
| response_type | `code` (Atualmente o único aceitado)                                |
| redirect_uri  | URL de redirecionamento configurada anteriormente                   |
| scope         | Escopos configurados anteriormente (deve ser indentado com espaços) |
| state         | Mensagem adicional que pode ser utilizada para ser retornado na rota de token de acesso (OPCIONAL) |

Exemplo de url: `https://id.layers.digital/?client_id=layers&redirect_uri=https://layers.com&response_type=code&scope=identity:basic:read identity:email:read identity:communities:read user:members:read user:enrollments:read group:read`

[Melhores práticas de OAuth com Layers Education](https://github.com/layers-digital/docs/blob/master/oauth2.0/best_practices.md)

### Autenticação
Todas as chamadas devems ser feitas na seguinte url: `https://api.layers.digital`
Após o usuário fazer o fluxo de login e aceitar os escopos, será redirecionado para `https://{{redirect_uri}}?code={{code}}`. Com este código de acesso `{{code}}`, será necessário fazer a seguinte requisição:

##### **POST** `/oauth/token`
###### Requisição do tipo FORM URL Encoded:
```js
{
    "grant_type": "authorization_code",
    "client_id": "{{client_id}}",
    "code": "{{code}}",
    "redirect_uri": "{{redirect_uri}}"
}
```
A API deve retornar um JSON com o seguinte formato:
###### Resposta:
```js
{
{
    "access_token": "{{jwtToken}}",
    "token_type": "Bearer",
    "expires_in": Number // Em quanto tempo irá expirar este token,
    "state": String // Irá retornar o mesmo valor caso tenha sido utilizado na primeira chamada
}
}
```
Todos os endpoints abaixos devem ser autenticados da seguinte forma:
##### Headers
```js
Authorization: 'Bearer {{jwtToken}}'
```

### Endpoints

##### **GET** `/v1/oauth/account`
Retorna os detalhes de uma conta
###### A API deve retornar um JSON com o seguinte formato:
###### Resposta:
```js
{
    "createdAt": Date, // Data de criação da conta
    "email": String, // Email de acesso da conta
    "firstName": String, // Primeiro nome
    "id": String, // Identificador único
    "language": String, // Idioma principal da conta
    "lastName": String, // Último nome
    "name": String, // Nome completo
    "timezone": String // Timezone principal da conta",
    "updatedAt": String // Última data de atualização da conta
}
```

##### **GET** `/v1/oauth/communities`
Retorna os detalhes de uma comunidade
###### A API deve retornar um JSON com o seguinte formato:
###### Resposta:
```js
[
    {
        "color": String, // Cor principal da comunidade
        "community": String, // Identificador único
        "icon": String, // Logo
        "name": String // Nome
    },
]
```

##### **GET** `/v1/oauth/members?_community={{community}}`
Retorna os membros de uma comunidade vinculados a conta
###### A API deve retornar um JSON com o seguinte formato:
###### Resposta:
```js
[
    {
        "alias": String, // Identificador do membro na comunidade
        "createdAt": Date, // Data de criação do membro
        "id": String, // Identificador único
        "name": String, // Nome
        "updatedAt": Date // Última data de atualização do membro
    }
]
```

##### **GET** `/v1/oauth/members/{{memberId}}/enrollments?_community={{community}}`
Retorna os vínculos de membro/turma de um determinado membro
###### A API deve retornar um JSON com o seguinte formato:
###### Resposta:
```js
[
    {
        "createdAt": String, // Data de criação do vínculo
        "entity": String, // Identificador único do membro
        "group": String, // Identificador único do grupo
        "id": String, // Identificador único do vínculo
        "kind": String, // Tipo do vínculo [member,user]
        "updatedAt": Date // Última data de atualização do vínculo
    },
]
```

##### **GET** `/v1/oauth/groups/{{groupId}}?_community={{community}}`
Retorna os detalhes de um determinado grupo
###### A API deve retornar um JSON com o seguinte formato:
###### Resposta:
```js
{
    "alias": String, // Identificador do grupo dentro da comunidade
    "createdAt": String, // Data de criação do grupo
    "id": String, // Identificador único do grupo
    "name": String, // Nome
    "updatedAt": Date // Última data de atualização do grupo
}
```

**ATENÇÃO:** Os comentários foram adicionados apenas para explicar as estruturas de dados, nem a requisição e nem a resposta devem ter comentários, ambos devem ser JSONs válidos.
