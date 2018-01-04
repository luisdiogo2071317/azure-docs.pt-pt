---
title: "Servidor Web do Azure AD v1 ASP.NET introdução | Microsoft Docs"
description: "Implementar Microsoft início de sessão numa solução ASP.NET com uma aplicação de baseadas no browser web tradicional utilizando o padrão de OpenID Connect"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/08/2017
ms.author: andret
ms.openlocfilehash: b23afd26f7ac1828381a0410d2455206c8f43c88
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/03/2018
---
<!--start-intro-->
# <a name="add-sign-in-with-microsoft-to-an-aspnet-web-app"></a>Adicionar início de sessão com a Microsoft para uma aplicação web ASP.NET

Este guia demonstra como implementar o início de sessão com a Microsoft através de uma solução de ASP.NET MVC com uma aplicação de baseada no browser tradicional web com OpenID Connect. 

No final deste guia, a aplicação irá aceitar inícios de sessão das contas profissionais e escolares de organizações que tenham integrado com o Azure Active Directory.

> [!NOTE]
> Esta configuração orientada ajuda-o para permitir inícios de sessão das contas profissionais e escolares na sua aplicação ASP.NET. Se estiver interessado permitir inícios de sessão para contas pessoais, para além das contas profissional e escolar, pode utilizar o [v2 endpoint](../active-directory-v2-compare.md). Consulte [este ASP.NET orientado programa de configuração para o ponto final v2](./active-directory-aspnetwebapp.md) , bem como [neste documento](../active-directory-v2-limitations.md) explicar as limitações atuais do ponto final v2.
<br/><br/>

<!--separator-->

> Este guia requer o Visual Studio 2015 Update 3 ou Visual Studio 2017.  Não o tiver?  [Transferir o Visual Studio 2017 gratuitamente](https://www.visualstudio.com/downloads/)

## <a name="how-this-guide-works"></a>Como funciona este guia

![Como funciona este guia](media/active-directory-aspnetwebapp-v1/aspnet-intro.png)

Este guia baseia-se no cenário onde um browser acede a um web site do ASP.NET, pedir um utilizador para autenticar através de um botão de início de sessão. Neste cenário, a maioria do trabalho para compor a página web ocorre no lado do servidor.

> [!NOTE]
> Esta configuração orientada demonstra como os utilizadores sobre uma aplicação Web de ASP.NET a partir de um modelo em branco a iniciar sessão e incluir passos como adicionar um início de sessão no botão e cada controlador e métodos, enquanto também explicar alguns conceitos. Em alternativa, também pode criar um projeto de início de sessão do Azure Active Directory utilizadores (contas profissionais e escolares) utilizando o [modelo de web do Visual Studio](https://docs.microsoft.com/aspnet/visual-studio/overview/2013/creating-web-projects-in-visual-studio#organizational-account-authentication-options) e selecionando *contas institucionais* e, em seguida, uma das opções em nuvem - esta opção utiliza um modelo mais rico, com vistas, métodos e controladores adicionais.

## <a name="libraries"></a>Bibliotecas

Este guia utiliza os seguintes pacotes:

|Biblioteca|Descrição|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|Middleware ativa uma aplicação a utilizar OpenIdConnect para autenticação|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|Middleware ativa uma aplicação manter a utilização de cookies de sessão de utilizador|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|Permite que as aplicações com base em OWIN são executadas no IIS através do pipeline de pedido do ASP.NET|


<!--end-intro-->

<!--start-setup-->

## <a name="set-up-your-project"></a>Configurar o projeto

Esta secção mostra os passos para instalar e configurar o pipeline de autenticação através da OWIN middleware num projeto ASP.NET com OpenID Connect. 

> Prefere transferir o projeto do Visual Studio este exemplo em vez disso? [Transferir um projeto](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/GuidedSetup.zip) e avance para o [passo da configuração](#configure-your-webconfig-and-register-an-application) para configurar o exemplo de código antes de executar.

## <a name="create-your-aspnet-project"></a>Criar o projeto ASP.NET
1. No Visual Studio:`File` > `New` > `Project`<br/>
2. Em *Visual C# \Web*, selecione `ASP.NET Web Application (.NET Framework)`.
3. Nome da aplicação e clique em *OK*
4. Selecione `Empty` e, em seguida, selecione a caixa de verificação para adicionar `MVC` referências

## <a name="add-authentication-components"></a>Adicionar componentes de autenticação

1. No Visual Studio:`Tools` > `Nuget Package Manager` > `Package Manager Console`
2. Adicionar *pacotes de NuGet OWIN middleware* , escrevendo o seguinte na janela da consola do Gestor de pacotes:

    ```powershell
    Install-Package Microsoft.Owin.Security.OpenIdConnect
    Install-Package Microsoft.Owin.Security.Cookies
    Install-Package Microsoft.Owin.Host.SystemWeb
    ```

<!--start-collapse-->
> ### <a name="about-these-packages"></a>Sobre estes pacotes
>Ative as bibliotecas acima-início de sessão único (SSO) utilizando o OpenID Connect através da autenticação baseada no cookie. Após a autenticação é concluída e o token que representa o utilizador é enviado para a sua aplicação, o OWIN middleware cria um cookie de sessão. O browser, em seguida, utiliza este cookie nos pedidos subsequentes para que o utilizador não tem de voltar e não é necessária nenhuma verificação adicional.
<!--end-collapse-->

## <a name="configure-the-authentication-pipeline"></a>Configurar o pipeline de autenticação
Os seguintes passos são utilizados para criar um middleware OWIN *classe de Startup* para configurar a autenticação OpenID Connect. Esta classe é executada automaticamente.

> [!TIP]
> Se o seu projeto não tem um `Startup.cs` ficheiro na pasta raiz:<br/>
> 1. Clique com o botão direito na pasta de raiz do projeto: >`Add` > `New Item...` > `OWIN Startup class`<br/>
> 2. Nome`Startup.cs`<br/>
>
>> Certifique-se a classe selecionada uma classe de Startup da OWIN e não uma padrão c# classe. Confirmar isto, verificando se vir `[assembly: OwinStartup(typeof({NameSpace}.Startup))]` acima o espaço de nomes.


1. Adicionar *OWIN* e *IdentityModel* espaços de nomes ao `Startup.cs`:

    [!code-csharp[main](../../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet\Startup.cs?name=AddedNameSpaces "Startup.cs")]

2. Substitua a classe de Startup com o seguinte código:

    [!code-csharp[main](../../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet\Startup.cs?name=Startup "Startup.cs")]
    
<!--start-collapse-->
> [!NOTE]
> Os parâmetros que fornece *OpenIDConnectAuthenticationOptions* servir como coordenadas para a aplicação comunicar com o Azure AD. Uma vez que o middleware OpenID Connect utiliza cookies, terá também de configurar a autenticação de cookie tal como mostra o código anterior. O *ValidateIssuer* valor indica OpenIdConnect não restringir o acesso a uma organização específica.
<!--end-collapse-->

<!--end-setup-->

<!--start-use-->

## <a name="add-a-controller-to-handle-sign-in-and-sign-out-requests"></a>Adicionar um controlador para processar pedidos de início de sessão e fim de sessão

Este passo mostra como criar um novo controlador para expor os métodos de início de sessão e fim de sessão.

1.  Clique com botão direito do `Controllers` pastas e selecione`Add` > `Controller`
2.  Selecione `MVC (.NET version) Controller – Empty`.
3.  Clique em *adicionar*
4.  Nome `HomeController` e clique em *adicionar*
5.  Adicionar *OWIN* espaços de nomes para a classe:

    [!code-csharp[main](../../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet\Controllers\HomeController.cs?name=AddedNameSpaces "HomeController.cs")]

6. Adicione os seguintes métodos para lidar com início de sessão e fim de sessão para o seu controlador iniciando um desafio de autenticação através de código:

    [!code-csharp[main](../../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet\Controllers\HomeController.cs?name=SigInAndSignOut "HomeController.cs")]
    
## <a name="create-the-apps-home-page-to-sign-in-users-via-a-sign-in-button"></a>Criar a home page a aplicação para iniciar sessão de utilizadores através de um botão de início de sessão

No Visual Studio, crie uma nova vista para adicionar o botão de início de sessão e apresentar informações de utilizador após a autenticação:

1.  Clique com botão direito do `Views\Home` pastas e selecione`Add View`
2.  Dê-lhe o nome `Index`.
3.  Adicione o seguinte HTML, que inclui o botão de início de sessão, para o ficheiro:

    [!code-html[main](../../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet/Views/Home/Index.cshtml "Index.cshtml")]

<!--start-collapse-->
> [!NOTE]
> Esta página adiciona um botão de início de sessão no formato SVG com um fundo preto:<br/>![Início de sessão com a Microsoft](media/active-directory-aspnetwebapp-v1/aspnetsigninbuttonsample.png)<br/> Para obter mais início de sessão botões, aceda a [nesta página](https://docs.microsoft.com/azure/active-directory/develop/active-directory-branding-guidelines "diretrizes de imagem corporativa").
<!--end-collapse-->

## <a name="display-users-claims-by-adding-a-controller"></a>Apresentar as afirmações do utilizador através da adição de um controlador
Este controlador demonstra as utilizações do `[Authorize]` atributo para proteger um controlador. Este atributo restringe o acesso para o controlador, permitindo apenas que os utilizadores autenticados. O seguinte código faz com que utiliza o atributo para apresentar as afirmações de utilizador que foram obtidas como parte de início de sessão.

1.  Clique com botão direito do `Controllers` pasta:`Add` > `Controller`
2.  Selecione `MVC {version} Controller – Empty`.
3.  Clique em *adicionar*
4.  Nome`ClaimsController`
5.  Substitua o código da sua classe de controlador com o seguinte código - esta ação adiciona a `[Authorize]` atributo para a classe:

    [!code-csharp[main](../../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet\Controllers\ClaimsController.cs?name=ClaimsController "ClaimsController.cs")]

<!--start-collapse-->
> [!NOTE]
> Devido à utilização do `[Authorize]` atributo, todos os métodos deste controlador só pode ser executado se o utilizador ser autenticado. Se o utilizador não é autenticado e tenta aceder ao controlador de, OWIN inicia um desafio de autenticação e forçar o utilizador a autenticar. O código acima analisa a coleção de afirmações do utilizador para os atributos específicos incluídos no token do utilizador. Estes atributos incluem o nome do utilizador completo e nome de utilizador, bem como o assunto de identificador do utilizador global. Também contém a *ID do inquilino*, que representa o ID de organização do utilizador. 
<!--end-collapse-->

## <a name="create-a-view-to-display-the-users-claims"></a>Criar uma vista para apresentar afirmações do utilizador

No Visual Studio, crie uma nova vista para apresentar afirmações do utilizador numa página web:

1.  Clique com botão direito do `Views\Claims` pasta e:`Add View`
2.  Dê-lhe o nome `Index`.
3.  Adicione o seguinte HTML para o ficheiro:

    [!code-html[main](../../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet/Views/Claims/Index.cshtml "Index.cshtml")]
    
<!--end-use-->

<!--start-configure-->
## <a name="configure-your-webconfig-and-register-an-application"></a>Configurar o seu *Web. config* e registar uma aplicação

1. No Visual Studio, adicione o seguinte ao `web.config` (localizado na pasta raiz) na secção `configuration\appSettings`:

    ```xml
    <add key="ClientId" value="Enter_the_Application_Id_here" />
    <add key="RedirectUrl" value="Enter_the_Redirect_Url_here" />
    <add key="Tenant" value="common" />
    <add key="Authority" value="https://login.microsoftonline.com/{0}" /> 
    ```
2. No Explorador de soluções, selecione o projeto e observe o <i>propriedades</i> janela (se não for apresentada uma janela de propriedades, prima F4)
3. Alteração SSL ativado para<code>True</code>
4. Copie o URL de SSL do projeto para a área de transferência:<br/><br/>![Propriedades do projeto](media/active-directory-aspnetwebapp-v1/visual-studio-project-properties.png)<br />
5. No <code>web.config</code>, substitua <code>Enter_the_Redirect_URL_here</code> com o URL de SSL do seu projeto 

### <a name="register-your-application-in-the-azure-portal-then-add-its-information-to-webconfig"></a>Registar a sua aplicação no Portal do Azure, em seguida, adicionar as respetivas informações para *Web. config*

1. Vá para o [Portal do Microsoft Azure - registos de aplicação](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) para registar uma aplicação
2. Selecione `New application registration`
3. Introduza um nome para a sua aplicação
4. Cole o projeto de Visual Studio *SSL URL* no `Sign-on URL` (este URL é também adicionado automaticamente à lista de URLs de resposta para a aplicação que está a registar)
5. Clique em `Create` para registar a aplicação. Esta ação leva-o novamente para a lista de aplicações
6. Agora, procurar e/ou selecione a aplicação que acabou de criar para abrir as respetivas propriedades
7. Copiar o guid em `Application ID` para a área de transferência
8. Voltar para o Visual Studio e, em `web.config`, substitua `Enter_the_Application_Id_here` com o ID de aplicação da aplicação que acabou de ser registado

> [!TIP]
> Se a conta está configurada para acesso a vários diretórios, certifique-se de que selecionou o diretório adequado para a organização pretende que a aplicação, registado ao clicar no nome da sua conta na parte superior com o botão direito no Portal do Azure e, em seguida, verificar o diretório selecionado como indicados:<br/>![Selecionar o diretório adequado](media/active-directory-aspnetwebapp-v1/tenantselector.png)

## <a name="configure-sign-in-options"></a>Configurar as opções de início de sessão

Pode configurar a sua aplicação para permitir que apenas os utilizadores que pertençam a instância do Azure Active Directory de uma organização para início de sessão, ou aceite inícios de sessão dos utilizadores que pertencem a qualquer organização. Siga as instruções de uma das seguintes opções:

### <a name="configure-your-application-to-allow-sign-ins-of-work-and-school-accounts-from-any-company-or-organization-multi-tenant"></a>Configurar a sua aplicação para permitir inícios de sessão das contas profissionais e escolares de qualquer empresa ou organização (multi-inquilino)

Siga os passos seguintes se pretender aceitar inícios de sessão das contas profissionais e escolares de qualquer da empresa ou organização que tem integrado com o Azure Active Directory. Este é um cenário comum para *aplicações SaaS*:

1. Volte ao [Portal do Microsoft Azure - registos de aplicação](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) e localize a aplicação que acabou de ser registado
2. Em `All Settings` selecione`Properties`
3. Alteração `Multi-tenanted` propriedade `Yes` e clique em`Save`

Para obter mais informações sobre esta definição e o conceito de aplicações de multi-inquilinos, consulte [neste artigo](../active-directory-devhowto-multi-tenant-overview.md "descrição geral da multi-inquilino").

### <a name="restrict-users-from-only-one-organizations-active-directory-instance-to-sign-in-to-your-application-single-tenant"></a>Restringir os utilizadores da instância do Active Directory da organização apenas um para iniciar sessão sua aplicação (único inquilino)

Esta opção é um cenário comum para *aplicações LOB*: Se pretender que a aplicação para aceitar inícios de sessão apenas a partir de contas que pertençam a uma instância específica do Azure Active Directory (incluindo *as contas de convidados*dessa instância), substitua o `Tenant` parâmetro *Web. config* de `Common` com o nome de inquilino da organização – exemplo, *contoso.onmicrosoft.com*. Depois disso, altere o `ValidateIssuer` argumento na sua [ *classe de Startup da OWIN* ](#configure-the-authentication-pipeline) para `true`.

Para permitir que os utilizadores apenas numa lista de organizações específicas, defina `ValidateIssuer` true e utilizar o `ValidIssuers` parâmetro para especificar uma lista das organizações.

Outra opção consiste em implementar um método personalizado para validar os emissores utilizando *IssuerValidator* parâmetro. Para obter mais informações sobre `TokenValidationParameters`, consulte [neste artigo do MSDN](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx "artigo do TokenValidationParameters MSDN").

<!--end-configure-->

<!--start-configure-arp-->
<!--
## Configure your ASP.NET Web App with the application's registration information

In this step, you will configure your project to use SSL, and then use the SSL URL to configure your application’s registration information. After this, add the application’ registration information to your solution via *web.config*.

1.  In Solution Explorer, select the project and look at the `Properties` window (if you don’t see a Properties window, press F4)
2.  Change `SSL Enabled` to `True`
3.  Copy the value from `SSL URL` above and paste it in the `Redirect URL` field on the top of this page, then click *Update*:<br/><br/>![Project properties](media/active-directory-aspnetwebapp-v1/vsprojectproperties.png)<br />
4.  Add the following in `web.config` file located in root’s folder, under section `configuration\appSettings`:

```xml
<add key="ClientId" value="[Enter the application Id here]" />
<add key="RedirectUri" value="[Enter the Redirect URL here]" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}" /> 
```
-->
<!--end-configure-arp-->
<!--start-test-->
## <a name="test-your-code"></a>Testar o seu código

Prima `F5` para executar o projeto no Visual Studio. O browser abre e direciona a *http://localhost: {porta}* onde vir o *iniciar sessão com a Microsoft* botão. Avançar e clique nele para iniciar sessão.

Quando estiver pronto para testar, utilizar uma conta profissional (Azure Active Directory) para iniciar sessão. 

![Inicie sessão com a janela do browser Microsoft](media/active-directory-aspnetwebapp-v1/aspnetbrowsersignin.png)

![Inicie sessão com a janela do browser Microsoft](media/active-directory-aspnetwebapp-v1/aspnetbrowsersignin2.png)

#### <a name="expected-results"></a>Resultados esperados
Após o início de sessão, o utilizador é redirecionado para a home page do web site, que é o URL HTTPS especificado nas informações de registo da sua aplicação no Portal de registo de aplicações do Microsoft. Esta página mostra agora *Olá {utilizador}* e uma hiperligação para terminar sessão e uma ligação para ver as afirmações do utilizador – que é uma hiperligação para o controlador de autorizar criada anteriormente.

### <a name="see-users-claims"></a>Consulte afirmações do utilizador
Selecione a hiperligação para ver as afirmações do utilizador. Esta ação orienta-o para o controlador e a vista que está disponível apenas para os utilizadores que são autenticados.

#### <a name="expected-results"></a>Resultados esperados
 Deverá ver uma tabela que contém as propriedades básicas do utilizador com sessão iniciada:

| Propriedade | Valor | Descrição|
|---|---|---|
| Nome | {Nome completo do utilizador} | O utilizador do primeiro e último nome
|Nome de utilizador | <span>user@domain.com</span>| O nome de utilizador utilizado para identificar o utilizador com sessão iniciada
| Assunto| {Requerente}|Uma cadeia para identificar exclusivamente o início de sessão do utilizador através da web|
| ID do inquilino| {Guid}| A *guid* para exclusivamente representar a organização do utilizador do Azure Active Directory.|

Além disso, pode ver uma tabela, incluindo todas as afirmações de utilizador incluídas no pedido de autenticação. Para obter uma lista de todas as afirmações no respetiva explicação e um Token de ID, consulte este [artigo](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims "lista de afirmações do Token de ID de").


### <a name="test-accessing-a-method-that-has-an-authorize-attribute-optional"></a>Teste aceder a um método que tenha um *[autorizar]* atributo (opcional)
Neste passo, testar o acesso a controlador de afirmações como um utilizador anónimo:<br/>
Selecione a ligação ao fim de sessão do utilizador e concluir o processo de início de sessão.<br/>
Agora no seu browser, escreva http://localhost: {porta} / afirmações para aceder ao seu controlador de que está protegido com o `[Authorize]` atributo

#### <a name="expected-results"></a>Resultados esperados
Deverá receber a linha de comandos exigir que se autentique, para ver a vista.

## <a name="additional-information"></a>Informações adicionais

<!--start-collapse-->
### <a name="protect-your-entire-web-site"></a>Proteger o web site completo
Para proteger todo o site web, adicione o `AuthorizeAttribute` para `GlobalFilters` no `Global.asax` `Application_Start` método:

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

<div></div>
<br/>

<!--end-test-->