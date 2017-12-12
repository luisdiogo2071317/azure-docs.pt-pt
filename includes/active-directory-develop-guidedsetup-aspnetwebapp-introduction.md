# <a name="add-sign-in-with-microsoft-to-an-aspnet-web-app"></a>Adicionar início de sessão com a Microsoft para uma aplicação web ASP.NET

Este guia demonstra como implementar o início de sessão com a Microsoft através de uma solução de ASP.NET MVC com uma aplicação de baseada no browser tradicional web com OpenID Connect. 

No final deste guia, a aplicação será capaz de aceitar o início de sessão ins do pessoal contas (incluindo outlook.com, live.com e outros), bem como funcionam contas escolares ou profissionais de qualquer da empresa ou organização que tem integrado com o Azure Active Directory. 

> Este guia requer o Visual Studio 2015 Update 3 ou Visual Studio 2017.  Não o tiver?  [Transferir o Visual Studio 2017 gratuitamente](https://www.visualstudio.com/downloads/)

## <a name="how-this-guide-works"></a>Como funciona este guia

![Como funciona este guia](media/active-directory-develop-guidedsetup-aspnetwebapp-intro/aspnetbrowsergeneral.png)

Este guia baseia-se no cenário onde um browser acede a um web site do ASP.NET, pedir um utilizador para autenticar através de um botão de início de sessão. Neste cenário, a maioria do trabalho para compor a página web ocorre no lado do servidor.

## <a name="libraries"></a>Bibliotecas

Este guia utiliza as bibliotecas seguintes:

|Biblioteca|Descrição|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|Middleware ativa uma aplicação a utilizar OpenIdConnect para autenticação|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|Middleware ativa uma aplicação manter a utilização de cookies de sessão de utilizador|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|Permite que as aplicações com base em OWIN são executadas no IIS através do pipeline de pedido do ASP.NET|

