
# Sincronizar dados

### Introdução

A sinconização de dados para o Layers education cria usuários, membros e grupos bem como seus vínculos uns aos outros na plataforma. Esse processo é ideal para quando é necessário realizar atualizações de uma grande quantidade de dados simultaneamente.

> **Atenção**: para realizar esse procedimento é necessário ter permissão ```sync:manage```

> **CUIDADO**: Essa rota altera pode alterar, criar e apagar dados na sua comunidade. 

### 1 - Crie um token para o seu app de sincronização


Todas as rotas da API do Layers Education exigem autenticação para que seja garantida sua segurança. Desse modo, para realizar uma requisição para a rota de sync será necessário que sua aplicação passe nos headers a chave "Authorization" com seu token. Caso você ainda não tenha um token para o seu app, clique (aqui)[http://link.com] para cadastrar seu app criá-lo. 

### 2 - Defina os dados que deseja importar

Os dados devem estar no formato abaixo para serem enviados para a rosta de sincronização: 

```JSON

{
	"members": {
		"data": [
			{
				"name": "aluno exemplo",
				"alias": "ID123",
				"birth": "22/08/2004",
				"active": true,
				"users": [
					"familiar.exemplo@email.com"
				]

			}
		],
		"cleansync": false
	},
	"users": {
		"data": [
			{
				"name": "familiar exemplo",
				"alias": "ID456",
				"birth": "16/11/1962",
				"roleSet": {
					"guardian": true,
				},
				"active": true,
				"invite": false,

			}
		],
		"cleansync": false
	},
	"classrooms": {
		"data": [
			{
				"name": "turma exemplo",
				"alias": "ID789",
				"active": true,
				"members": [
					"ID123"
				],
				"admins": [
					"ID456"
				],
				"tags": [
					"ensino médio"
				]
			}
		],
		"cleansync": false
	}
}

```

> **CUIDADO**:  A chave de cleansync define se devem ser apagados documentos do tipo especificado que não estão na planilha. Se a sincronização for incremental utilize **```cleansync: false```** como no exemplo acima

### 3 - Envie esse esses para a nossa API

Para efetuar a sincronização de dados, envie uma requisição do tipo POST para a /sync com os headers de de autorização e identificador da comunidade e o seu objeto no corpo. Para mais detalhes sobre a requisição, seus headers, corpo e possíveis respostas, consulte a (specificação da rota de sincronização)[http://sync.spec.com]

### 4 - Testando sua sincronização

Para realizar testes de sincronização de dados, utilize o identificador da sua comunidade de desenvolvimento do app no header ```community-id``` e seu token na chave ```authorization```.