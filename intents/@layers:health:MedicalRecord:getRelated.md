# Intent `@layers:health:MedicalRecord:getRelated`

Esta intent é usada para obter dados de ficha médica de uma ou mais pessoas.

Deve haver uma API que recebe uma requisição `POST` com a seguinte estrutura de dados:

#### Requisição:

```js
{
  "context": {
    "issuedAt": Date,  // Quando a chamada foi feita
    "action": "@layers:health:MedicalRecord:getRelated",
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
    // Prontuários médicos (medical records)
    {
      // ID interno do prontuário (opcional)
      "id": "0001",

      // Nome da pessoa do prontuário (obrigatório)
      "name": "Ivan Seidel",

      // Última atualização do prontuário (opcional)
      "updatedAt": "2020-10-31T09:00:00Z",

      // Seções do prontuário (obrigatório)
      "sections": [
        {
          // Título da seção do prontuário
          "title": "Contato de emergencia",

          // Campos do da seção do prontuário
          "fields": [
            // Os "fields" tem 4 tipos ("types") possíveis: "number", "text", "tags" e "attachment"

            // Field "type": "number"
            // Campos aceitos: "title", "caption", "number", "unit" e "loinc"
            { 
              "type": "number", // obrigatório
              "title": "Peso", // obrigatório
              "caption": "(Medido com roupa)", // opcional
              "number": 57.5, // Tipos aceitos: Number ou String (obrigatório)
              "unit": "kg", // opcional
              "loinc": "3141-9" // LOINC Code Reference: https://loinc.org/3141-9/ (opcional)
            },

            // Field "type": "text"
            // Campos aceitos: "title", "caption", "number", "unit" e "loinc"
            { 
              "type": "text", // obrigatório
              "title": "Paulo Yoko Amancio", // obrigatório
              "caption": "Responsável legal", // opcional
              "text": "+55 11 99331-6662 (cel)", // Tipo aceito: String (obrigatório)
              "loinc": "54136-7" // LOINC Code Reference: https://loinc.org/54136-7/ (opcional)
            },

            // Field "type": "tags"
            // Campos aceitos: "title", "tags", "mood" e "loinc"
            { 
              "type": "tags", // obrigatório
              "title": "Tipo sanguineo", // obrigatório
              "tags": ["O-", "B-"], // Tipos aceitos: String ou Array String (obrigatório)
              "mood": "neutral", // Valores possíveis: neutral, success, warning e danger (obrigatório)
              "loinc": "34532-2" // LOINC Code Reference: https://loinc.org/34532-2/ (opcional)
            },

            // Field "type": "attachment"
            // Campos aceitos: "title", "attachment", "attachment.title", "attachment.type" e "attachment.url"
            { 
              "type": "attachment",
              "title": "Title",
              "attachment": {
                "title": "Cálculo de notas.pdf", // Título do anexo
                "type": "file", // Valores possíveis: "file" | "link" | "image"
                "url": "https://hospitalexample.com.br/prontuariojaneiro.pdf" // URL para baixar o anexo
              }
            }
          ]
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
