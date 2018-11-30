---
title: Referência de modelo de dados de modelo de gestão de API do Azure | Documentos da Microsoft
description: Saiba mais sobre as representações de entidade e tipo de itens comuns utilizados em modelos de dados para os modelos de portal do programador na gestão de API do Azure.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: b0ad7e15-9519-4517-bb73-32e593ed6380
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: apimpm
ms.openlocfilehash: 8c21ed737cab98c9136e1c1991997ff3931a4c9d
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/28/2018
ms.locfileid: "52447202"
---
# <a name="azure-api-management-template-data-model-reference"></a>Referência de modelo de dados de modelo de gestão de API do Azure
Este tópico descreve as representações de entidade e tipo de itens comuns utilizados em modelos de dados para os modelos de portal do programador na gestão de API do Azure.  
  
 Para obter mais informações sobre como trabalhar com modelos, consulte [como personalizar o portal do Programador de gestão de API através de modelos](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

O portal do Programador não está disponível na camada de consumo.

## <a name="reference"></a>Referência

-   [API](#API)  
-   [Resumo de API](#APISummary)  
-   [Aplicação](#Application)  
-   [Anexo](#Attachment)  
-   [Exemplo de código](#Sample)  
-   [Comentário](#Comment)  
-   [Filtragem](#Filtering)  
-   [Cabeçalho](#Header)  
-   [Pedido de HTTP](#HTTPRequest)  
-   [Resposta de HTTP](#HTTPResponse)  
-   [Problema](#Issue)  
-   [Operação](#Operation)  
-   [Menu de operação](#Menu)  
-   [Item de menu de operação](#MenuItem)  
-   [Paginação](#Paging)  
-   [Parâmetro](#Parameter)  
-   [Produto](#Product)  
-   [Fornecedor](#Provider)  
-   [Representação](#Representation)  
-   [Subscrição](#Subscription)  
-   [Resumo da subscrição](#SubscriptionSummary)  
-   [Informações da conta de utilizador](#UserAccountInfo)  
-   [Sessão do utilizador](#UseSignIn)  
-   [Inscrição do utilizador](#UserSignUp)  
  
##  <a name="API"></a> API  
 O `API` entidade tem as seguintes propriedades:  
  
|Propriedade|Tipo|Descrição|  
|--------------|----------|-----------------|  
|ID|cadeia|Identificador de recurso. Identifica exclusivamente a API dentro da instância de serviço de gestão de API atual. O valor é um URL relativo válido no formato `apis/{id}` onde `{id}` é um identificador de API. Esta propriedade é só de leitura.|  
|nome|cadeia|Nome da API. Não pode estar vazio. Comprimento máximo é 100 carateres.|  
|descrição|cadeia|Descrição da API. Não pode estar vazio. Pode incluir a formatação de etiquetas HTML. Comprimento máximo é de 1000 carateres.|  
|serviceUrl|cadeia|URL absoluto do serviço de back-end que implementa esta API.|  
|caminho|cadeia|URL relativo, identificando exclusivamente esta API e todos os seus caminhos de recursos dentro da instância de serviço de gestão de API. Ele é acrescentado para o ponto final base URL da API especificado durante a criação de instância de serviço para formar um URL público para esta API.|  
|Protocolos|matriz de número|Descreve em quais protocolos as operações nessa API podem ser invocadas. Valores permitidos são `1 - http` e `2 - https`, ou ambos.|  
|authenticationSettings|[Definições de autenticação de servidor de autorização](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#AuthenticationSettings)|Coleção de definições de autenticação incluído nesta API.|  
|subscriptionKeyParameterNames|objeto|Propriedade opcional que pode ser utilizada para especificar os nomes personalizados para os parâmetros de consulta e/ou de cabeçalho que contém a chave de subscrição. Quando esta propriedade estiver presente, tem de conter, pelo menos, uma das duas propriedades seguintes.<br /><br /> `{   "subscriptionKeyParameterNames":   {     "query": “customQueryParameterName",     "header": “customHeaderParameterName"   } }`|  
  
##  <a name="APISummary"></a> Resumo de API  
 O `API summary` entidade tem as seguintes propriedades:  
  
|Propriedade|Tipo|Descrição|  
|--------------|----------|-----------------|  
|ID|cadeia|Identificador de recurso. Identifica exclusivamente a API dentro da instância de serviço de gestão de API atual. O valor é um URL relativo válido no formato `apis/{id}` onde `{id}` é um identificador de API. Esta propriedade é só de leitura.|  
|nome|cadeia|Nome da API. Não pode estar vazio. Comprimento máximo é 100 carateres.|  
|descrição|cadeia|Descrição da API. Não pode estar vazio. Pode incluir a formatação de etiquetas HTML. Comprimento máximo é de 1000 carateres.|  
  
##  <a name="Application"></a> Aplicação  
 O `application` entidade tem as seguintes propriedades:  
  
|Propriedade|Tipo|Descrição|  
|--------------|----------|-----------------|  
|Id|cadeia|O identificador exclusivo da aplicação.|  
|Cargo|cadeia|O título do aplicativo.|  
|Descrição|cadeia|A descrição da aplicação.|  
|Url|URI|O URI para a aplicação.|  
|Versão|cadeia|Informações de versão para a aplicação.|  
|Requisitos|cadeia|Uma descrição dos requisitos para a aplicação.|  
|Estado|número|O estado atual do aplicativo.<br /><br /> -0 - registado<br /><br /> -1 - submetido<br /><br /> -2 - publicado<br /><br /> -3 - rejeitado<br /><br /> -4 - não publicado|  
|RegistrationDate|DateTime|A data e hora, que a aplicação foi registrada.|  
|CategoryId|número|A categoria da aplicação (Finanças, entretenimento, etc.)|  
|DeveloperId|cadeia|O identificador exclusivo do desenvolvedor que submeteu a aplicação.|  
|Anexos|Coleção de [anexo](#Attachment) entidades.|Os anexos para a aplicação, tais como capturas de ecrã ou ícones.|  
|Ícone|[Anexo](#Attachment)|O ícone da para a aplicação.|  
  
##  <a name="Attachment"></a> Anexo  
 O `attachment` entidade tem as seguintes propriedades:  
  
|Propriedade|Tipo|Descrição|  
|--------------|----------|-----------------|  
|ID exclusiva|cadeia|O identificador exclusivo para o anexo.|  
|Url|cadeia|O URL do recurso.|  
|Tipo|cadeia|O tipo de anexo.|  
|ContentType|cadeia|O tipo de suporte do anexo.|  
  
##  <a name="Sample"></a> Exemplo de código  
  
|Propriedade|Tipo|Descrição|  
|--------------|----------|-----------------|  
|título|cadeia|O nome da operação.|  
|Trecho de código|cadeia|Esta propriedade foi preterida e não deve ser utilizada.|  
|Pincel|cadeia|Qual código cores da sintaxe modelo a utilizar quando se apresenta o código de exemplo. Valores permitidos são `plain`, `php`, `java`, `xml`, `objc`, `python`, `ruby`, e `csharp`.|  
|modelo|cadeia|O nome deste modelo de exemplo de código.|  
|corpo|cadeia|Um marcador de posição para a parte do exemplo de código do trecho.|  
|método|cadeia|O método HTTP da operação.|  
|Esquema|cadeia|O protocolo a utilizar para o pedido de operação.|  
|caminho|cadeia|O caminho da operação.|  
|consulta|cadeia|Exemplo de cadeia de caracteres de consulta com parâmetros definidos.|  
|anfitrião|cadeia|O URL do gateway de serviço de gestão de API para a API que contém esta operação.|  
|Cabeçalhos|Coleção de [cabeçalho](#Header) entidades.|Cabeçalhos para esta operação.|  
|parâmetros|Coleção de [parâmetro](#Parameter) entidades.|Parâmetros que são definidos para esta operação.|  
  
##  <a name="Comment"></a> Comentário  
 O `API` entidade tem as seguintes propriedades:  
  
|Propriedade|Tipo|Descrição|  
|--------------|----------|-----------------|  
|Id|número|O id do comentário.|  
|CommentText|cadeia|O corpo do comentário. Pode incluir HTML.|  
|DeveloperCompany|cadeia|O nome da empresa do desenvolvedor.|  
|PostedOn|DateTime|A data e hora, que o comentário foi postado.|  
  
##  <a name="Issue"></a> Problema  
 O `issue` entidade tem as seguintes propriedades.  
  
|Propriedade|Tipo|Descrição|  
|--------------|----------|-----------------|  
|Id|cadeia|O identificador exclusivo para o problema.|  
|ApiID|cadeia|O id para a API para o qual este problema foi relatado.|  
|Cargo|cadeia|Título do problema.|  
|Descrição|cadeia|Descrição do problema.|  
|SubscriptionDeveloperName|cadeia|Nome próprio do desenvolvedor que comunicou o problema.|  
|IssueState|cadeia|O estado atual do problema. Valores possíveis são proposta, aberto, fechado.|  
|ReportedOn|DateTime|A data e hora, que o problema foi relatado.|  
|Comentários|Coleção de [comentário](#Comment) entidades.|Comentários sobre este problema.|  
|Anexos|Coleção de [anexo](api-management-template-data-model-reference.md#Attachment) entidades.|Os anexos para o problema.|  
|Serviços|Coleção de [API](#API) entidades.|As APIs subscrito pelo utilizador que arquivou o problema.|  
  
##  <a name="Filtering"></a> Filtragem  
 O `filtering` entidade tem as seguintes propriedades:  
  
|Propriedade|Tipo|Descrição|  
|--------------|----------|-----------------|  
|Padrão|cadeia|O termo de pesquisa atual; ou `null` se não houver nenhum termo de pesquisa.|  
|Marcador de posição|cadeia|O texto a apresentar na caixa de pesquisa quando não existe nenhum termo de pesquisa especificado.|  
  
##  <a name="Header"></a> Cabeçalho  
 Esta secção descreve o `parameter` representação.  
  
|Propriedade|Descrição|Tipo|  
|--------------|-----------------|----------|  
|nome|cadeia|Nome do parâmetro.|  
|descrição|cadeia|Descrição do parâmetro.|  
|valor|cadeia|Valor do cabeçalho.|  
|typeName|cadeia|Tipo de dados do valor do cabeçalho.|  
|opções|cadeia|Opções.|  
|obrigatório|boolean|Se o cabeçalho é obrigatório.|  
|readOnly|boolean|Se o cabeçalho é só de leitura.|  
  
##  <a name="HTTPRequest"></a> Pedido de HTTP  
 Esta secção descreve o `request` representação.  
  
|Propriedade|Tipo|Descrição|  
|--------------|----------|-----------------|  
|descrição|cadeia|Descrição do pedido de operação.|  
|Cabeçalhos|matriz de [cabeçalho](#Header) entidades.|Cabeçalhos de pedido.|  
|parâmetros|matriz de [parâmetro](#Parameter)|Coleção de parâmetros do pedido de operação.|  
|representações|matriz de [representação](#Representation)|Coleção de representações de pedido de operação.|  
  
##  <a name="HTTPResponse"></a> Resposta de HTTP  
 Esta secção descreve o `response` representação.  
  
|Propriedade|Tipo|Descrição|  
|--------------|----------|-----------------|  
|statusCode|número inteiro positivo|Código de estado de resposta de operação.|  
|descrição|cadeia|Descrição de resposta de operação.|  
|representações|matriz de [representação](#Representation)|Coleção de representações de resposta de operação.|  
  
##  <a name="Operation"></a> Operação  
 O `operation` entidade tem as seguintes propriedades:  
  
|Propriedade|Tipo|Descrição|  
|--------------|----------|-----------------|  
|ID|cadeia|Identificador de recurso. Identifica exclusivamente a operação dentro da instância de serviço de gestão de API atual. O valor é um URL relativo válido no formato `apis/{aid}/operations/{id}` em que `{aid}` é um identificador de API e `{id}` é um identificador de operação. Esta propriedade é só de leitura.|  
|nome|cadeia|Nome da operação. Não pode estar vazio. Comprimento máximo é 100 carateres.|  
|descrição|cadeia|Descrição da operação. Não pode estar vazio. Pode incluir a formatação de etiquetas HTML. Comprimento máximo é de 1000 carateres.|  
|Esquema|cadeia|Descreve em quais protocolos as operações nessa API podem ser invocadas. Valores permitidos são `http`, `https`, ou ambos `http` e `https`.|  
|uriTemplate|cadeia|Modelo de URL relativo identificar o recurso de destino para esta operação. Pode incluir parâmetros. Exemplo: `customers/{cid}/orders/{oid}/?date={date}`|  
|anfitrião|cadeia|O URL do gateway de gestão de API que aloja a API.|  
|HttpMethod|cadeia|Método de operação HTTP.|  
|pedido|[Pedido de HTTP](#HTTPRequest)|Uma entidade que contém detalhes do pedido.|  
|Respostas|matriz de [resposta de HTTP](#HTTPResponse)|Matriz de operação [resposta HTTP](#HTTPResponse) entidades.|  
  
##  <a name="Menu"></a> Menu de operação  
 O `operation menu` entidade tem as seguintes propriedades:  
  
|Propriedade|Tipo|Descrição|  
|--------------|----------|-----------------|  
|apiId|cadeia|O id da API atual.|  
|CurrentOperationId|cadeia|O id da operação atual.|  
|Ação|cadeia|O tipo de menu.|  
|MenuItems|Coleção de [item de menu de operação](#MenuItem) entidades.|As operações para a API atual.|  
  
##  <a name="MenuItem"></a> Item de menu de operação  
 O `operation menu item` entidade tem as seguintes propriedades:  
  
|Propriedade|Tipo|Descrição|  
|--------------|----------|-----------------|  
|Id|cadeia|O id da operação.|  
|Cargo|cadeia|A descrição da operação.|  
|HttpMethod|cadeia|O método Http da operação.|  
  
##  <a name="Paging"></a> Paginação  
 O `paging` entidade tem as seguintes propriedades:  
  
|Propriedade|Tipo|Descrição|  
|--------------|----------|-----------------|  
|Página|número|O número de página atual.|  
|PageSize|número|O máximo de resultados a serem exibidos numa única página.|  
|TotalItemCount|número|O número de itens para exibição.|  
|Mostrartudo|boolean|Se deve ser Mostr tudo resulta numa única página.|  
|PageCount|número|O número de páginas de resultados.|  
  
##  <a name="Parameter"></a> Parâmetro  
 Esta secção descreve o `parameter` representação.  
  
|Propriedade|Descrição|Tipo|  
|--------------|-----------------|----------|  
|nome|cadeia|Nome do parâmetro.|  
|descrição|cadeia|Descrição do parâmetro.|  
|valor|cadeia|Valor do parâmetro.|  
|opções|matriz da cadeia|Valores definidos para valores de parâmetro de consulta.|  
|obrigatório|boolean|Especifica se o parâmetro é necessário ou não.|  
|tipo|número|Se este parâmetro é um parâmetro de caminho (1) ou um parâmetro de cadeia de consulta (2).|  
|typeName|cadeia|Tipo de parâmetro.|  
  
##  <a name="Product"></a> Produto  
 O `product` entidade tem as seguintes propriedades:  
  
|Propriedade|Tipo|Descrição|  
|--------------|----------|-----------------|  
|Id|cadeia|Identificador de recurso. Identifica exclusivamente o produto dentro da instância de serviço de gestão de API atual. O valor é um URL relativo válido no formato `products/{pid}` onde `{pid}` é um identificador de produto. Esta propriedade é só de leitura.|  
|Cargo|cadeia|Nome do produto. Não pode estar vazio. Comprimento máximo é 100 carateres.|  
|Descrição|cadeia|Descrição do produto. Não pode estar vazio. Pode incluir a formatação de etiquetas HTML. Comprimento máximo é de 1000 carateres.|  
|Termos|cadeia|Termos de produto de utilização. Os desenvolvedores tentando subscrever o produto serão apresentados e de aceitar estes termos, para que poderem concluir o processo de subscrição.|  
|ProductState|número|Especifica se o produto é publicado ou não. Produtos publicados podem ser detetados pelos programadores no portal do programador. Publicado por não os produtos são visíveis apenas para administradores.<br /><br /> Os valores permitidos para o estado de produto são:<br /><br /> - `0 - Not Published`<br /><br /> - `1 - Published`<br /><br /> - `2 - Deleted`|  
|AllowMultipleSubscriptions|boolean|Especifica se um utilizador pode ter várias subscrições neste produto ao mesmo tempo.|  
|MultipleSubscriptionsCount|número|Número máximo de subscrições para este produto um utilizador pode ter ao mesmo tempo.|  
  
##  <a name="Provider"></a> Fornecedor  
 O `provider` entidade tem as seguintes propriedades:  
  
|Propriedade|Tipo|Descrição|  
|--------------|----------|-----------------|  
|Propriedades|dicionário da cadeia de caracteres|Propriedades para este fornecedor de autenticação.|  
|AuthenticationType|cadeia|O tipo de fornecedor. (O azure Active Directory, início de sessão do Facebook, conta do Google, Microsoft Account, Twitter).|  
|Legenda|cadeia|Nome a apresentar do fornecedor.|  
  
##  <a name="Representation"></a> Representação  
 Esta secção descreve uma `representation`.  
  
|Propriedade|Tipo|Descrição|  
|--------------|----------|-----------------|  
|contentType|cadeia|Especifica um tipo de conteúdo personalizado ou registado para este representação, por exemplo, `application/xml`.|  
|Exemplo|cadeia|Um exemplo da representação.|  
  
##  <a name="Subscription"></a> Subscrição  
 O `subscription` entidade tem as seguintes propriedades:  
  
|Propriedade|Tipo|Descrição|  
|--------------|----------|-----------------|  
|Id|cadeia|Identificador de recurso. Identifica de forma exclusiva a subscrição dentro da instância de serviço de gestão de API atual. O valor é um URL relativo válido no formato `subscriptions/{sid}` onde `{sid}` é um identificador de subscrição. Esta propriedade é só de leitura.|  
|Id do Produto|cadeia|O identificador de recurso de produto do produto subscrito. O valor é um URL relativo válido no formato `products/{pid}` onde `{pid}` é um identificador de produto.|  
|ProductTitle|cadeia|Nome do produto. Não pode estar vazio. Comprimento máximo é 100 carateres.|  
|ProductDescription|cadeia|Descrição do produto. Não pode estar vazio. Pode incluir a formatação de etiquetas HTML. Comprimento máximo é de 1000 carateres.|  
|ProductDetailsUrl|cadeia|URL relativa para os detalhes do produto.|  
|state|cadeia|O estado da subscrição. Estados possíveis são:<br /><br /> - `0 - suspended` – a subscrição está bloqueada e o subscritor não é possível chamar qualquer API do produto.<br /><br /> - `1 - active` – a subscrição está ativa.<br /><br /> - `2 - expired` – a subscrição atingiu a respetiva data de expiração e foi desativada.<br /><br /> - `3 - submitted` – o pedido de subscrição foi efetuado pelo desenvolvedor, mas ainda não foi aprovado ou rejeitado.<br /><br /> - `4 - rejected` – o pedido de subscrição foi negado por um administrador.<br /><br /> - `5 - cancelled` – a subscrição foi cancelada pelo desenvolvedor ou administrador.|  
|Nome a Apresentar|cadeia|Nome a apresentar da subscrição.|  
|Colunas CreatedDate|DateTime|A data a subscrição foi criada, no formato ISO 8601: `2014-06-24T16:25:00Z`.|  
|CanBeCancelled|boolean|Se a subscrição pode ser cancelada pelo utilizador atual.|  
|IsAwaitingApproval|boolean|Se a subscrição está aguardando aprovação.|  
|startDate|DateTime|A data de início para a subscrição, no formato ISO 8601: `2014-06-24T16:25:00Z`.|  
|ExpirationDate|DateTime|A data de expiração da subscrição, no formato ISO 8601: `2014-06-24T16:25:00Z`.|  
|NotificationDate|DateTime|A data de notificação para a subscrição, no formato ISO 8601: `2014-06-24T16:25:00Z`.|  
|primaryKey|cadeia|A chave de subscrição primária. Comprimento máximo é de 256 carateres.|  
|Secundária|cadeia|A chave de subscrição secundário. Comprimento máximo é de 256 carateres.|  
|CanBeRenewed|boolean|Se a subscrição pode ser renovada do utilizador atual.|  
|HasExpired|boolean|Se a subscrição expirou.|  
|IsRejected|boolean|Se o pedido de subscrição foi negado.|  
|CancelUrl|cadeia|A Url relativa para cancelar a subscrição.|  
|RenewUrl|cadeia|A Url relativa para renovar a subscrição.|  
  
##  <a name="SubscriptionSummary"></a> Resumo da subscrição  
 O `subscription summary` entidade tem as seguintes propriedades:  
  
|Propriedade|Tipo|Descrição|  
|--------------|----------|-----------------|  
|Id|cadeia|Identificador de recurso. Identifica de forma exclusiva a subscrição dentro da instância de serviço de gestão de API atual. O valor é um URL relativo válido no formato `subscriptions/{sid}` onde `{sid}` é um identificador de subscrição. Esta propriedade é só de leitura.|  
|Nome a Apresentar|cadeia|O nome a apresentar da subscrição|  
  
##  <a name="UserAccountInfo"></a> Informações da conta de utilizador  
 O `user account info` entidade tem as seguintes propriedades:  
  
|Propriedade|Tipo|Descrição|  
|--------------|----------|-----------------|  
|FirstName|cadeia|Nome próprio. Não pode estar vazio. Comprimento máximo é 100 carateres.|  
|LastName|cadeia|Apelido. Não pode estar vazio. Comprimento máximo é 100 carateres.|  
|Email|cadeia|Endereço de e-mail. Não pode estar vazio e tem de ser exclusivo dentro da instância de serviço. Comprimento máximo é 254 carateres.|  
|Palavra-passe|cadeia|Palavra-passe da conta de utilizador.|  
|NameIdentifier|cadeia|Identificador de conta, o mesmo que a mensagem de e-mail do utilizador.|  
|ProviderName|cadeia|Nome do fornecedor de autenticação.|  
|IsBasicAccount|boolean|TRUE se esta conta foi registrada com o e-mail e palavra-passe; FALSO se a conta foi registada através de um fornecedor.|  
  
##  <a name="UseSignIn"></a> Início de sessão de utilizador  
 O `user sign in` entidade tem as seguintes propriedades:  
  
|Propriedade|Tipo|Descrição|  
|--------------|----------|-----------------|  
|Email|cadeia|Endereço de e-mail. Não pode estar vazio e tem de ser exclusivo dentro da instância de serviço. Comprimento máximo é 254 carateres.|  
|Palavra-passe|cadeia|Palavra-passe da conta de utilizador.|  
|ReturnUrl|cadeia|O URL da página em que o usuário clicou iniciar sessão.|  
|RememberMe|boolean|Se guardar as informações do utilizador atual.|  
|RegistrationEnabled|boolean|Se o registo está ativado.|  
|DelegationEnabled|boolean|Se o início de sessão delegado está ativado.|  
|DelegationUrl|cadeia|O delegado de sessão no url, se estiver ativada.|  
|SsoSignUpUrl|cadeia|O início de sessão único na URL para o utilizador, se estiver presente.|  
|AuxServiceUrl|cadeia|Se o usuário atual for um administrador, esta é uma ligação para a instância de serviço no portal do Azure.|  
|Fornecedores|Coleção de [fornecedor](#Provider) entidades|Os fornecedores de autenticação para este utilizador.|  
|UserRegistrationTerms|cadeia|Termos de um utilizador tem de aceitar antes de iniciar sessão.|  
|UserRegistrationTermsEnabled|boolean|Se os termos estão ativados.|  
  
##  <a name="UserSignUp"></a> Inscrição de utilizador  
 O `user sign up` entidade tem as seguintes propriedades:  
  
|Propriedade|Tipo|Descrição|  
|--------------|----------|-----------------|  
|PasswordConfirm|boolean|Valor usado pelos [Inscreva-se](api-management-page-controls.md#sign-up)controle Inscreva-se.|  
|Palavra-passe|cadeia|Palavra-passe da conta de utilizador.|  
|PasswordVerdictLevel|número|Valor usado pelos [Inscreva-se](api-management-page-controls.md#sign-up)controle Inscreva-se.|  
|UserRegistrationTerms|cadeia|Termos de um utilizador tem de aceitar antes de iniciar sessão.|  
|UserRegistrationTermsOptions|número|Valor usado pelos [Inscreva-se](api-management-page-controls.md#sign-up)controle Inscreva-se.|  
|ConsentAccepted|boolean|Valor usado pelos [Inscreva-se](api-management-page-controls.md#sign-up)controle Inscreva-se.|  
|Email|cadeia|Endereço de e-mail. Não pode estar vazio e tem de ser exclusivo dentro da instância de serviço. Comprimento máximo é 254 carateres.|  
|FirstName|cadeia|Nome próprio. Não pode estar vazio. Comprimento máximo é 100 carateres.|  
|LastName|cadeia|Apelido. Não pode estar vazio. Comprimento máximo é 100 carateres.|  
|UserData|cadeia|Valor usado pelos [Inscreva-se](api-management-page-controls.md#sign-up) controle.|  
|NameIdentifier|cadeia|Valor usado pelos [Inscreva-se](api-management-page-controls.md#sign-up)controle Inscreva-se.|  
|ProviderName|cadeia|Nome do fornecedor de autenticação.|

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre como trabalhar com modelos, consulte [como personalizar o portal do Programador de gestão de API através de modelos](api-management-developer-portal-templates.md).
