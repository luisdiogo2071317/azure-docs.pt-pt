---
title: Colaborar com outros contribuintes LUIS aplicações do Azure | Microsoft Docs
description: Saiba como Collaborate com contribuintes outros aplicações de compreensão de idiomas (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/05/2018
ms.author: v-geberr
ms.openlocfilehash: c0451f7621a3c18dbf365f3a03934924c030092f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354506"
---
# <a name="collaborate-with-others-on-language-understanding-luis-apps"></a>Colaborar com outras pessoas nas aplicações de compreensão de idiomas (LUIS)  

Podem colaborar com outras pessoas na sua aplicação LUIS em conjunto. 

## <a name="owner-and-collaborators"></a>Proprietário e colaboradores
Uma aplicação tem um único proprietário, mas pode ter muitos colaboradores. 

## <a name="add-collaborator"></a>Adicionar colaborador

Para permitir que os colaboradores editar a sua aplicação LUIS, no **definições** página da sua aplicação LUIS, introduza o e-mail do colaborador e clique em **adicionar colaborador**.

![Adicionar colaborador](./media/luis-how-to-collaborate/add-collaborator.png)

* Os colaboradores podem iniciar sessão e editar a sua aplicação LUIS ao mesmo tempo que está a trabalhar na aplicação. <!--If a collaborator edits the LUIS app, you see a notification at the top of the browser.-->
* Os colaboradores não é possível adicionar outros colaboradores.

## <a name="set-application-as-public"></a>Conjunto aplicacional como público
Consulte [pública endpoint acesso à aplicação](luis-concept-security.md#public-app-endpoint-access) para obter mais informações.

### <a name="transfer-of-ownership"></a>Transferência de propriedade
Enquanto LUIS atualmente não suporta a transferência de propriedade, pode exportar a sua aplicação e outro utilizador LUIS pode importar a aplicação. Poderão existir diferenças menores em pontuações LUIS entre as duas aplicações. 
