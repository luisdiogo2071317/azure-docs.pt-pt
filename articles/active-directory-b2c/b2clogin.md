---
title: Utilizar b2clogin.com| Microsoft Docs
description: Saiba como utilizar b2clogin.com em vez de login.microsoftonline.com.
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 04/29/2018
ms.author: davidmu
ms.openlocfilehash: d2737e995b91caa2dcc55027baa2b552e64af23e
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/04/2018
---
#<a name="using-b2clogincom"></a>Utilizar b2clogin.com

>[!IMPORTANT]
>Esta funcionalidade é pré-visualização pública 
>

Tem agora a opção para utilizar o serviço do Azure AD B2C com `<YourTenantName>.b2clogin.com` em vez de utilizar `login.microsoftonline.com`.  Isto tem várias vantagens:
* Já não vai partilhar o mesmo limite de tamanho de cabeçalho de cookie com outros produtos da Microsoft
* Pode remover todas as referências para a Microsoft no seu URL (pode substituir `<YourTenantName>.onmicrosoft.com` com o ID de inquilino)

 Para tirar partido das b2clogin.com, tem de definir alguns dos seguintes procedimentos:

1. Para sua **executar agora endpoint** Certifique-se de que está a utilizar `<YourTenantName>.b2clogin.com` em vez de utilizar `login.microsoftonline.com`.
2. Se estiver a utilizar MSAL, tem de definir `validateauthority=false`.  Isto acontece porque o emissor de token fica`<YourTenantName>.b2clogin.com`.