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
      "alias": String | Number | null,  // Alias do usuário
      "timezone": String,  // Fuso horário do usuário
      "language": String,  // Língua preferencial do usuário
      "accountId": String,  // ID da account do usuário
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
      // ID interno do grupo de livro de notas, para possível fetch individual futuro (opcional)
      "id": "0001",

      // Período letivo (opcional)
      "season": "2019",

      // Nome do estudante (obrigatório)
      "student": "Ivan Seidel Gomes",

      // Nome do curso/série (obrigatório)
      "course": "9º Ano",

      // Status desse livro de notas (obrigatório)
      // Valores possívels: 'current' | 'ended'
      // current: Livro de notas do ano atual
      // ended: Livro de notas de um ano passado, já encerrado
      "status": "ended",

      // Lista de livros de notas para cada período deste ano letivo (obrigatório)
      "terms": [
        {
          // Título do período letivo (obrigatório)
          "label": "1º Bimestre",

          // Data de início do período (obrigatório)
          "startsAt": "2019-01-10", // Formato ISO 8601 (date: YYYY-MM-DD)

          // Data de encerramento do período (obrigatório)
          "endsAt": "2019-04-10", // Formato ISO 8601 (date: YYYY-MM-DD)

          // Status do período letivo (obrigatório)
          // Valores possíveis: 'unknown' | 'scheduled' | 'current' | 'ended'
          "status": "ended",

          // Lista de resultados de disciplinas (opcional)
          "subjects": [
            {
              // Título da disciplina (obrigatório)
              "label": "Português",

              // Abreviação da disciplina (opcional)
              "abbr": "PORT",

              // Lista de atividades (opcional)
              "activities": [{

                // Categoria da atividade (opcional)
                "category": "Provas",

                // Nome da atividade (obrigatório)
                "label": "Prova 1",

                // Descrição da atividade (opcional)
                "description": "Verbos e substantivos",

                // Data da atividade (opcional)
                "date": "2020-03-02",

                // Comentário sobre a atividade (opcional)
                "comment": "O aluno deve estudar mais o infinitivo",

                // Nota que o aluno recebeu (String, Number ou null) (obrigatório)
                "scoreGiven": "6.5",

                // Nota máxima da atividade (opcional)
                "scoreMaximum": "10",

                // Indica se a nova é boa (nota azul), ruim (nota vermelha) ou neutra (opcional)
                // Valores possíveis: 'neutral' | 'good' | 'bad'
                "scoreMood": "good",

                // Quando saiu a nota (opcional)
                "scoreDate": "2020-03-19"
              }],

              // Lista de resultados gerais (opcional)
              "overall": [{

                // Tipo do resultado (obrigatório)
                // Valores possívels: 'other' | 'attendance' | 'partial_grade' | 'final_grade'
                "type": "attendance",

                // Título do resultado (obrigatório)
                "label": "Faltas",

                // Nota que o aluno recebeu neste resultado (String, Number ou null) (obrigatório)
                "scoreGiven": "6.5",

                // Nota máxima do resultado (opcional)
                "scoreMaximum": "10",

                // Indica se a nova é boa (nota azul), ruim (nota vermelha) ou neutra (opcional)
                // Valores possíveis: 'neutral' | 'good' | 'bad'
                "scoreMood": "good",

                // Indica se o resultado é o mais importante (opcional)
                "featured": true
              }],

              // Lista de categorias (opcional)
              "categories": [{
                // Mesmo nome utilizado na chave 'category' da 'Activity' (obrigatório)
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

      // Lista de anexos desse livro de notas (opcional)
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
