# Estrutura do Layers

O Layers Education é organizado em comunidades. Cada instituição de ensino tem sua comunidade que cotém seu dados e seus apps. Comunidades podem também ser organizadas na forma de redes com uma comunidade matriz que tem como filhos as comunidades de suas unidades.

### Dados

Os dados de uma comunidade podem ser separados nas seguintes categorias:

+ Usuários

Usuários no Layers Education tem a seguinte estrutura:

```js
{
    "community": String
    "name": String,
    "alias": String,
    "email": String,
    "roles": [String],
    "status": String,
    "active": Boolean,
    "invitationCount": Number,
    "membersCount": Number
    "membersId": [String]
}
```

+ Membros

Membros no Layers Education são a representação de alunos e tem a seguinte estrutura:

```js
{
    "community": String
    "name": String,
    "alias": String,
    "active": Boolean,
    "birth": String,
    "access": {
        "user": String
    }
}
```

+ Grupos

Grupos são a representação de turmas no Layers Education e tem a seguinte estrutura:

```js
{
    "community": String
    "name": String,
    "alias": String,
    "tags": [
        {
            "_id": String,
            "alias": String
        }
    ],
    "admins": [
        {
            "user": String
        }
    ]
    "active": Boolean
}
```

+ Matrículas

Matriculas representam o vínculo entre grupos e membros:

```js
{
    "kind": String,
    "community": String,
    "entity": String,
    "group": String,
    "active": Boolean,
}
```

### Apps

Apps sào a maneira pela qual é possível adicionar funcionalidades ao Layers Education. O próprio Layers disponibiliza alguns apps como as visualizações de informações de ERP's integrados além das API's de comunicação, solicitações e pagamentos, mas outras empresas podem também desenvolver e disponibilizar seus apps para instituições de ensino utilizando as features disponibilizadas.

+ API
+ Services
+ Autenticação e Autorização
+ Portais
+ Notificações
+ Campos personalizados
+ Permissões personalizadas
+ Pagamentos