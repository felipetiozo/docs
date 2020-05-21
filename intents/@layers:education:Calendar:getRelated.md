# Intent `@layers:education:Calendar:getRelated`

Esta intent é usada para obter dados de calendário para um usuário específico.

Deve haver uma API que recebe uma requisição `POST` com a seguinte estrutura de dados:

#### Requisição:

```js
{
  "context": {
    "issuedAt": Date,  // Quando a chamada foi feita
    "action": "@layers:education:Calendar:getRelated",
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

Resposta:
```js
{
  "result": [
    // Grade horária individual
    {
      // ID interno da grade horária (opcional)
      "id": "0001",

      // Nome do estudante (obrigatório)
      "name": "Calendário acadêmico",

      // Intervalo de tempo do calendário
      "season": {
        // Data de inicio do calendário - Formato AAAA-MM-DD
        "dtstart": "2020-01-01",
        // Data de fim do calendário - Formato AAAA-MM-DD
        "dtend": "2020-12-31"
      },

      // Eventos do calendário (obrigatório)
      "events": [
        {
          // Nome do evento (obrigatório)
          "name": "Festa junina"
          // Data de inicio do evento - Formato AAAA-MM-DDTHH:mm:ssZ (ISO 8601) (obrigatório)
          "dtstart": "2020-07-27T09:00:00Z",
          // Data de final do evento - Formato AAAA-MM-DDTHH:mm:ssZ (ISO 8601) (obrigatório)
          "dtend": "2020-07-27T21:45:00Z",
          // Categoria do evento (opcional)
          "category": "Eventos"
        },
        {
          // Para representar um evento que ocupa o dia todo basta enviar apenas ANO-MES-DIA
          "name": "Corpus Christi"
          // Data de inicio e de final devem ser iguais neste caso - Formato AAAA-MM-DD
          "dtstart": "2020-06-11",
          "dtend": "2020-06-11",
          "category": "Feriados"
        },
        {
          // Para representar um evento que ocupa varios dias basta enviar apenas ANO-MES-DIA
          "name": "Férias escolares"
          "dtstart": "2020-06-01",
          "dtend": "2020-06-30",
          "category": "Ferias"
        }
      ],

      // Categorias associadas aos eventos (opcional)
      // São utilizadas para definir a cor de cada evento
      // Caso uma categoria seja definida no evento mas não nesta estrutura uma cor aleatória será designada automaticamente
      "categories": [
        {
          // Nome da categoria. Deve corresponder a uma categoria atribuida a pelo menos um evento (obrigatório)
          "name": "Eventos",
          // Cor que será atribuida a todos os eventos desta categoria (obrigatório)
          // Cores disponiveis:
          // TODO
          "color": "Blue"
        },
        {
          "name": "Feriados",
          "color": "red"
        },
        {
          "name": "Ferias",
          "color": "green"
        }
      ]
    }
  ]
}
```


### Validações que sugerimos:
- secret é idêntica à salva no código? (IMPORTANTE)
- context.community é uma comunidade aceita? (DESEJAVEL)
- context.action é uma action implementada e válida? (IMPORTANTE)
- data.user possui permissão de acesso ao recurso? (OPCIONAL, pois o Layers já aplica regras de escopamento nas telas)
- data.user pertence à context.comunity? (OPCIONAL)

**Obs:** Caso algum erro ocorra, retornar códigos HTTP na faixa 4xx-5xx

**ATENÇÃO:**: Os comentários foram adicionados apenas para explicar as estruturas de dados, nem a requisição e nem a resposta devem ter comentários, ambos devem ser JSONs válidos.