---
title: Registar uma aplicação com o ponto final v2.0 do Azure AD através do portal do | Documentos da Microsoft
description: Como registar uma aplicação com a Microsoft para ativar o início de sessão e aceder aos serviços da Microsoft com o ponto final v2.0
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: bb2f701f-3bc3-4759-94a5-8b9d53a8a0b6
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2018
ms.author: celested
ms.custom: aaddev
ms.openlocfilehash: 8ab4e6b5b2813a216b6dd6f0fc108a09239ca9a6
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39506550"
---
# <a name="how-to-register-an-app-with-the-v20-endpoint"></a>Como registar uma aplicação com o ponto final v2.0
Para criar uma aplicação que aceita a conta Microsoft (MSA) de pessoal e profissional ou escolar início de sessão na conta (Azure AD), terá primeiro de registar uma aplicação com a Microsoft. Neste momento, não será possível usar todas as aplicações existentes podem ter com o Azure AD ou MSA - terá de criar um novo.

> [!NOTE]
> Nem todos os cenários do Azure Active Directory e funcionalidades são compatíveis com o ponto final v2.0. Para determinar se deve utilizar o ponto final v2.0, leia sobre o [v2.0 limitações](active-directory-v2-limitations.md).


## <a name="visit-the-microsoft-app-registration-portal"></a>Visite o portal de registo de aplicação do Microsoft
Em primeiro lugar, navegue para o portal de registo de aplicações Microsoft em [ https://apps.dev.microsoft.com/ ](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList). 

Iniciar sessão com um pessoal ou conta escolar ou profissional Microsoft. Se não tiver qualquer um, inscreva-se uma nova conta pessoal.

Feito? Deve agora procurar a sua lista de aplicações da Microsoft, que é provavelmente vazio. Vamos mudar.

Clique em **adicionar uma aplicação**e dê a ele um nome. O portal irá atribuir a aplicação um ID de aplicação globalmente exclusivo, que irá utilizar mais tarde no seu código. Se a sua aplicação inclui um componente do lado do servidor que tem os tokens de acesso para chamar APIs (pensar: Office, o Azure ou o seu próprio web API), vai querer criar uma **segredo de aplicação** aqui também.

Em seguida, adicione a **plataformas** que irá utilizar a sua aplicação.

* Para aplicações baseadas na web, forneça uma **URI de redirecionamento** onde possam ser enviadas mensagens de início de sessão.
* Para aplicações móveis, copie o redirecionamento de padrão criado automaticamente para.
* Para web APIs, um âmbito de padrão para aceder à API Web é criado automaticamente para. Pode optar por adicionar âmbitos adicionais a utilizar o **Adicionar âmbito** botão. Também pode adicionar todas as aplicações que já estão autorizadas a utilizar a sua Web API utilizando o **pré-autorizados aplicativos** formulário. 

Opcionalmente, pode personalizar o aspeto e funcionalidade da sua página de início de sessão no **perfil** secção. Certifique-se de clicar **guardar** antes de continuar.

> [!NOTE]
> Quando cria uma aplicação com [ https://apps.dev.microsoft.com/ ](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), a aplicação será registada no inquilino principal da conta que utiliza para iniciar sessão no portal. Isso significa que não pode registar uma aplicação no seu inquilino do Azure AD com uma conta Microsoft pessoal. Se desejar explicitamente registar uma aplicação num inquilino específico, inicie sessão com uma conta criada originalmente nesse inquilino.


## <a name="build-a-quickstart-app"></a>Criar uma aplicação de início rápido
Agora que tem uma aplicação da Microsoft, pode concluir um dos tutoriais de início rápido v2.0. Seguem-se algumas recomendações:

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

