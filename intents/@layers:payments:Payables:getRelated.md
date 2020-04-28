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
    
    // ID interno do grupo de cobranças, para possível fetch individual futuro
    id: "RA",
    
    // Título do grupo de cobranças. Pode ser o nome do aluno, ou como preferirem que apareça este "grupo"
    title: "Aluno Munir Seduca",
    
    // Descrição do grupo de cobranças (aceita Quebras de linha)
    description: "Responsável Financeiro: ...\n...",
    
    // Qual é o número total de parcelas (por padrão, será a contagem de payables)
    installments: 12,
    
    // Lista de cobranças que fazem parte desse grupo
    payables: [
      {
    
        // ID interno da cobrança no ERP, para possível futura integração
        id: "ID_DA_COBRANÇA",
    
        // Identificador personalizado da cobrança (opcional, será vísivel na Interface)
        alias: "3213",
    
        // Qual é o número da parcela dessa cobrança?
        installment: 1,
    
        // Descrição da cobrança (aceita markdown)
        description: "Mensalidade referente ao mês de Janeiro de 2020",
    
        // Status da cobrança, valores possíveis: 
        // paid: Pago
        // partially_paid: Parcialmente Pago
        // pending: Aguardando Pagamento
        // open: Em Aberto
        // canceled: Cancelado
        // late: Atrasado 
        status: "pending",
    
        // Data de vencimento da cobrança (formato AAAA-MM-DD)
        dueAt: "2020-02-01",
    
        // Quando a cobrança foi paga (opcional. formato AAAA-MM-DD)
        paidAt: null,
    
        // Quando a cobrança foi enviada (opcional. formato AAAA-MM-DD)
        sentAt: null,
    
        // Valor que já foi pago
        centsPaid: 0,
    
        // Valor total a ser pago (com multas/taxas, se aplicável)
        centsTotal: 150000,
    
        // Valor original (sem multas/taxas)
        centsOriginal: 150000,
    
        boleto: {
    
          // Link para baixar o boleto
          link: "https://boleto.pdf", //URL,
    
          // Linha digitável do boleto, será usada para o usuário copiar o código sem ter que baixar o boleto
          code: "12341234123412341234",
        },
    
        // Lista de anexos da cobrança (opcional)
        attachments: [
          {
    
            // Tipo do anexo
            // Valores possivels:
            // invoice: Nota fiscal
            // file: Arquivo
            kind: 'file',
    
            // Nome do anexo
            title: "Comprovante de estorno",
    
            // Descrição do anexo
            description: "Estorno realizado em 31/10/2019",
    
            // Link para baixar o anexo
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
