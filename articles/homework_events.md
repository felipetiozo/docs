# Criando eventos na Agenda

### Introdução

A automatização da criação de publicações na agenda por meio da API com a rota de `/post`  pode ser feito seguindo o passo a passo abaixo.

### 1 - Monte o conteúdo do seu post

A payload da criação de um evento pode ser vista abaixo. Cada um dos campos está expicado abaixo do exemplo de código.

```js
// POST /post
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
        }
      ]
}
```

+ **type**: Existem diversos tipos de posts no Layers. O mais indicado para a criação de tarefas para casa é o ```activity``` como mostrado acima. Os tipos de publicações existentes no Layers estão explicados [aqui](#Tipos-de-publicações-na-linha-do-tempo).
+ **title**: Título da sua publicação que aparecerá na linha do tempo e na agenda de no máximo 45 caractéres.
+ **text**: Texto do corpo da publicação que aparecerá quando o usuário clicar na publicação em markdown.
+ **role**: Perfis de usuários que receberão a publicação.
+ **date**: Data do evento.
+ **viewAt**: Data na qual o evento deve aparecer para os usuários.
+ **targets**: Público alvo da publicação. Array contento objetos as chaves:
  + **id|email|alias**: Indica o identificador do target. Obs.: Informe apenas uma destas chaves por topic.
  + **kind**: Indica o tipo do target. Pode ser ```user```, ```member```, ```group``` ou ```tag```.
+ **event**: Objeto contendo o booleano ```allDay``` indicando se o evento deve durar o dia todo e a string ```endDate``` com a data de final do evento.

### 2 - Envie sua publicação

Com as informações da publicação já configuradas de acordo com o modelo acima, envie sua publicação fazendo uma requisição do tipo **POST** para `/post` de acordo com os headers explicados [aqui](link.com). Essa requisição retornará o documento da sua publicação. 

### 3 - Editando sua publicação

É possível editar uma publicação fazendo uma requisição **PUT** para `/post/{{ postId }}`, onde `postId` é o ID da publicação que é retornado no momento de sua criação, na chave `id`, o corpo da requisição deve conter os dados a serem modificados, por exemplo:

Editando o título e o texto de uma publicação:

```js
// PUT /post/:postId
{
      "title": "Título novo",
      "text": "Texto novo"
}
```

### Tipos de publicações na linha do tempo

##### 1 - ```event```

Cria um evento na linha do tempo e na agenda. Aceita os campos explicados abaixo:

+ **title**: Título da publicação
+ **text**: Texto de corpo da publicação
+ **date**: Data de início
+ **event**: Objeto contendo a propriedade ```endDate``` com a data de fim do evento

##### 2 - ```message```

Cria um informativo na linha do tempo. Aceita os campos explicados abaixo:

+ **title**: Título da publicação
+ **text**: Texto de corpo da publicação

##### 3 - ```report```

Cria um relatório que pode ser respondido individualmente para cada aluno nos targets. Aceita os campos explicados abaixo:

+ **title**: Título da publicação
+ **text**: Texto de corpo da publicação
+ **answers**: Array contendo objetos com as respostas de cada pergunta do modelo de relatório para cada um dos alunos no público alvo
+ **report**: Objeto contento a chave ```_id``` com o id do modelo de relatório e a chave ```name``` com o nome do modelo de relatório

##### 4 - ```banner```

Cria uma imagem em destaque na linha do tempo. Aceita os campos explicados abaixo:

+ **title**: Título da publicação
+ **banner**: Objeto contendo a chave ```link``` com o link para o qual o usuário deve ser redirecionado ao clicar na imagem
+ **attachments**: Array contendo o objeto do upload da imagem que deve aparecer em destaque na linha do tempo

##### 5 - ```gallery```

Cria uma galeria na linha do tempo. Aceita os campos explicados abaixo:

+ **title**: Título da publicação
+ **text**: Texto de corpo da publicação
+ **attachments**: Array contendo até 30 objetos de uploads de imagens com até 5MB cada

##### 6 - ```test```

Cria uma prova na agenda. Aceita os campos explicados abaixo:

+ **title**: Título da publicação
+ **text**: Texto de corpo da publicação
+ **date**: Data que a prova será agendada

##### 7 - ```activity```

Cria uma atividade de aula na agenda. Aceita os campos explicados abaixo:

+ **title**: Título da publicação
+ **text**: Texto de corpo da publicação
+ **date**: Data que a atividade será agendada

##### 8 - ```diary```

Cria uma atividade de aula na agenda. Aceita os campos explicados abaixo:

+ **title**: Título da publicação
+ **text**: Texto de corpo da publicação
+ **date**: Data que a diário de aula será agendada















