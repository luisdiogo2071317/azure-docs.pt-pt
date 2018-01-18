
# <a name="call-the-microsoft-graph-api-from-an-android-app"></a>Chamar o Microsoft Graph API a partir de uma aplicação Android

Este guia demonstra como uma aplicação Android nativa pode obter um token de acesso e chamar outras APIs que necessitam de tokens de acesso do ponto final v2 do Azure Active Directory ou o Microsoft Graph API.

Quando concluiu o guia, a aplicação será capaz de aceitar inícios de sessão de contas pessoais (incluindo outlook.com, live.com e outros) e contas profissional e escolar de qualquer da empresa ou organização que utiliza o Azure Active Directory. A aplicação, em seguida, irá chamar uma API que está protegida pelo ponto final do Azure Active Directory v2.  

## <a name="how-this-sample-works"></a>Como funciona este exemplo
![Como funciona este exemplo](media/active-directory-develop-guidedsetup-android-intro/android-intro.png)

A aplicação de exemplo que criar com este guia baseia-se um cenário onde uma aplicação Android é utilizada para consultar uma API Web que aceita tokens de ponto final v2 do Azure Active Directory (Microsoft Graph API, neste caso). Para este cenário, a aplicação adiciona o token adquirido a pedidos de HTTP através do cabeçalho de autorização. A biblioteca de autenticação da Microsoft (MSAL) processa a aquisição do token e a renovação por si.

## <a name="prerequisites"></a>Pré-requisitos
* Esta configuração orientado concentra-se no Android Studio, mas qualquer outro ambiente de desenvolvimento de uma aplicação Android também é aceitável. 
* É necessário o Android SDK 21 ou posterior (recomenda-se SDK 25).
* Google Chrome ou um browser que utiliza separadores personalizada é necessário para esta versão do MSAL para Android.

> [!NOTE]
> Google Chrome não está incluído no Visual Studio emulador para Android. Recomendamos que teste este código um emulador com API 25 ou uma imagem com API 21 ou mais recente que tenha instalado do Google Chrome.

## <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Processamento de aquisição de token para aceder ao protegidos APIs da Web

Depois do utilizador é autenticado, a aplicação de exemplo recebe um token de acesso que pode ser utilizado para consultar o Microsoft Graph API ou uma API Web que está protegida pelo Azure Active Directory v2.

APIs, tais como o Microsoft Graph requerem um token de acesso para permitir o acesso a recursos específicos. Por exemplo, um token de acesso é necessário um perfil de utilizador de leitura, aceder ao calendário de um utilizador ou enviar correio eletrónico. A aplicação pode pedir um token de acesso utilizando MSAL para aceder a estes recursos, especificando os âmbitos de API. Este token de acesso é então adicionada ao cabeçalho de autorização de HTTP para cada chamada que é efetuada em relação ao recurso protegido. 

MSAL gere a colocação em cache e atualizar os tokens de acesso para si, para que a aplicação não tem.

## <a name="libraries"></a>Bibliotecas

Este guia utiliza as bibliotecas seguintes:

|Biblioteca|Descrição|
|---|---|
|[com.microsoft.identity.client](http://javadoc.io/doc/com.microsoft.identity.client/msal)|Biblioteca de autenticação da Microsoft (MSAL)|
