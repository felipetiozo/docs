# Tipos de Publicação no Layers Education

A comunicação no Layers Education pode ser feita através de diversos tipos de publicações separados em dois grupos principais: [Publicações para a linha do tempo](#Publicações-para-a-linha-do-tempo) e [Publicações para a agenda](#Publicações-para-a-agenda). As características e exemplos de uso de cada um desses tipos de publicação está explicado abaixo.

### Estrutura básica de uma publicação

Todos os tipos de publicações compartilham alguns campos obrigatórios. O objeto abaixo contém os campos presentes em qualquer post do Layers Education.

```js
{
    "title": "informativo de exemplo",
    "text": "texto do corpo do informativo",
    "roles": ["student", "guardian"],
    "targets": [
        {
            "id": "id_exemplo",
            "type": "group"
        }
    ]
}
```

+ **title**: Título da sua publicação que aparecerá na linha do tempo ou na agenda. Deve ter no máximo 45 caractéres.
+ **text**: Texto do corpo da publicação que aparecerá quando o usuário clicar na publicação em sintaxe markdown.
+ **roles**: Perfis de usuários que receberão a publicação.
+ **viewAt**: Data na qual o evento deve aparecer para os usuários. Quando não especificado, a publicação é enviada instantaneamente.
+ **targets**: Público alvo da publicação. Array contento objetos as chaves:
  + **id | email | alias**: Indica o identificador do target. Obs.: Informe apenas uma destas chaves por target.
  + **kind**: Indica o tipo do target. Pode ser ```user```, ```member```, ```group``` ou ```tag```.

### Publicações para a linha do tempo

##### 1 - Informativo (```message```)

Informativos são a publicação recomendada para recados ou avisos simples. Esse tipo de publicação ser visto na linha do tempo. Abaixo da payload de exemplo estão explicados cada um dos campos:

```js
{
    "type": "message",
    "title": "informativo de exemplo",
    "text": "texto do corpo do informativo",
    "roles": ["student", "guardian"],
    "targets": [
        {
            "id": "id_exemplo",
            "type": "group"
        }
    ]
}
```
+ **type**: Tipo da publicação. No caso do informativo o tipo é ```message```
+ **title**: Título do informativo
+ **text**: Texto de corpo do informativo

##### 2 - Evento (```event```)

Eventos são o tipo de publicação ideal para avisos que tem uma data específica como um convite ou lembrete de dever de casa. Esse tipo de publicação vai tanto para a linha do tempo quanto para a agenda. 

> obs: eventos são o único tipo de publicação que aparece tanto na linha do tempo quanto na agenda.

```js
{
      "type": "event",
      "title": "título da publicação",
      "text": "texto do corpo da publicação",
      "date": "2020-05-13T18:00:00.000Z",
      "event": {
          "allDay": false,
          "endDate": "2020-05-13T23:59:59.999Z",
      }
      "roles": ["student", "guardian"],
      "targets": [
        {
            "id": "id_exemplo",
            "type": "group"
        }
      ]
}
```

+ **type**: Tipo da publicação. No caso do evento o tipo é ```event```
+ **title**: Título do evento
+ **text**: Texto de corpo do evento
+ **date**: Data de início do evento
+ **event**: Objeto contendo as chaves ```endDate``` com a data de quando acaba o evento e ```allDay```com um booleano indicando se o evento ocupará o dia todo.

##### 3 - Imagem em Destaque (```banner```)

Imagens em destaque são ideais para chamar atenção dos usuários e realizar divulgações. Esse tipo de publicação aparece como uma imagem sem texto na frente na linha do tempo que pode ou não funcionar como um link para outra página.

```js
{
    "type": "banner"
    "title": "título da publicação",
    "banner": {
        "link": "https://link.com"
    }
    "attachments": []
    "roles": ["student", "guardian"],
    "targets": [
        {
            "id": "id_exemplo",
            "type": "group"
        }
    ]
}
```

> **obs**: Publicações do tipo Imagem em Destaque não tem texto de corpo e o título não fica visível, sendo usado apenas para buscar a publicação.

+ **type**: Tipo da publicação. No caso da imagem em destaque o tipo é ```banner```
+ **title**: Título da publicação
+ **attachments**: Array contendo o objeto que referencia a imagem anexada ao post

##### 4 - Galeria de fotos (```gallery```)

Galerias de fotos são publicadas na linha do tempo do app e são ideais para a divulgação de atividades realizadas. Elas podem conter até 30 imagens de até 5MB cada.

> obs: Para fazer o upload de uma imagem pode ser feito através da rota ```/media/upload```. Essa rota retorna os objetos contendo as informações da imagem que estão mostrados no array de attachments.

```js
{
    "type": "gallery"
    "title": "título da publicação",
    "attachments": [
        {"objeto da imagem 1"},
        {"objeto da imagem 2"}
    ]
    "roles": ["student", "guardian"],
    "targets": [
        {
            "id": "id_exemplo",
            "type": "group"
        }
    ]
}
```

+ **type**: Tipo da publicação. No caso da galeria o tipo é ```gallery```
+ **title**: Título da publicação
+ **attachments**: Array contendo os objetos que referenciam as imagens anexadas ao post

##### 5 - Relatório induvidual (```report```)

Relatórios inddividuais sào publicações que, além dos campos de título e texto de corpo tem também um formulário personalizado cujos campos devem ser respondidos individualmente para cada um dos memberos no público alvo. As informações de título e texto de corpo são recebidas por todos os usuários na linha do tempo juntamente às repostas para as perguntas do formulário relativas aos alunos a quem estão vinculados.

```js
{
    "type": "report"
    "title": "título da publicação",
    "report": {
        "_id": "id_do_modelo_de_relatorio"
        "name": "nome_do_modelo_de_relatorio"
    },
    "answers": [
        {"respostas para o aluno 1"},
        {"respostas para o aluno 2"}
    ]
    "attachments": []
    "roles": ["student", "guardian"],
    "targets": [
        {
            "id": "id_exemplo",
            "type": "group"
        }
    ]
}
```

**title**: Título da publicação
**text**: Texto de corpo da publicação
**answers**: Array contendo objetos com as respostas de cada pergunta do modelo de relatório para cada um dos alunos no público alvo
**report**: Objeto contento a chave _id com o id do modelo de relatório e a chave name com o nome do modelo de relatório

### Publicações para a agenda

##### 1 - Prova (```test```)

Publicações do tipo prova são ideais para marcar atividades avaliativas. 

```js
{
      "type": "test",
      "title": "título da publicação",
      "text": "texto do corpo da publicação",
      "roles": ["student", "guardian"],
      "date": "2020-05-13T18:00:00.000Z",
      "viewAt": "2020-05-22T18:00:00.000Z",
      "targets": [
        {
            "id": "id_exemplo",
            "type": "group"
        },
        {
            "alias": "alias_exemplo",
            "type": "member"
        },
        {
            "email": "email_exemplo",
            "type": "user"
        }
      ]
}
```

+ **type**: Tipo da publicação. No caso da prova é ```test```
+ **title**: Título da sua publicação que aparecerá na linha do tempo e na agenda de no máximo 45 caractéres.
+ **text**: Texto do corpo da publicação que aparecerá quando o usuário clicar na publicação em markdown.
+ **date**: Data da prova.
 
##### 2 - Atividade de aula (```activity```)

Publicações do tipo atividade de aula são ideais para marcar marcar tarefas para casa e outras lições na agenda.

```js
{
      "type": "activity",
      "title": "título da publicação",
      "text": "texto do corpo da publicação",
      "roles": ["student", "guardian"],
      "date": "2020-05-13T18:00:00.000Z",
      "viewAt": "2020-05-22T18:00:00.000Z",
      "targets": [
        {
            "id": "id_exemplo",
            "type": "group"
        },
        {
            "alias": "alias_exemplo",
            "type": "member"
        },
        {
            "email": "email_exemplo",
            "type": "user"
        }
      ]
}
```

+ **type**: Tipo da publicação. No caso da atividade de aula é ```activity```
+ **title**: Título da sua publicação que aparecerá na linha do tempo e na agenda de no máximo 45 caractéres.
+ **text**: Texto do corpo da publicação que aparecerá quando o usuário clicar na publicação em markdown.
+ **date**: Data da atividade de aula.

##### 3 - Atividade de aula (```diary```)

Diários de aula na agenda são ideais para comunicar quais atividades foram ou serão feitas em um dia.

```js
{
      "type": "activity",
      "title": "título da publicação",
      "text": "texto do corpo da publicação",
      "roles": ["student", "guardian"],
      "date": "2020-05-13T18:00:00.000Z",
      "viewAt": "2020-05-22T18:00:00.000Z",
      "targets": [
        {
            "id": "id_exemplo",
            "type": "group"
        },
        {
            "alias": "alias_exemplo",
            "type": "member"
        },
        {
            "email": "email_exemplo",
            "type": "user"
        }
      ]
}
```

+ **type**: Tipo da publicação. No caso do diário de aula é ```diary```
+ **title**: Título da sua publicação que aparecerá na linha do tempo e na agenda de no máximo 45 caractéres.
+ **text**: Texto do corpo da publicação que aparecerá quando o usuário clicar na publicação em markdown.
+ **date**: Data do diário de aula.