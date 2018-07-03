---
title: Colaborar com outros contribuidores no LUIS aplicações no Azure | Documentos da Microsoft
description: Saiba como colabore com outros contribuintes em aplicativos de compreensão de idiomas (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/05/2018
ms.author: v-geberr
ms.openlocfilehash: 55c50f5276e2cf97ac98d75b8fc2bd56f1c5d644
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/02/2018
ms.locfileid: "37340418"
---
# <a name="collaborate-with-others-on-language-understanding-luis-apps"></a>Colaborar com outras pessoas em aplicações de compreensão de idiomas (LUIS)  

Pode colaborar com outras pessoas na sua aplicação LUIS em conjunto. 

## <a name="owner-and-collaborators"></a>Proprietário e os funcionários
Uma aplicação tem um único proprietário, mas pode ter muitos funcionários. 

## <a name="add-collaborator"></a>Adicionar acesso do funcionário

Para permitir que os funcionários para editar a sua aplicação do LUIS, sobre o **definições** página da sua aplicação LUIS, introduza o e-mail do colaborador e clique em **colaborador de adicionar**.

![Adicionar acesso do funcionário](./media/luis-how-to-collaborate/add-collaborator.png)

* Colaboradores podem iniciar sessão e editar a sua aplicação do LUIS, ao mesmo tempo em que estiver a trabalhar na aplicação. <!--If a collaborator edits the LUIS app, you see a notification at the top of the browser.-->
* Colaboradores não é possível adicionar outros colaboradores.

Ver [utilizador de inquilino do Azure Active Directory](luis-how-to-account-settings.md#azure-active-directory-tenant-user) para saber mais sobre contas de utilizador do Active Directory. 

## <a name="set-application-as-public"></a>Conjunto de aplicações como público
Ver [acesso de ponto final de aplicações públicas](luis-concept-security.md#public-app-endpoint-access) para obter mais informações.

### <a name="transfer-of-ownership"></a>Transferência de propriedade
Enquanto o LUIS atualmente não suporta a transferência de propriedade, pode exportar a sua aplicação e outro utilizador de LUIS pode importar a aplicação. Pode haver pequenas diferenças na pontuações de LUIS entre as duas aplicações. 
