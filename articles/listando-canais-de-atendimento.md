# Listando canais de atendimento

Canais de atendimento no Layers são a maneira de organizar mensagens em grupos por departamento ou assunto. É possível listar os canais de atendimento existentes em uma determinada comunidade através da seguinte requisição:

##### **GET** ```/tickets/channels```

A resposta para essa requisição será uma lista dos canais de atendimento existentes.

```js
[
    {
        "id": String,
        "public": Boolean,
        "name": String,
        "agents": [String],
        "allowAudio": Boolean,
        "subjects": [
            {
                "title": String,
                "description": String
            }
        ],
    }
]
```

+ **id**: identificador do documento do canal de atendimento
+ **public**: indica se o canal será público ou interno. Se for público aparecerá para usuários quando forem criar uma solicitaçõa
+ **name**: nome do canal de atendimento
+ **agents**: array contendo os identificadores de documentos de usuários que podem responder mensagens para esse canal
+ **allowAudio**: indica se esse canal deve permitir o envio de mensagens de áudio
+ **subjects**: array contendo um objeto para cada assunto sugerido
    + **title**: título do assunto 
    + **description**: descrição do assunto

Caso deseje ver informações de um canal de atendimento específico, isso pode ser feito especificando o id como parametro na query.

##### **GET** ```/tickets/channels/{channelId}```

Essa requisição retorna um objeto com as informações do canal de atendimento especificado.

```js
{
    "id": String,
    "public": Boolean,
    "name": String,
    "agents": [String],
    "allowAudio": Boolean,
    "subjects": [
        {
            "title": String,
            "description": String
        }
    ],
}
```
