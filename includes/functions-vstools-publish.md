---
title: incluir ficheiro
description: incluir ficheiro
services: functions
author: ggailey777
ms.service: functions
ms.topic: include
ms.date: 07/17/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: e8cb5dadb7eed5eb33c15d7a4d1d4640a10099c9
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/21/2018
ms.locfileid: "40245790"
---
1. No **Explorador de Soluções**, clique com o botão direito do rato no projeto e selecione **Publicar**.

2. Selecione **Aplicação de Função do Azure**, escolha **Criar Nova**  e selecione **Publicar**.

    ![Escolher um destino de publicação](./media/functions-vstools-publish/functions-vstools-create-new-function-app.png)

3. Se ainda não tiver associado o Visual Studio à sua conta do Azure, selecione **Adicionar uma conta...**.

4. Na caixa de diálogo **Criar Serviço de Aplicações**, utilize as definições de **Anfitrião**, conforme especificadas na tabela abaixo da imagem:

    ![Caixa de diálogo Criar App Service](./media/functions-vstools-publish/functions-vstools-publish.png)

    | Definição      | Valor sugerido  | Descrição                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nome da Aplicação** | Nome globalmente exclusivo | Nome que identifica exclusivamente a sua nova aplicação de funções. |
    | **Subscrição** | Escolher a sua subscrição | A subscrição do Azure que deve utilizar. |
    | **[Grupo de Recursos](../articles/azure-resource-manager/resource-group-overview.md)** | myResourceGroup |  Nome do grupo de recursos no qual a sua aplicação de funções será criada. Escolha **Novo** para criar um grupo de recursos novo.|
    | **[Plano do Serviço de Aplicações](../articles/azure-functions/functions-scale.md)** | Plano de consumo | Certifique-se de que escolhe **Consume** por baixo de **Tamanho** depois de clicar em **Novo** para criar um plano sem servidor. Escolha também uma **Localização** numa [região](https://azure.microsoft.com/regions/) perto de si ou de outros serviços aos quais as suas funções acedem. Quando executa um plano diferente de **Consumo**, deve gerir o [dimensionamento da sua aplicação de funções](../articles/azure-functions/functions-scale.md).  |
    | **[Conta de Armazenamento](../articles/storage/common/storage-quickstart-create-account.md)** | Conta de armazenamento para fins gerais | O runtime das Funções precisa de uma conta de armazenamento do Azure. Clique em **Novo** para criar uma conta de armazenamento para fins gerais. Também pode utilizar uma conta existente que cumpra os [requisitos de conta de armazenamento](../articles/azure-functions/functions-scale.md#storage-account-requirements).  |

5. Clique em **Criar** para criar uma aplicação de função e os recursos relacionados no Azure com estas definições e implemente o código do projeto de função. 

6. Quando a implementação estiver concluída, aponte o valor **URL do Site**, que é o endereço da sua aplicação de função no Azure.

    ![Mensagem de publicação bem-sucedida](./media/functions-vstools-publish/functions-vstools-publish-profile.png)
