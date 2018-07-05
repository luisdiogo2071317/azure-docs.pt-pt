---
title: Utilizar b2clogin.com | Documentos da Microsoft
description: Saiba como utilizar b2clogin.com em vez de login.microsoftonline.com.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/29/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 1d42d9a97244eeff501b9d02b0f143d6ef0c91b2
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37440635"
---
# <a name="using-b2clogincom"></a>Utilizar b2clogin.com

>[!IMPORTANT]
>Esta funcionalidade é a pré-visualização pública 
>

Agora tem a opção para utilizar o serviço do Azure AD B2C com `<YourTenantName>.b2clogin.com` em vez de usar `login.microsoftonline.com`.  Isso tem muitas vantagens:
* Já não partilham o mesmo limite de tamanho do cabeçalho de cookie com os outros produtos da Microsoft.
* Pode remover todas as referências a Microsoft em sua URL (pode substituir `<YourTenantName>.onmicrosoft.com` com o seu ID de inquilino). Por exemplo: `https://<tenantname>.b2clogin.com/tfp/<tenantname>/<policyname>/v2.0/.well-known/openid-configuration`.

 Para tirar partido do b2clogin.com, precisa definir algumas das seguintes ações:

1. Para sua **executar ponto final agora** Certifique-se de que está a utilizar `<YourTenantName>.b2clogin.com` em vez de usar `login.microsoftonline.com`.
2. Se estiver a utilizar a MSAL, precisa definir `validateauthority=false`.  Isto acontece porque o emissor do token se torna`<YourTenantName>.b2clogin.com`.