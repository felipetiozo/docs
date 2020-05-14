
# Enviar notificação pelo Layers

### Introdução

É possível enviar notificações personalizadas através da API do Layers para dispositivos Android, iOS e também na plataforma web.

### Faça a requisição para a rota de notificação

> **Atenção**: para realizar esse procedimento é necessário ter permissão `notification:send`

O corpo da requisição segue o seguinte formato:

##### POST `/notification/send`
```js
{
    "title": "Título da notificação",
    "body": "Texto do corpo da notificação"
    "targets": [{
        "alias": "alias" // 'alias' do user/member/group/tag
        "kind": "member" // Tipo do target, pode ser: user, member, group ou tag
    }, {
        "id": "id" // 'id' do user/member/group/tag no Layers
        "kind": "group"
    }, {
        "email": "email@escola.com.br" // 'email' do user
        "kind": "user"
    }]
    "roles": ["guardian"], // Permissões que serão notificadas (pode ser omitida caso os todos os targets tenham `"kind": "user"`)
}
```

A chave `targets` recebe um Array de objetos com as seguintes chaves:

* `kind`: Indica o tipo do target, pode ser `user`, `member`, `group` ou `tag`
* `alias` | `id` | `email`: Indica o identificador do target. **Obs.: Informe apenas uma destas chaves por target.**

> **Obs.:** A chave `roles` pode ser omitida caso todos os `targets` tenham `"kind": "user"`.
