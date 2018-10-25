---
title: incluir ficheiro
description: incluir ficheiro
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 09/18/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: f21710354572c9b54dbf0e08d02809bb68a8e120
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49805054"
---
Para criar uma conta de armazenamento para fins gerais v2 no portal do Azure, siga estes passos:

1. No portal do Azure, expanda o menu do lado esquerdo para abrir o menu de serviços e escolha **Todos os Serviços**. Em seguida, desloque para baixo até **Armazenamento**e escolha **Contas de armazenamento**. Na janela **Contas de Armazenamento** que é apresentada, escolha **Adicionar**.
1. Selecione a subscrição na qua pretende criar a conta de armazenamento.
1. No campo **Grupo de recursos**, clique em **Criar novo**. Introduza um nome para o novo grupo de recursos, conforme exemplificado na imagem seguinte.

    ![Captura de ecrã que mostra como criar um grupo de recursos no portal](./media/storage-create-account-portal-include/create-resource-group.png)

1. A seguir, introduza um nome para a sua conta de armazenamento. O nome que escolher tem de ser exclusivo no Azure, deve ter entre 3 e 24 carateres de comprimento e apenas pode conter números e letras minúsculas.
1. Selecione uma localização para a sua conta de armazenamento ou utilize a localização predefinida.
1. Deixe os outros campos definidos para os respetivos valores predefinidos:
    - Por predefinição, o **Modelo de implementação** é definido como **Resource manager**.
    - Por predefinição, o campo **Desempenho** é definido como **Padrão**.
    - Por predefinição, o campo **Tipo de conta** é definido como **StorageV2 (armazenamento para fins gerais v2)**.
    - Por predefinição, o campo **Replicação** é definido como **Armazenamento localmente redundante (LRS)**.
    - Por predefinição, **Camada de acesso** está definida como **Frequente**.

1. Clique em **Rever + Criar** para rever as definições de conta de armazenamento e criar a conta.
1. Clique em **Criar**.

Para obter mais informações sobre tipos de contas de armazenamento e outras definições de conta de armazenamento, consulte [Descrição geral da conta de armazenamento do Azure](https://docs.microsoft.com/azure/storage/common/storage-account-overview). Para obter mais informações sobre os grupos de recursos, veja [Descrição geral do Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). 
