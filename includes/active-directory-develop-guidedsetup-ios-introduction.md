
# <a name="call-the-microsoft-graph-api-from-an-ios-application"></a>Chamar o Microsoft Graph API a partir de uma aplicação iOS

Este guia mostra como uma aplicação nativa do iOS (Swift) pode chamar as APIs que necessitam de tokens de acesso do ponto final v 2.0 do Microsoft Azure Active Directory (Azure AD). O guia explica como obter os tokens de acesso e utilizá-los em chamadas para o Microsoft Graph API e outras APIs.

Depois de concluir os exercícios neste guia, a aplicação pode chamar uma API protegida a partir de qualquer da empresa ou organização que tenha o Azure AD. A aplicação pode efetuar chamadas de API protegidas através da utilização de contas pessoais, como o outlook.com, live.com e outros, bem como contas profissionais ou escolares.

## <a name="prerequisites"></a>Pré-requisitos
- XCode versão 8. x não é necessária para o exemplo que é criado neste guia. Pode transferir o XCode do [iTunes site](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode transferir URL").
- O [Carthage](https://github.com/Carthage/Carthage) Gestor de dependência é necessário para a gestão de pacotes.

## <a name="how-this-guide-works"></a>Como funciona este guia

![Como funciona este guia](media/active-directory-develop-guidedsetup-ios-introduction/iosintro.png)

Neste guia, a aplicação de exemplo permite que uma aplicação iOS consultar a Graph API do Microsoft ou uma API web que aceita tokens do ponto final v 2.0 do Azure AD. Neste cenário, um token é adicionado a pedidos de HTTP utilizando o **autorização** cabeçalho. Aquisição do token e renovação são processadas pela biblioteca de autenticação do Microsoft (MSAL).


### <a name="handle-token-acquisition-for-access-to-protected-web-apis"></a>Identificador de aquisição de token de acesso a APIs da web protegidas

Depois do utilizador é autenticado, a aplicação de exemplo recebe um token. O token é utilizado para consultar a Graph API do Microsoft ou uma API web que está protegida pelo ponto final v 2.0 do Azure AD.

APIs, tais como o Microsoft Graph, necessitam de um token de acesso para permitir o acesso a recursos específicos. Tokens são necessários para ler um perfil de utilizador, aceder calendário de um utilizador, envie um e-mail e assim sucessivamente. A aplicação pode pedir um token de acesso utilizando MSAL e especificar âmbitos de API. O token de acesso é adicionado ao HTTP **autorização** cabeçalho para cada chamada que é efetuada em relação ao recurso protegido.

MSAL gere a colocação em cache e atualizar os tokens de acesso para si, pelo que não tem da aplicação.


## <a name="libraries"></a>Bibliotecas

Este guia utiliza a biblioteca seguinte:

|Biblioteca|Descrição|
|---|---|
|[MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)|Pré-visualização biblioteca de autenticação de Microsoft para iOS|

