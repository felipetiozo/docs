# O que é o API Hub?

O Hub de APIs é uma forma de simplificar a comunicação entre apps que requisitam informações e apps de serviços utilizados pela instituição de ensino que contém essas informações. Com esse objetivo, o Layers implementa actions: protocolos de comunicação que devem ser seguidas tanto pelo app que faz uma requisição de informação para o Layers quanto pelo app que recebe e responde por aquela requisição. Apps que tem a feature de serviços habilitada podem requisitar ou responder actions nas comunidades em que estão instalados.

### Fluxo de uma action

##### 1 - Encontrar provedores para uma determinada action

O primeiro passo de um app que tem permissão para requisitar uma determinada action é identificar quais são os possíveis provedores daquela informação. Isso pode ser feito através da requisição mostrada abaixo: 

###### **GET** ```/services/discover/{action}?version={versao}&_community={idComunidade}```
+ **action**: Identificador da notificação 
+ **version**: Versão do app
+ **_community**: Identificador da comunidade


Essa requisição retorna os provedores que podem responder a action especificada como parametro na rota mostrada acima no seeguinte formato:

```js
{
    "version": Number, // Versão do app
    "communityInstallations": [String], // Provedores instalados que podem responser por essa ação
    "action": String // Action 
}
```
##### 2 - Envio de uma requisição para um dos provedores de uma informação

Uma vez que o app sabe qual provedor irá usar, o app deve realizar a requisição abaixo para:

###### **GET** ```/services/call/{action}/{provider}?version={versao}&_community={idComunidade}```
```js
{
  "context": {
    "issuedAt": Date,  // Quando a chamada foi feita
    "action": "@layers:action:de:exemplo", // Identificador da action
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

Ao receber essa requisição, o Layers verifica não somente o corpo da requisição mas também  se o app tem permissão para requisitar a action especificada checando também se o provider especificado existe e está instalado na comunidade especificada. Apenas se a requisição passar nas validações ela será repassada para o provedor.

##### 3 - Repasse da resposta validada

Ao receber a resposta do provedor, o Layers valida se ela está de acordo com o padrão de resposta esperado para essa action. Se a resposta for validada com sucesso ela é então repassada para o app que fez a requisição.

### Hubs disponíveis

O Layers já conta com uma série de actions implementadas para atender as principais necessidades das escolas. As documentações para cada uma dessas actions pode ser encontrada [aqui](https://github.com/layers-digital/docs/tree/master/intents). Caso não encontre uma action que contemple as necessidades do seu app, entre em contatdo [conosco](mailto:name@email.com)!
