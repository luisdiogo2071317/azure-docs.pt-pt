---
title: "Crie parceiros para mensagens de empresa-empresa (B2B) – Azure Logic Apps | Microsoft Docs"
description: "Saiba como adicionar parceiros à sua conta de integração com o pacote de integração do Enterprise e Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: divyaswarnkar
manager: anneta
editor: 
ms.assetid: b179325c-a511-4c1b-9796-f7484b4f6873
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 17f15c49e0f8137d5f11c57fa600588cda791c28
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="add-or-update-partners-in-business-to-business-agreements-in-your-workflow"></a>Adicionar ou atualizar parceiros nos contratos de empresa-empresa no seu fluxo de trabalho

Os parceiros são entidades que participam na transações do empresa-empresa (B2B) e trocam mensagens entre si. Antes de poder criar parceiros que representam e outra organização estas transações, tem ambos partilhar informações que identificam e valida as mensagens enviadas por si. Depois de abordar estes detalhes e estiver pronto para começar a relação de negócio, pode criar parceiros na sua conta de integração para representar a ambos.

## <a name="what-roles-do-partners-play-in-your-integration-account"></a>As funções às quais reproduzir parceiros na sua conta de integração?

Para definir os detalhes sobre as mensagens trocadas entre parceiros, criar contratos entre essas parceiros. No entanto, antes de poder criar um contrato, tem de ter adicionado, pelo menos, dois parceiros à sua conta de integração. Organização deve fazer parte do contrato, como o **parceiro anfitrião**. O outro parceiro ou **parceiro convidado** representa a organização que trocas de mensagens com a sua organização. O parceiro de convidado pode ser outra empresa, ou até mesmo um departamento na sua própria organização.

Depois de adicionar estas parceiros, pode criar um contrato.

Receber e enviar as definições são orientado por do ponto de vista do parceiro de alojado. Por exemplo, as definições de receção de um contrato de determinam como o parceiro alojado recebe mensagens enviadas a partir de um parceiro de convidado. Da mesma forma, as definições de envio no contrato indicam como o parceiro alojado envia mensagens para o parceiro de convidado.

## <a name="create-partner"></a>Criar parceiro

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

2. No menu principal do Azure, selecione **todos os serviços**. Na caixa de pesquisa, introduza "integração" e, em seguida, selecione **contas de automatização**.

   ![Localizar a conta de integração](./media/logic-apps-enterprise-integration-partners/account-1.png)

3. Em **contas de automatização**, selecione a conta de integração em que pretende adicionar os seus parceiros.

   ![Selecione a conta de integração](./media/logic-apps-enterprise-integration-partners/account-2.png)

4. Escolha o **parceiros** mosaico.

   ![Escolha "Parceiros"](./media/logic-apps-enterprise-integration-partners/partner-1.png)

5. Em **parceiros**, escolha **adicionar**.

   ![Escolha "Adicionar"](./media/logic-apps-enterprise-integration-partners/partner-2.png)

6. Introduza um nome para o seu parceiro, em seguida, selecione um **qualificador**. Introduza um **valor** para identificar os documentos que recebem as suas aplicações. Quando tiver terminado, escolha **OK**.

   ![Adicione os detalhes de parceiro](./media/logic-apps-enterprise-integration-partners/partner-3.png)

7. Escolha o **parceiros** mosaico novamente.

   ![Escolha o que mosaico "Parceiros"](./media/logic-apps-enterprise-integration-partners/partner-5.png)

   O novo parceiro aparece agora. 

   ![Parceiro de nova vista](./media/logic-apps-enterprise-integration-partners/partner-6.png)

## <a name="edit-partner"></a>Editar parceiro

1. No [portal do Azure](https://portal.azure.com), localize e selecione a sua conta de integração. Escolha o **parceiros** mosaico.

   ![Escolha o que mosaico "Parceiros"](./media/logic-apps-enterprise-integration-partners/edit.png)

2. Em **parceiros**, selecione o parceiro que pretende editar.

   ![Selecione o parceiro a eliminar](./media/logic-apps-enterprise-integration-partners/edit-1.png)

3. Em **atualização parceiro**, efetue as alterações.
Depois de terminar, escolha **guardar**. 

   ![Faça e guarde as alterações](./media/logic-apps-enterprise-integration-partners/edit-2.png)

   Para cancelar as alterações, selecione **rejeitar**.

## <a name="delete-partner"></a>Eliminar parceiro

1. No [portal do Azure](https://portal.azure.com), localize e selecione a sua conta de integração. Escolha o **parceiros** mosaico.

   ![Escolha o que mosaico "Parceiros"](./media/logic-apps-enterprise-integration-partners/delete.png)

2. Em **parceiros**, selecione o parceiro que pretende eliminar.
Escolha **eliminar**.

   ![Eliminar parceiro](./media/logic-apps-enterprise-integration-partners/delete-1.png)

## <a name="next-steps"></a>Passos Seguintes

* [Saiba mais sobre contratos](../logic-apps/logic-apps-enterprise-integration-agreements.md "Saiba mais sobre contratos de integração do enterprise")  

