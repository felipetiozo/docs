
# Enviar notificação pelo Layers

### Introdução

É possível enviar notificações personalizadas através da API do Layers para dispositivos Android, iOS e também na plataforma web.

### Faça a requisição para a rota de notificação

> **Atenção**: para realizar esse procedimento é necessário ter permissão `notification:send`

O corpo da requisição segue o seguinte formato:

##### POST `/notification/send`
```js
{
    // Título principal da notificação
    "title": "Título da notificação",

    // Descrição da notificação
    "body": "Texto do corpo da notificação",

    // Públicos para enviar a notificação. Pode ser uma lista com múltiplos tipos
    "targets": [{
        // Tipo do tópico, pode ser: user, member, group ou tag
        "kind": "member",
        
        // Referência externa do user/member/group/tag
        "alias": "alias"
    }, {
        // Exemplo de Envio para um grupo
        "kind": "group",
        
        // Identificador do user/member/group/tag no Layers
        "id": "id"
    }, {
        // Exemplo de Envio para um usuário
        "kind": "user",
        
        // Email do user
        "email": "email@escola.com.br" // 'email' do user
    }]

    // Permissões que serão notificadas (pode ser omitida caso os todos os targets tenham `"kind": "user"`)
    "roles": ["guardian"], 
}
```

A chave `targets` recebe um Array de objetos com as seguintes chaves:

* `kind`: Indica o tipo do target, pode ser `user`, `member`, `group` ou `tag`
* `alias` | `id` | `email`: Indica o identificador do target. **Obs.: Informe apenas uma destas chaves por target.**

> **Obs.:** A chave `roles` pode ser omitida caso todos os `targets` tenham `"kind": "user"`.
