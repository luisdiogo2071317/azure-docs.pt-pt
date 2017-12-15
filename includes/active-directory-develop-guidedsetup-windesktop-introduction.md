# <a name="call-the-microsoft-graph-api-from-a-windows-desktop-app"></a>Chamar o Microsoft Graph API a partir de uma aplicação de ambiente de trabalho do Windows

Este guia demonstra como uma aplicação .NET de ambiente de trabalho do Windows (XAML) nativo pode obter um token de acesso e chamar outras APIs que necessitam de tokens de acesso a partir de um ponto de final do Azure Active Directory v2 ou o Microsoft Graph API.

Quando concluiu o guia, a aplicação será possível chamar uma API protegida que utilize contas pessoais (incluindo outlook.com, live.com e outros). A aplicação será também utilizar o trabalho contas escolares ou profissionais de qualquer da empresa ou organização que utiliza o Azure Active Directory.  

> [!NOTE] 
> O guia requer o Visual Studio 2015 Update 3 ou Visual Studio 2017.  Não tem qualquer uma destas versões? [Transferir o Visual Studio 2017 gratuitamente](https://www.visualstudio.com/downloads/).

## <a name="how-this-guide-works"></a>Como funciona este guia

![Como funciona este guia](./media/active-directory-develop-guidedsetup-windesktop-intro/windesktophowitworks.png)

A aplicação de exemplo que criar deste guia permite uma aplicação de ambiente de trabalho do Windows que consulta a Graph API do Microsoft ou uma API Web que aceita tokens a partir de um ponto de final do Azure Active Directory v2. Para este cenário, é possível adicionar um token a pedidos de HTTP através do cabeçalho de autorização. Biblioteca de autenticação da Microsoft (MSAL) processa a aquisição do token e a renovação.

## <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Processamento de aquisição de token para aceder ao protegidos APIs da Web

Depois do utilizador é autenticado, a aplicação de exemplo recebe um token que pode ser utilizado para consultar o Microsoft Graph API ou uma API Web que está protegida pelo Azure Active Directory v2.

APIs, tais como o Microsoft Graph requerem um token para permitir o acesso a recursos específicos. Por exemplo, um token é necessário um perfil de utilizador de leitura, aceder ao calendário de um utilizador ou enviar correio eletrónico. A aplicação pode pedir um token de acesso utilizando MSAL para aceder a estes recursos, especificando os âmbitos de API. Este token de acesso é então adicionada ao cabeçalho de autorização de HTTP para cada chamada que é efetuada em relação ao recurso protegido. 

MSAL gere a colocação em cache e atualizar os tokens de acesso para si, para que a aplicação não tem.

## <a name="nuget-packages"></a>Pacotes NuGet

Este guia utiliza os seguintes pacotes de NuGet:

|Biblioteca|Descrição|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Biblioteca de autenticação da Microsoft (MSAL)|

