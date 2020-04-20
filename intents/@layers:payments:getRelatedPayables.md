# Intent `@layers:payments:getRelatedPayables`

Esta intent é usada para obter dados de cobranças para a tela de "Boletos" para um usuário específico.

Deve haver uma API que recebe uma requisição `POST` com a seguinte estrutura de dados:

#### Requisição:

```js
{
  "context": {
    "issuedAt": Date,  // Quando a chamada foi feita
    "action": '@layers:payments:getRelatedPayables',
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
  // Grupo de cobranças
  {
    // ID interno do grupo de cobranças
    "id": "RA",

    // Título do grupo de cobranças
    "title": "Aluno Munir Seduca",

    // Descrição do grupo de cobranças
    "description": "Responsável Financeiro: ...\n...",

    // Lista de cobranças que fazem parte desse grupo
    "payables": [
      {
        // ID interno da cobrança no ERP
        "id": "ID_DA_COBRANÇA",

        // Identificador personalizado da cobrança (opcional)
        "alias": "mensalidade_01_2020",

        // Título da cobrança
        "title": "Mensalidade 01/2020",

        // Descrição da cobrança
        "description": "Mensalidade referente ao mês de Janeiro de 2020",

        // Status da cobrança
        // Valores possíveis: 'paid' | 'partially_paid' | 'pending' | 'open' | 'canceled' | 'late' 
        "status": "pending",

        // Data de vencimento da cobrança
        "dueAt": "2020-02-01",

        // Quando a cobrança foi paga
        "paidAt": null,

        // Quando a cobrança foi enviada
        "sentAt": null,

        // Valor que já foi pago
        "paidCents": 0,

        // Valor total a ser pago (com multas/taxas, se aplicável)
        "totalCents": 150000,

        // Valor original (sem multas/taxas)
        "originalCents": 150000,

        // Qual é o número de parcela essa cobrança
        "installment": 1,
        // Qual é o número total de parcelas
        "installments": 12,

        "boleto": {
          // Link para baixar o boleto
          "downloadUrl": "https://boleto.pdf", //URL,

          // Linha digitável do boleto, será usada para o usuário copiar o código sem ter que baixar o boleto
          "digitableCode": "12341234123412341234",
        },

        // Lista de anexos da cobrança
        "attachments": [
          {
            // Tipo do anexo
            // Valores possivels: 'invoice' | 'file'
            "kind": "file",

            // Nome do anexo
            "title": "Nota fiscal",

            // Descrição do anexo
            "description": "Nota fiscal referente à cobrança",

            // Link para baixar o anexo
            "downloadUrl": "https://arquivo.pdf",
          },
        ]
      }
    ]
  }
]
}
```

**ATENÇÃO:** Os comentários foram adicionados apenas para explicar as estruturas de dados, nem a requisição e nem a resposta devem ter comentários, ambos devem ser JSONs válidos.
