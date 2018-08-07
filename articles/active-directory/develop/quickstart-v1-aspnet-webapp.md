---
title: Servidor Web do Azure AD v1 ASP.NET introdução | Documentos da Microsoft
description: Implementar início de sessão no Microsoft numa solução ASP.NET, com um aplicativo de baseadas no browser da web tradicional com OpenID Connect standard
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/23/2018
ms.author: andret
ms.openlocfilehash: 5353e22d7ae77adecfe126bb589d08c808752550
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39579355"
---
<!--start-intro-->
# <a name="add-sign-in-with-microsoft-to-an-aspnet-web-app"></a>Adicionar início de sessão com a Microsoft a uma aplicação web ASP.NET

Este guia demonstra como implementar o início de sessão com a Microsoft com uma solução ASP.NET MVC tradicional baseada no browser e aplicações web utilizando OpenID Connect. 

No final deste guia, a aplicação irá aceitar inícios de sessão de contas profissionais e escolares de organizações que tem integrado com o Azure Active Directory.

> [!NOTE]
> Esta configuração assistida ajuda-o para permitir inícios de sessão das contas profissionais e escolares no seu aplicativo ASP.NET. Se estiver interessado permitir inícios de sessão de contas pessoais, além de contas profissionais e escolares, pode utilizar o [ponto final v2](azure-ad-endpoint-comparison.md). Ver [este ASP.NET orientada a configuração para o ponto final v2](tutorial-v2-asp-webapp.md) , bem como [neste documento](active-directory-v2-limitations.md) explicando as limitações atuais do ponto final v2.
<br/><br/>

<!--separator-->

> Este guia requer o Visual Studio 2015 Update 3 ou o Visual Studio 2017.  Não tem?  [Transfira gratuitamente o Visual Studio 2017](https://www.visualstudio.com/downloads/)

## <a name="how-this-guide-works"></a>Como funciona este guia

![Como funciona este guia](./media/quickstart-v1-aspnet-webapp/aspnet-intro.png)

Este guia baseia-se no cenário em que um navegador acede a um site ASP.NET, solicitando um utilizador seja autenticado por meio de um botão de início de sessão. Neste cenário, a maior parte do trabalho para processar a página da web ocorre no lado do servidor.

> [!NOTE]
> Esta configuração assistida demonstra como iniciar sessão dos utilizadores num aplicativo da Web de ASP.NET a partir de um modelo em branco e incluir passos como adicionar um início de sessão no botão e cada controlador e métodos, enquanto também Explicamos alguns conceitos. Em alternativa, também pode criar um projeto de início de sessão do Azure Active Directory utilizadores (contas profissionais e escolares) ao utilizar o [modelo de web do Visual Studio](https://docs.microsoft.com/aspnet/visual-studio/overview/2013/creating-web-projects-in-visual-studio#organizational-account-authentication-options) e selecionando *contas institucionais* e, em seguida, uma das opções na cloud - esta opção utiliza um modelo mais avançado, com vistas, métodos e controladores adicionais.

## <a name="libraries"></a>Bibliotecas

Este guia utiliza os seguintes pacotes:

|Biblioteca|Descrição|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|Middleware que permite que uma aplicação utilizar OpenIdConnect para autenticação|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|Middleware que permite que uma aplicação manter a utilização de cookies de sessão de utilizador|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|Permite que as aplicações com base em OWIN ser executado no IIS através do pipeline de solicitação do ASP.NET|


<!--end-intro-->

<!--start-setup-->

## <a name="set-up-your-project"></a>Configurar seu projeto

Esta secção mostra os passos para instalar e configurar o pipeline de autenticação por meio de middleware da OWIN num projeto do ASP.NET através do OpenID Connect. 

> Prefere transferir o projeto do Visual Studio este exemplo em vez disso? [Transfira um projeto](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/GuidedSetup.zip) e avance para o [passo de configuração](#configure-your-webconfig-and-register-an-application) para configurar o exemplo de código antes de executar.

## <a name="create-your-aspnet-project"></a>Criar o seu projeto ASP.NET
1. No Visual Studio: `File` > `New` > `Project`<br/>
2. Sob *Visual C# \Web*, selecione `ASP.NET Web Application (.NET Framework)`.
3. Nome da sua aplicação e clique em *OK*
4. Selecione `Empty` e, em seguida, selecione a caixa de verificação para adicionar `MVC` referências

## <a name="add-authentication-components"></a>Adicionar componentes de autenticação

1. No Visual Studio: `Tools` > `Nuget Package Manager` > `Package Manager Console`
2. Adicione *pacotes de NuGet de middleware do OWIN* digitando o seguinte na janela da consola do Gestor de pacotes:

    ```powershell
    Install-Package Microsoft.Owin.Security.OpenIdConnect
    Install-Package Microsoft.Owin.Security.Cookies
    Install-Package Microsoft.Owin.Host.SystemWeb
    ```

<!--start-collapse-->
> ### <a name="about-these-packages"></a>Sobre esses pacotes
>As bibliotecas acima ativar início de sessão único (SSO) através do OpenID Connect através da autenticação baseada em cookies. Após a autenticação é concluída e o token que representa o utilizador é enviado para a sua aplicação, o middleware da OWIN cria um cookie de sessão. O browser, em seguida, utiliza esse cookie em solicitações subsequentes para que o utilizador não tem de autenticar e nenhuma verificação adicional é necessária.
<!--end-collapse-->

## <a name="configure-the-authentication-pipeline"></a>Configurar o pipeline de autenticação
Os passos seguintes são utilizados para criar um middleware da OWIN *classe de Startup* para configurar a autenticação OpenID Connect. Esta classe é executada automaticamente.

> [!TIP]
> Se o seu projeto não tiver um `Startup.cs` ficheiro na pasta raiz:<br/>
> 1. Com o botão direito na pasta raiz do projeto: >    `Add` > `New Item...` > `OWIN Startup class`<br/>
> 2. Nomeie-o `Startup.cs`<br/>
>
>> Certifique-se de que a classe selecionada é uma classe de Startup da OWIN e não uma padrão classe c#. Confirmar isso verificando se vir `[assembly: OwinStartup(typeof({NameSpace}.Startup))]` acima o espaço de nomes.


1. Adicione *OWIN* e *IdentityModel* espaços de nomes para `Startup.cs`:

    [!code-csharp[main](../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet\Startup.cs?name=AddedNameSpaces "Startup.cs")]

2. Substitua a classe de inicialização com o código a seguir:

    [!code-csharp[main](../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet\Startup.cs?name=Startup "Startup.cs")]
    
<!--start-collapse-->
> [!NOTE]
> Os parâmetros que fornecer no *OpenIDConnectAuthenticationOptions* servir de coordenadas para a aplicação comunicar com o Azure AD. Como o middleware de OpenID Connect usa cookies, terá também de configurar a autenticação de cookie, tal como mostra o código anterior. O *ValidateIssuer* valor indica OpenIdConnect não restringir o acesso a uma organização específica.
<!--end-collapse-->

<!--end-setup-->

<!--start-use-->

## <a name="add-a-controller-to-handle-sign-in-and-sign-out-requests"></a>Adicionar um controlador para processar pedidos de início de sessão e fim de sessão

Este passo mostra como criar um novo controlador para expor métodos de início de sessão e fim de sessão.

1.  Com o botão direito do `Controllers` pasta e selecione `Add` > `Controller`
2.  Selecione `MVC (.NET version) Controller – Empty`.
3.  Clique em *adicionar*
4.  Atribua o nome `HomeController` e clique em *adicionar*
5.  Adicione *OWIN* espaços de nomes para a classe:

    [!code-csharp[main](../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet\Controllers\HomeController.cs?name=AddedNameSpaces "HomeController.cs")]

6. Adicione os seguintes métodos para lidar com início de sessão e fim de sessão para o seu controlador, iniciando um desafio de autenticação por meio do código:

    [!code-csharp[main](../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet\Controllers\HomeController.cs?name=SigInAndSignOut "HomeController.cs")]
    
## <a name="create-the-apps-home-page-to-sign-in-users-via-a-sign-in-button"></a>Criar home page a aplicação para iniciar sessão dos utilizadores através de um botão de início de sessão

No Visual Studio, crie uma nova vista para adicionar o botão de início de sessão e apresentar informações de utilizador após a autenticação:

1.  Com o botão direito do `Views\Home` pasta e selecione `Add View`
2.  Dê-lhe o nome `Index`.
3.  Adicione o seguinte HTML, que inclui o botão de início de sessão, para o ficheiro:

    [!code-html[main](../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet/Views/Home/Index.cshtml "Index.cshtml")]

<!--start-collapse-->
> [!NOTE]
> Esta página adiciona um botão de início de sessão no formato SVG com um plano de fundo preto:<br/>![Início de sessão com a Microsoft](./media/quickstart-v1-aspnet-webapp/aspnetsigninbuttonsample.png)<br/> Para obter mais início de sessão botões, aceda a [esta página](https://docs.microsoft.com/azure/active-directory/develop/active-directory-branding-guidelines "diretrizes de imagem corporativa").
<!--end-collapse-->

## <a name="display-users-claims-by-adding-a-controller"></a>Apresentar afirmações de utilizadores ao adicionar um controlador
Este controlador demonstra as utilizações do `[Authorize]` atributo para proteger um controlador. Este atributo restringe o acesso para o controlador, permitindo apenas que usuários autenticados. O seguinte código faz com usa o atributo para afirmações de utilizador que foram obtidas são apresentadas como parte da entrada.

1.  Com o botão direito do `Controllers` pasta: `Add` > `Controller`
2.  Selecione `MVC {version} Controller – Empty`.
3.  Clique em *adicionar*
4.  Nomeie-o `ClaimsController`
5.  Substitua o código da sua classe de controlador com o código a seguir - esta ação adiciona o `[Authorize]` à classe de atributo:

    [!code-csharp[main](../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet\Controllers\ClaimsController.cs?name=ClaimsController "ClaimsController.cs")]

<!--start-collapse-->
> [!NOTE]
> Devido à utilização do `[Authorize]` atributo, todos os métodos deste controlador só pode ser executado se o usuário é autenticado. Se o utilizador não é autenticado e tenta acessar o controlador, o OWIN inicia um desafio de autenticação e forçar o utilizador a autenticar. O código acima examina a coleção de afirmações do utilizador para os atributos específicos incluídos no token do usuário. Esses atributos incluem o nome do utilizador completo e nome de utilizador, bem como o assunto de identificador de utilizador global. Também contém o *ID de inquilino*, que representa a ID para a organização do utilizador. 
<!--end-collapse-->

## <a name="create-a-view-to-display-the-users-claims"></a>Criar uma vista para apresentar as afirmações do utilizador

No Visual Studio, crie uma nova vista para apresentar as afirmações do utilizador numa página da web:

1.  Com o botão direito do `Views\Claims` pasta e: `Add View`
2.  Dê-lhe o nome `Index`.
3.  Adicione o seguinte HTML para o ficheiro:

    [!code-html[main](../../../WebApp-OpenIDConnect-DotNet/WebApp-OpenIDConnect-DotNet/Views/Claims/Index.cshtml "Index.cshtml")]
    
<!--end-use-->

<!--start-configure-->
## <a name="configure-your-webconfig-and-register-an-application"></a>Configurar o seu *Web. config* e registar uma aplicação

1. No Visual Studio, adicione o seguinte ao `web.config` (localizado na pasta raiz) na seção `configuration\appSettings`:

    ```xml
    <add key="ClientId" value="Enter_the_Application_Id_here" />
    <add key="RedirectUrl" value="Enter_the_Redirect_Url_here" />
    <add key="Tenant" value="common" />
    <add key="Authority" value="https://login.microsoftonline.com/{0}" /> 
    ```
2. No Solution Explorer, selecione o projeto e observe os <i>propriedades</i> janela (se não vir uma janela de propriedades, pressione F4)
3. Alteração SSL ativado para <code>True</code>
4. Copie o URL de SSL do projeto para a área de transferência:<br/><br/>![Propriedades do projeto](./media/quickstart-v1-aspnet-webapp/visual-studio-project-properties.png)<br />
5. Na <code>web.config</code>, substitua <code>Enter_the_Redirect_URL_here</code> com o URL de SSL do seu projeto 

### <a name="register-your-application-in-the-azure-portal-then-add-its-information-to-webconfig"></a>Registar a sua aplicação no Portal do Azure, em seguida, adicionar as suas informações em *Web. config*

1. Vá para o [Portal do Microsoft Azure – registos de aplicações](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) registar uma aplicação
2. Selecione `New application registration`
3. Introduza um nome para a sua aplicação
4. Cole o projeto do Visual Studio *URL de SSL* no `Sign-on URL` (este URL é também adicionado automaticamente à lista de URLs de resposta para a aplicação que está a registar)
5. Clique em `Create` de registar a aplicação. Esta ação leva-o novamente para a lista de aplicações
6. Agora, pesquisar e/ou selecione a aplicação que acabou de criar para abrir as respetivas propriedades
7. Copie o guid em `Application ID` na área de transferência
8. Voltar ao Visual Studio e, em `web.config`, substitua `Enter_the_Application_Id_here` com o ID de aplicação a partir da aplicação que acabou de registar

> [!TIP]
> Se a sua conta está configurada para acesso a vários diretórios, certifique-se de que selecionou o diretório correto para a organização pretende que o aplicativo seja registrado ao clicar no nome da sua conta no canto superior com o botão direito no Portal do Azure e, em seguida, verificar o diretório selecionado conforme indicado:<br/>![Selecionar o diretório correto](./media/quickstart-v1-aspnet-webapp/tenantselector.png)

## <a name="configure-sign-in-options"></a>Configurar as opções de início de sessão

Pode configurar a sua aplicação para permitir que apenas os utilizadores que pertencem a instância do Azure Active Directory da organização de um início de sessão, ou aceitar inícios de sessão dos utilizadores que pertencem a qualquer organização. Siga as instruções de uma das seguintes opções:

### <a name="configure-your-application-to-allow-sign-ins-of-work-and-school-accounts-from-any-company-or-organization-multi-tenant"></a>Configurar a sua aplicação para permitir inícios de sessão de contas profissionais e escolares de qualquer empresa ou organização (multi-inquilino)

Siga os passos seguintes, se pretender aceitar inícios de sessão de contas profissionais e escolares de qualquer empresa ou organização que está integrado com o Azure Active Directory. Este é um cenário comum para *aplicações SaaS*:

1. Volte ao [Portal do Microsoft Azure – registos de aplicações](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) e localize a aplicação que acabou de registar
2. Em `All Settings` selecione `Properties`
3. Alteração `Multi-tenanted` propriedade `Yes` e clique em `Save`

Para obter mais informações sobre esta definição e o conceito de aplicações multi-inquilino, consulte [este artigo](howto-convert-app-to-be-multi-tenant.md "descrição geral da multi-inquilino").

### <a name="restrict-users-from-only-one-organizations-active-directory-instance-to-sign-in-to-your-application-single-tenant"></a>Restringir os utilizadores da instância do Active Directory da organização apenas um para iniciar sessão na sua aplicação (inquilino único)

Esta opção é um cenário comum para *aplicativos LOB*: Se quiser que o aplicativo para aceitar inícios de sessão apenas a partir de contas que pertencem a uma instância específica do Azure Active Directory (incluindo *contas de convidado*dessa instância), substitua o `Tenant` parâmetro na *Web. config* partir `Common` com o nome de inquilino da organização – exemplo, *contoso.onmicrosoft.com*. Depois disso, alterar os `ValidateIssuer` argumento em seu [ *classe de Startup da OWIN* ](#configure-the-authentication-pipeline) para `true`.

Para permitir que os utilizadores de apenas uma lista de organizações específicas, defina `ValidateIssuer` true e utilizar o `ValidIssuers` parâmetro para especificar uma lista das organizações.

Outra opção é implementar um método personalizado para validar os emissores usando *IssuerValidator* parâmetro. Para obter mais informações sobre `TokenValidationParameters`, consulte [este artigo MSDN](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx "artigo do TokenValidationParameters MSDN").

<!--end-configure-->

<!--start-configure-arp-->
<!--
## Configure your ASP.NET Web App with the application's registration information

In this step, you will configure your project to use SSL, and then use the SSL URL to configure your application’s registration information. After this, add the application’ registration information to your solution via *web.config*.

1.  In Solution Explorer, select the project and look at the `Properties` window (if you don’t see a Properties window, press F4)
2.  Change `SSL Enabled` to `True`
3.  Copy the value from `SSL URL` above and paste it in the `Redirect URL` field on the top of this page, then click *Update*:<br/><br/>![Project properties](./media/quickstart-v1-aspnet-webapp/vsprojectproperties.png)<br />
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

Prima `F5` para executar o seu projeto no Visual Studio. O navegador é aberto e lhe dá instruções para *http://localhost:{port}* onde vir o *iniciar sessão com a Microsoft* botão. Vá em frente e clique no mesmo para iniciar sessão.

Quando estiver pronto para testar, utilizar uma conta profissional (Azure Active Directory) para iniciar sessão. 

![Inicie sessão com a janela do browser Microsoft](./media/quickstart-v1-aspnet-webapp/aspnetbrowsersignin.png)

![Inicie sessão com a janela do browser Microsoft](./media/quickstart-v1-aspnet-webapp/aspnetbrowsersignin2.png)

#### <a name="expected-results"></a>Resultados esperados
Após o início de sessão, o utilizador é redirecionado para a home page do seu web site, que é o URL HTTPS especificado nas informações de registo da sua aplicação no Portal de registo de aplicação do Microsoft. Esta página mostra agora *Olá {User}* e uma ligação para terminar sessão e uma ligação para ver as afirmações do utilizador – o que é uma ligação para o controlador de autorizar criado anteriormente.

### <a name="see-users-claims"></a>Veja as afirmações do utilizador
Selecione a hiperligação para ver as afirmações do utilizador. Esta ação orienta-o para o controlador e o modo de exibição que só está disponível para utilizadores que são autenticados.

#### <a name="expected-results"></a>Resultados esperados
 Deverá ver uma tabela que contém as propriedades básicas do utilizador com sessão iniciada:

| Propriedade | Valor | Descrição|
|---|---|---|
| Nome | {Nome completo do utilizador} | O utilizador do primeiro e último nome
|Nome de utilizador | <span>user@domain.com</span>| O nome de utilizador utilizado para identificar o utilizador com sessão iniciada
| Assunto| {Subject}|Uma cadeia de caracteres para identificar exclusivamente o início de sessão do utilizador em toda a web|
| ID do inquilino| {Guid}| R *guid* que representa exclusivamente a organização do utilizador do Azure Active Directory.|

Além disso, verá uma tabela, incluindo todas as afirmações de utilizador incluídas no pedido de autenticação. Para obter uma lista de todas as afirmações no Token de ID e sua explicações, veja este [artigo](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims "lista de afirmações no Token de ID").


### <a name="test-accessing-a-method-that-has-an-authorize-attribute-optional"></a>Teste a aceder a um método que tem um *[Authorize]* atributo (opcional)
Neste passo, vai testar a aceder ao controlador de afirmações como um utilizador anónimo:<br/>
Selecione a ligação para o fim de sessão do utilizador e concluir o processo de fim de sessão.<br/>
Agora no seu browser, escreva http://localhost:{port}/claims para aceder ao seu controlador que está protegido com o `[Authorize]` atributo

#### <a name="expected-results"></a>Resultados esperados
Deverá receber a linha de comandos que seja necessário autenticar para ver a vista.

## <a name="additional-information"></a>Informações adicionais

<!--start-collapse-->
### <a name="protect-your-entire-web-site"></a>Proteger todo o web site
Para proteger todo o web site, adicione a `AuthorizeAttribute` para `GlobalFilters` na `Global.asax` `Application_Start` método:

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

<div></div>
<br/>

<!--end-test-->