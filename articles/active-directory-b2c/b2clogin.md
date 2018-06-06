---
title: Utilizar b2clogin.com | Microsoft Docs
description: Saiba como utilizar b2clogin.com em vez de login.microsoftonline.com.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 04/29/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: c41c02acaeffa170d55f3c59f34a4b1ecae1c523
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34712458"
---
# <a name="using-b2clogincom"></a>Utilizar b2clogin.com

>[!IMPORTANT]
>Esta funcionalidade é pré-visualização pública 
>

Tem agora a opção para utilizar o serviço do Azure AD B2C com `<YourTenantName>.b2clogin.com` em vez de utilizar `login.microsoftonline.com`.  Isto tem várias vantagens:
* Já não vai partilhar o mesmo limite de tamanho de cabeçalho de cookie com outros produtos da Microsoft
* Pode remover todas as referências para a Microsoft no seu URL (pode substituir `<YourTenantName>.onmicrosoft.com` com o ID de inquilino)

 Para tirar partido das b2clogin.com, tem de definir alguns dos seguintes procedimentos:

1. Para sua **executar agora endpoint** Certifique-se de que está a utilizar `<YourTenantName>.b2clogin.com` em vez de utilizar `login.microsoftonline.com`.
2. Se estiver a utilizar MSAL, tem de definir `validateauthority=false`.  Isto acontece porque o emissor de token fica`<YourTenantName>.b2clogin.com`.