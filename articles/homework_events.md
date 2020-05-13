# Criando tarefas na Agenda

### Introdução

A automatização da criação de tarefas na agenda por meio da API com a rota de `/post`  pode ser feito seguindo o passoa passo abaixo.

### 1 - Crie um token de autenticação

Todas as rotas da API do Layers Education exigem autenticação para que seja garantida sua segurança. Desse modo, para realizar uma requisição para a rota de fazer publicações2 você deve ter um token. Caso ainda não tenha cadastrado seu app ou não tenha um token, siga esse [tutorial](https://github.com/layers-digital/docs/blob/master/articles/app_register.md).

### 2 - Monte o conteúdo do seu post

A payload da criação de uma tarefa pode ser vista abaixo. Cada um dos campos está expicado abaixo do exemplo de código.

```js
{
      type: 'event',
      title: 'título da publicação',
      text: 'texto do corpo da publicação',
      roles: ['student', 'guardian'],
      date: '2020-05-13T18:00:00.000Z',
      viewAt: '2020-05-22T18:00:00.000Z',
      targets: [
        {
            _id: 'id_exemplo',
            type: 'group'
        }
      ],
      event: {
          allDay: false,
          endDate: '2020-05-13T23:59:59.999Z',
      }
}
```

+ **type**: Existem diversos tipos de posts no Layers. O mais indicado para a criação de tarefas é o ```event``` como mostrado acima.
+ **title**: Título da sua publicação que aparecerá na linha do tempo e na agenda de no máximo 45 caractéres.
+ **text**: Texto do corpo da publicação que aparecerá quando o usuário clicar na publicação em markdown.
+ **role**: Perfis de usuários que receberão a publicação.
+ **date**: Data do evento.
+ **viewAt**: Data na qual o evento deve aparecer para os usuários.
+ **targets**: Público alvo da publicação. Array contento objetos com a string ```id``` com o identificador de um membro, usuário, grupo ou tag e a string ```type``` contendo o tipo dessa entidade. Nesse caso, o público alvo são os Familiares ou Alunos que estão vinculados com o grupo de id_exemplo/
+ **event**: Objeto contendo o booleano ```allDay``` indicando se o evento deve durar o dia todo e a string ```endDate``` com a data de final do evento.

### 3 - Envie sua publicação

Com as informações da publicação já configuradas de acordo com o modelo acima, envie sua publicação fazendo uma requisição do tipo **POST** para `/post` de acordo com os headers explicados [aqui](link.com). Essa requisição retornará o documento da sua publicação. 