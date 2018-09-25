---
title: Autenticação com o Azure AD em clouds nacionais
description: Saiba mais sobre pontos finais de autenticação e de registo de aplicação para nuvens nacionais.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: negoe
ms.reviewer: negoe,andret,saeeda
ms.custom: aaddev
ms.openlocfilehash: 866a86178d66b7b4af069d684e4eb56c12db47ca
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46982006"
---
# <a name="national-clouds"></a>Clouds nacionais

Clouds nacionais (também conhecido como clouds soberanas) são instâncias isoladas fisicamente do Azure. Estas regiões do Azure foram concebidos para se certificar de que os requisitos de residência, de soberania de dados e de conformidade de dados são honrados dentro dos limites geográficos.

Incluindo nuvem global, o Azure Active Directory é implementado nas clouds nacionais do seguintes:  

- Azure US Government
- Azure Alemanha
- Azure China 21Vianet

## <a name="app-registration-endpoints"></a>Pontos finais de registo de aplicação

A tabela seguinte lista os URLs de bases para os pontos de extremidade do Azure Active Directory (Azure AD) utilizados para registar uma aplicação para cada nuvem nacional.

| Em nuvem nacionais | Ponto de final de portal AD do Azure
| --- | --- |
| Azure AD para o Governo dos E.U.A. |https://portal.azure.us
|O Azure AD Alemanha |https://portal.microsoftazure.de
|O Azure AD China, explorado pela 21Vianet |https://portal.azure.cn
|O Azure AD (serviço global)|https://portal.azure.com

## <a name="azure-ad-authentication-endpoints"></a>Pontos finais de autenticação do Azure AD

A tabela seguinte lista os URLs de bases para os pontos de extremidade do Azure Active Directory (Azure AD) utilizados para adquirir tokens para chamar o Microsoft Graph para cada nuvem nacional.

| Em nuvem nacionais | Ponto final de autenticação do Azure AD
| --- | --- |
| Azure AD para o Governo dos E.U.A. |`https://login.microsoftonline.us`
|O Azure AD Alemanha| `https://login.microsoftonline.de`
|O Azure AD China, explorado pela 21Vianet | `https://login.chinacloudapi.cn`
|O Azure AD (serviço global)|`https://login.microsoftonline.com`

Pedidos para os pontos finais de autorização ou token do Azure AD podem ser formados utilizando o URL de base específico da região adequado. Por exemplo, em caso de Alemanha:

- É o ponto de extremidade comum de autorização `https://login.microsoftonline.de/common/oauth2/authorize`
- É o ponto final do token comuns `https://login.microsoftonline.de/common/oauth2/token` 

Para aplicações de inquilino único, substitua comuns nos URLs acima com sua id do inquilino ou o nome, por exemplo, `https://login.microsoftonline.de/contoso.com`

>[!NOTE]
> O [autorização do Azure AD v2.0]( https://docs.microsoft.com/azure/active-directory/develop/active-directory-appmodel-v2-overview) e pontos finais de token apenas estão disponíveis para o serviço global. Ele ainda não é suportado para implementações em nuvem nacionais.

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre [Azure Government](https://docs.microsoft.com/azure/azure-government/)
- Saiba mais sobre [Azure China 21Vianet](https://docs.microsoft.com/azure/china/)
- Saiba mais sobre [Azure Alemanha](https://docs.microsoft.com/azure/germany/)
- Saiba mais sobre o [Noções básicas de autenticação do Azure AD](authentication-scenarios.md)
- Saiba mais sobre [implementação na cloud nacional do Microsoft Graph](https://developer.microsoft.com/graph/docs/concepts/deployments)