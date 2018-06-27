---
title: Privacidade do utilizador e o Azure AD totalmente integrado de sessão único-| Microsoft Docs
description: Este artigo lida com SSO totalmente integrada do Azure Active Directory (Azure AD) e GDPR compatibilidade.
services: active-directory
keywords: o que é o Azure AD Connect, GDPR, os componentes necessários para o Azure AD, SSO, o início de sessão único
documentationcenter: ''
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: a4fc779cdfb177a9817049fd7b62b0014e141ce0
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "34592413"
---
# <a name="user-privacy-and-azure-ad-seamless-single-sign-on"></a>Privacidade do utilizador e do Azure AD totalmente integrada Single Sign-On

[!INCLUDE [Privacy](../../../includes/gdpr-intro-sentence.md)]

## <a name="overview"></a>Descrição geral


SSO totalmente integrada de AD do Azure cria o tipo de registo seguinte, o que pode conter dados pessoais: 

- Ficheiros de registo de rastreio do Azure AD Connect.

Melhorar a privacidade do utilizador para SSO totalmente integrada de duas formas:

1.  Mediante pedido, extrair dados para uma pessoa e remover dados dessa pessoa das instalações.
2.  Certifique-se de que não existem dados são mantidos para além de 48 horas.

Recomendamos vivamente a segunda opção, é mais fácil de implementar e manter. Consulte as seguintes instruções para cada tipo de registo:

### <a name="delete-azure-ad-connect-trace-log-files"></a>Eliminar ficheiros de registo de rastreio do Azure AD Connect

Verifique o conteúdo do **%ProgramData%\AADConnect** conteúdo de registo de pasta e elimine o rastreio (**rastreio -\*. log** ficheiros) desta pasta dentro de 48 horas de instalar ou atualizar o Azure AD Connect ou modificar a configuração de SSO totalmente integrada, como esta ação pode criar dados abrangidos por GDPR.

>[!IMPORTANT]
>Não elimine o **PersistedState.xml** de ficheiros nesta pasta, este ficheiro é utilizado para manter o estado da instalação anterior do Azure AD Connect e é utilizado quando é efetuada uma instalação de atualização. Este ficheiro nunca irá conter todos os dados sobre uma pessoa e nunca deve ser eliminado.

Pode rever e eliminar estes ficheiros de registo de rastreio utilizando o Explorador do Windows ou pode utilizar o seguinte script do PowerShell para executar as ações necessárias:

```
$Files = ((Get-Item -Path "$env:programdata\aadconnect\trace-*.log").VersionInfo).FileName 
 
Foreach ($file in $Files) { 
    {Remove-Item -Path $File -Force} 
}
```

Guarde o script num ficheiro com o ". PS1 "extensão. Execute este script conforme necessário.

Para saber mais sobre relacionados com os requisitos do Azure AD Connect GDPR, consulte [neste artigo](active-directory-aadconnect-gdpr.md).

### <a name="note-about-domain-controller-logs"></a>Tenha em atenção sobre os registos do controlador de domínio

Se um registo de auditoria estiver ativado, este produto poderá gerar registos de segurança para os controladores de domínio. Para obter mais informações sobre como configurar políticas de auditoria, leia este [artigo](https://technet.microsoft.com/library/dd277403.aspx).

## <a name="next-steps"></a>Passos Seguintes
* [Rever a política de Microsoft Privacy no Centro de confiança](https://www.microsoft.com/trustcenter)
- [**Resolver problemas** ](active-directory-aadconnect-troubleshoot-sso.md) -Saiba como resolver problemas comuns com a funcionalidade.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - para apresentação de pedidos de funcionalidades de novo.
