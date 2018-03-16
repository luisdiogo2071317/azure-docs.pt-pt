---
title: 'O Azure AD Connect: Totalmente integrada Single Sign-On - conformidade GDPR | Microsoft Docs'
description: Este artigo lida com SSO totalmente integrada do Azure Active Directory (Azure AD) e GDPR compatibilidade.
services: active-directory
keywords: "o que é o Azure AD Connect, GDPR, os componentes necessários para o Azure AD, SSO, o início de sessão único"
documentationcenter: 
author: swkrish
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: billmath
ms.openlocfilehash: 0c7ed376accb1eed01106358491e925d3b8126c5
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="azure-ad-seamless-single-sign-on-gdpr-compliance"></a>Compatibilidade de GDPR do Azure AD totalmente integrada-início de sessão único:

## <a name="overview"></a>Descrição geral

Na Maio de 2018, leis de privacidade Europa, o [regulamento gerais de proteção de dados (GDPR)](http://ec.europa.eu/justice/data-protection/reform/index_en.htm), deve entrem em vigor. O GDPR impõe novas regras em empresas, agências governamentais, não lucros e outras organizações que oferta bens serviços e para as pessoas da União Europeia (EU) ou que recolher e analisam dados associados ao residentes de EU. O GDPR aplica-se, independentemente de onde estão localizadas. 

Produtos e serviços Microsoft, estão disponíveis atualmente para o ajudar a satisfazer os requisitos de GDPR. Saiba mais sobre a política de Microsoft Privacy [Centro de confiança](https://www.microsoft.com/trustcenter).

SSO totalmente integrada de AD do Azure cria o tipo de registo seguinte, o que pode conter EUII:

- Ficheiros de registo de rastreio do Azure AD Connect.

Pode ser contactada conformidade GDPR para SSO totalmente integrada de duas formas:

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

## <a name="next-steps"></a>Passos seguintes

- [**Resolver problemas** ](active-directory-aadconnect-troubleshoot-sso.md) -Saiba como resolver problemas comuns com a funcionalidade.
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - para apresentação de pedidos de funcionalidades de novo.
