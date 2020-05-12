# Intent `@layers:payments:Payables:getRelated`

Esta intent é usada para obter dados de cobranças para a tela de "Boletos" para um usuário específico.

Deve haver uma API que recebe uma requisição `POST` com a seguinte estrutura de dados:

#### Requisição:

```js
{
  "context": {
    "issuedAt": Date,  // Quando a chamada foi feita
    "action": '@layers:payments:Payables:getRelated',
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
  // Grupo de cobranças
  {

    // ID interno do grupo de cobranças, para possível fetch individual futuro (obrigatório)
    id: "RA",

    // Título do grupo de cobranças. Pode ser o nome do aluno, ou como preferirem que apareça este "grupo" (obrigatório)
    title: "Aluno Munir Seduca",

    // Descrição do grupo de cobranças (aceita Quebras de linha) (opcional)
    description: "Responsável Financeiro: ...\n...",

    // Qual é o número total de parcelas (por padrão, será a contagem de payables) (opcional)
    installments: 12,

    // Lista de cobranças que fazem parte desse grupo (obrigatório)
    payables: [
      {

        // ID interno da cobrança no ERP, para possível futura integração (obrigatório)
        id: "ID_DA_COBRANÇA",

        // Identificador personalizado da cobrança, será vísivel na interface (opcional)
        alias: "3213",

        // Qual é o número da parcela dessa cobrança? (obrigatório)
        installment: 1,

        // Descrição da cobrança (aceita markdown) (opcional)
        description: "Mensalidade referente ao mês de Janeiro de 2020",

        // Status da cobrança, valores possíveis: (obrigatório)
        // paid: Pago
        // partially_paid: Parcialmente Pago
        // pending: Aguardando Pagamento
        // open: Em Aberto
        // canceled: Cancelado
        // late: Atrasado
        status: "pending",

        // Data de vencimento da cobrança (formato AAAA-MM-DD) (obrigatório)
        dueAt: "2020-02-01",

        // Quando a cobrança foi paga, formato AAAA-MM-DD (opcional)
        paidAt: null,

        // Quando a cobrança foi enviada,formato AAAA-MM-DD (opcional)
        sentAt: null,

        // Valor que já foi pago (obrigatório)
        centsPaid: 0,

        // Valor total a ser pago (com multas/taxas, se aplicável) (obrigatório)
        centsTotal: 150000,

        // Valor original (sem multas/taxas) (obrigatório)
        centsOriginal: 150000,

        // Arquivo e código do boleto (obrigatório)
        boleto: {

          // Link para baixar o boleto (obrigatório)
          link: "https://boleto.pdf", //URL,

          // Linha digitável do boleto, será usada para o usuário copiar o código sem ter que baixar o boleto (obrigatório)
          code: "12341234123412341234",
        },

        // Lista de anexos da cobrança (opcional)
        attachments: [
          {

            // Tipo do anexo (obrigatório)
            // Valores possivels:
            // invoice: Nota fiscal
            // file: Arquivo
            kind: 'file',

            // Nome do anexo (obrigatório)
            title: "Comprovante de estorno",

            // Descrição do anexo (opcional)
            description: "Estorno realizado em 31/10/2019",

            // Link para baixar o anexo (obrigatório)
            url: "https://url.para-download.com",
          },
        ]
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
