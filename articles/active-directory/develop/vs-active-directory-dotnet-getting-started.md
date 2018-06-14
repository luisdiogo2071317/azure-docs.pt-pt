---
title: Introdução ao Azure AD nos projetos de MVC do Visual Studio .NET
description: Como começar a utilizar o Azure Active Directory em projetos de MVC do .NET depois de ligar ou criar um Azure AD com o Visual Studio ligada a serviços
services: active-directory
author: ghogen
manager: douge
ms.assetid: 1c8b6a58-5144-4965-a905-625b9ee7b22b
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev
ms.openlocfilehash: d521b8c85fb66b6c50d1b9f07e5f4d653e9e57b9
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
ms.locfileid: "31784076"
---
# <a name="getting-started-with-azure-active-directory-aspnet-mvc-projects"></a>Introdução ao Azure Active Directory (projetos de MVC do ASP.NET)

> [!div class="op_single_selector"]
> - [Introdução](vs-active-directory-dotnet-getting-started.md)
> - [O que aconteceu](vs-active-directory-dotnet-what-happened.md)

Este artigo fornece orientação adicional depois de acrescentar do Active Directory para o projeto MVC do ASP.NET através de **projeto > Serviços ligados** comando do Visual Studio. Se ainda não adicionou o serviço ao seu projeto, pode fazê-lo em qualquer altura.

Consulte [o que aconteceu ao meu projeto MVC?](vs-active-directory-dotnet-what-happened.md) para que as alterações efetuadas ao seu projeto ao adicionar o serviço ligado.

## <a name="requiring-authentication-to-access-controllers"></a>Exigir autenticação para os controladores de acesso

Foram adorned todos os controladores no seu projeto com o `[Authorize]` atributo. Este atributo exige que o utilizador ser autenticado antes de aceder a estes controladores. Para permitir que o controlador ser acedida anonimamente, remova este atributo do controlador de. Se pretender definir as permissões de um nível mais granular, aplique o atributo para cada método de que necessita de autorização em vez de aplicá-la para a classe de controlador.

## <a name="adding-signin--signout-controls"></a>Adicionar início de sessão / SignOut controla

Para adicionar os controlos de início de sessão/SignOut a vista, pode utilizar o `_LoginPartial.cshtml` vista parcial para adicionar a funcionalidade a uma das vistas. Eis um exemplo da funcionalidade adicionada ao padrão `_Layout.cshtml` vista. (Tenha em atenção o último elemento no div com classe navbar-fechar):

```html
<!DOCTYPE html>
 <html>
 <head>
     <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>@ViewBag.Title - My ASP.NET Application</title>
    @Styles.Render("~/Content/css")
    @Scripts.Render("~/bundles/modernizr")
</head>
<body>
    <div class="navbar navbar-inverse navbar-fixed-top">
        <div class="container">
            <div class="navbar-header">
                <button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                </button>
                @Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })
            </div>
            <div class="navbar-collapse collapse">
                <ul class="nav navbar-nav">
                    <li>@Html.ActionLink("Home", "Index", "Home")</li>
                    <li>@Html.ActionLink("About", "About", "Home")</li>
                    <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
                </ul>
                @Html.Partial("_LoginPartial")
            </div>
        </div>
    </div>
    <div class="container body-content">
        @RenderBody() 
        <hr />
        <footer>
            <p>&copy; @DateTime.Now.Year - My ASP.NET Application</p>
        </footer>
    </div>
    @Scripts.Render("~/bundles/jquery")
    @Scripts.Render("~/bundles/bootstrap")
    @RenderSection("scripts", required: false)
</body>
</html>
```

## <a name="next-steps"></a>Passos Seguintes

- [Cenários de autenticação do Azure Active Directory](active-directory-authentication-scenarios.md)
- [Adicionar início de sessão com a Microsoft para uma aplicação web ASP.NET](guidedsetups/active-directory-aspnetwebapp-v1.md)
