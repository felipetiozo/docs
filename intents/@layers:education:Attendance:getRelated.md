# Intent `@layers:education:Attendance:getRelated`

Esta intent é usada para retornar a frequência de um ou mais alunos relacionados. Essa intent contempla 3 visões de frequência:
- Visão geral da frequência do curso
- Visão geral por disciplinas
- Visão geral por categorias (ou períodos)

As categorias podem ser utilizadas para agrupadar registros de frequência e listar uma visão geral dessa categoria relacionando com a disciplina. Ex: Faltas e Carga Horária do 1º bimestre de Matemática.

Deve haver uma API que recebe uma requisição `POST` com a seguinte estrutura de dados:

#### Requisição:

```js
{
  "result": [
    // Frequência de um determinado aluno
    {

      // ID interno do histórico, para possível fetch individual futuro (opcional)
      "id": "0001",

      // Nome do aluno (obrigatório)
      "student": "Marília Castelli",

      // Curso que o aluno está matrículado (obrigatório)
      "course": "9º Ano - Ensino Fundamental",

      // Visão geral da frequência do curso (obrigatório)
      "overall": {
        // Carga de trabalho geral (apenas valores positivos)
        "workload": 1600, // Tipo: Integer (opcional)

        // Unidade da carga de trabalho. Valores possíveis: "h", "m"
        "workloadUnit": "h", // Valor padrão: "h" (opcional)

        // Percentual total de frequência 
        "presencePercentage": 0.10, // Valor de 0 - 1 (opcional)

        // Total de faltas
        "absenceTotal": 10, // Tipo: Integer (opcional)

        // Indica se a frequência está boa (nota verde), ruim (nota vermelha) ou neutra (opcional)
        // Valores possíveis: "neutral", "attention", "good", "bad"
        "mood": "bad" // Valor padrão: "neutral" (opcional)
      },

      // Total de frequência computada por disciplina (opcional)
      "subjects": [{
        // Nome da disciplina (obrigatório)
        "name": "Lingua Portuguesa", 

        // Visão geral da disciplina (opcional)
        "overall": {
          // Carga de trabalho total da disciplina (apenas valores positivos)
          "workload": 40, // Tipo: Integer (opcional)

          // Unidade da carga de trabalho. Valores possíveis: "h", "m"
          "workloadUnit": "h", // Valor padrão: "h" (opcional)

          // Percentual total de frequência 
          "presencePercentage": 0.10, // Valor de 0 - 1 (opcional)

          // Total de faltas
          "absenceTotal": 10, // Tipo: Integer (opcional)

          // Indica se a frequência está boa (nota verde), ruim (nota vermelha) ou neutra (opcional)
          // Valores possíveis: "neutral", "attention", "good", "bad"
          "mood": "bad" // Valor padrão: "neutral" (opcional)
        },

        // A frequência geral de uma disciplina pode ser agrupada em categorias
        // essas categorias podem ser "bimestres", "trimestres", "horal/escrita", "prática/teórica" "termos"...
        // (opcional)
        "categories": [{ 
          // Nome da categoria
          "name": "1º bimestre",

          // Carga de trabalho total da disciplina (apenas valores positivos)
          "workload": 40, // Tipo: Integer (opcional)

          // Unidade da carga de trabalho. Valores possíveis: "h", "m"
          "workloadUnit": "h", // Valor padrão: "h" (opcional)

          // Percentual total de frequência 
          "presencePercentage": 0.10, // Valor de 0 - 1 (opcional)

          // Total de faltas
          "absenceTotal": 10, // Tipo: Integer (opcional)

          // Indica se a frequência está boa (nota verde), ruim (nota vermelha) ou neutra (opcional)
          // Valores possíveis: "neutral", "attention", "good", "bad"
          "mood": "bad" // Valor padrão: "neutral" (opcional)
        }]
      }],

      // Registro de frequência por data (opcional)
      "records": [
        {
          // Tipo da frequência, podendo ser "Presente", "Falta", "Falta justificada" ou "Dispensado", 
          // Valores possíveis: "present", "absent", "justified_absency", "excused"
          "type": "absent", // Valor padrão: absent (opcional)

          // Data em que aconteceu a falta (obrigatório)
          "date": "2020-04-04", // ISO 8601 - (date: YYYY-MM-DD)

          // Disciplina em que ocorreu a falta (obrigatório)
          "subject": "Lingua Portuguesa",

          // Descrição da frequência (opcional)
          "caption": "09h30 - 10h30", // String

          // Categoria da falta, meio pelo qual ela pode ser agrupada. Ex: 1º bimestre (opcional)
          "category": "1º bimestre"
        }
      ]
    }
  ]
}
```

#### Resposta:

A API deve retornar apenas status 200 confirmando que a operação foi efetuada.

### Validações que sugerimos:
- `secret` é idêntica à salva no código? (IMPORTANTE)
- `context.community` é uma comunidade aceita? (DESEJAVEL)
- `context.action` é uma action implementada e válida? (IMPORTANTE)
- `data.user` possui permissão de acesso ao recurso? (OPCIONAL, pois o Layers já aplica regras de escopamento nas telas)
- `data.user` pertence à `context.comunity`? (OPCIONAL)

**Obs:** Caso algum erro ocorra, retornar códigos HTTP na faixa 4xx-5xx


**ATENÇÃO:** Os comentários foram adicionados apenas para explicar as estruturas de dados, nem a requisição e nem a resposta devem ter comentários, ambos devem ser JSONs válidos.
