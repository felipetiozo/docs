# Intent `@layers:education:GradeBooks:getRelated`

Esta intent é usada para obter dados de notas para a tela de "Notas" para um usuário específico.

Deve haver uma API que recebe uma requisição `POST` com a seguinte estrutura de dados:

#### Requisição:

```js
{
  "context": {
    "issuedAt": Date,  // Quando a chamada foi feita
    "action": '@layers:education:GradeBooks:getRelated',
    "community": String,  // Comunidade do usuário
  },
  "data": {
    "user": {
      "id": String,  // ID do usuário
      "name": String,  // Nome do usuário
      "email": String,  // Email do usuário
      "alias": String | Number | null,  // Alias do usuário
    },
  },
  "secret": String, // Chave secreta
}
```


A API deve retornar um JSON com o seguinte formato:

#### Resposta:

```js
{
  "result": [
    // Livro de notas individual
    {
      // Ano letivo (opcional)
      "season": "2019",

      // Nome do estudante
      "student": "Ivan Seidel Gomes",

      // Nome do curso/série
      "course": "9º Ano",

      // Status desse livro de notas
      // Valores possívels: 'current' | 'ended'
      // current: Livro de notas do ano atual
      // ended: Livro de notas de um ano passado, já encerrado
      "status": "ended",

      // Lista de livros de notas para cada período deste ano letivo
      "terms": [
        {
          // Título do período letivo
          "label": "1º Bimestre",

          // Data de início do período
          "startsAt": "2019-01-10",

          // Data de encerramento do período
          "endsAt": "2019-04-10",

          // Status do período letivo
          // Valores possíveis: 'unknown' | 'scheduled' | 'current' | 'ended'
          "status": "ended",

          // Lista de resultados de disciplinas
          "subjects": [
            {
              // Título da disciplina
              "label": "Português",

              // Abreviação da disciplina (opcional)
              "abbr": "PORT",

              // Lista de atividades
              "activities": [{

                // Categoria da atividade (opcional)
                "category": "Provas",

                // Nome da atividade
                "label": "Prova 1",

                // Descrição da atividade (opcional)
                "description": "Verbos e substantivos",

                // Data da atividade (opcional)
                "date": "2020-03-02",

                // Comentário sobre a atividade (opcional)
                "comment": "O aluno deve estudar mais o infinitivo",

                // Nota que o aluno recebeu (String, Number ou null)
                "scoreGiven": "6.5",

                // Nota máxima da atividade (opcional)
                "scoreMaximum": "10",

                // Indica se a nova é boa (nota azul), ruim (nota vermelha) ou neutra
                // Valores possíveis: 'neutral' | 'good' | 'bad'
                "scoreMood": "good",

                // Quando saiu a nota (opcional)
                "scoreDate": "2020-03-19"
              }],

              // Lista de resultados gerais
              "overall": [{

                // Tipo do resultado
                // Valores possívels: 'other' | 'attendance' | 'partial_grade' | 'final_grade'
                "type": "attendance",

                // Título do resultado
                "label": "Faltas",

                // Valor do resultado (String, Number ou null)
                "result": 8.1,

                // Indica se o resultado é bom, ruim ou neutro
                // Valores possívels: 'neutral' | 'good' | 'bad'
                "mood": "good",

                // Indica se o resultado é o mais importante
                "featured": true
              }],

              // Lista de categorias (opcional)
              "categories": [{
                // Mesmo nome utilizado na chave 'category' da 'Activity'
                "name": "Provas",

                // Descrição adicional da categoria (opcional)
                "description": "Provas discussivas e/ou objetivas com intuito de avaliar o aprendizado",

                // Ordem de aparição, do menor para maior (opcional)
                "order": 1
              }]
            }
          ]
        }
      ],

      // Lista de anexos desse livro de notas
      "attachments": [
        {
          // Título do anexo
          "title": "Cálculo de notas.pdf",

          // Tipo do anexo
          // Valores possíveis: 'file' | 'link' | 'image'
          "type": "file",

          // URL para baixar o anexo
          "url": "https://calculo.pdf"
        }
      ]
    }
  ]
}
```


### Validações que sugerimos:
- `secret` é idêntica à salva no código? (IMPORTANTE)
- `context.community` é uma comunidade aceita? (DESEJAVEL)
- `context.action` é uma action implementada e válida? (IMPORTANTE)
- `data.user` possui permissão de acesso ao recurso? (OPCIONAL, pois o Layers já aplica regras de escopamento nas telas)
- `data.user` pertence à `context.comunity`? (OPCIONAL)

**Obs:** Caso algum erro ocorra, retornar códigos HTTP na faixa 4xx-5xx


**ATENÇÃO:** Os comentários foram adicionados apenas para explicar as estruturas de dados, nem a requisição e nem a resposta devem ter comentários, ambos devem ser JSONs válidos.
