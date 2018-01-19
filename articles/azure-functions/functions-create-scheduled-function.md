---
title: "Criar uma função que é executada de acordo com uma agenda no Azure | Microsoft Docs"
description: "Saiba como criar uma função no Azure que é executada com base numa agenda definida por si."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: ba50ee47-58e0-4972-b67b-828f2dc48701
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/03/2018
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 33f6cd9e10782bb8ff1b0ddf6d047aebc83f8008
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2018
---
# <a name="create-a-function-in-azure-that-is-triggered-by-a-timer"></a>Criar uma função no Azure que é acionada por um temporizador

Saiba como utilizar as Funções do Azure para criar uma função [sem servidor](https://azure.microsoft.com/overview/serverless-computing/) que é executada com base numa agenda definida por si.

![Criar uma aplicação de função no portal do Azure](./media/functions-create-scheduled-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial:

+ Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Criar uma aplicação de Funções do Azure

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Aplicação Function App criada com êxito.](./media/functions-create-first-azure-function/function-app-create-success.png)

Em seguida, vai criar uma função na aplicação Function App nova.

<a name="create-function"></a>

## <a name="create-a-timer-triggered-function"></a>Criar uma função acionada por temporizador

1. Expanda a aplicação de funções e clique no botão **+**, junto a **Funções**. Se esta for a primeira função na sua aplicação de funções, selecione **Função personalizada**. É apresentado o conjunto completo de modelos de função.

    ![Página de início rápido das funções no portal do Azure](./media/functions-create-scheduled-function/add-first-function.png)

2. No campo de pesquisa, escreva `timer` e, em seguida, escolha o idioma pretendido para o modelo de acionador do temporizador. 

    ![Escolha o modelo de função acionada por temporizador.](./media/functions-create-scheduled-function/functions-create-timer-trigger.png)

3. Configure o novo acionador com as definições, conforme especificado na tabela abaixo da imagem.

    ![Crie uma função acionada por um cronómetro no portal do Azure.](./media/functions-create-scheduled-function/functions-create-timer-trigger-2.png)

    | Definição | Valor sugerido | Descrição |
    |---|---|---|
    | **Nome** | Predefinição | Define o nome da sua função acionada por temporizador. |
    | **[Agenda](http://en.wikipedia.org/wiki/Cron#CRON_expression)** | 0 \*/1 \* \* \* \* | Uma [expressão CRON](http://en.wikipedia.org/wiki/Cron#CRON_expression) de seis campos que agenda a função para ser executada todos os minutos. |

2. Clique em **Criar**. É criada uma função na linguagem que escolheu e que é executada todos os minutos.

3. Veja as informações de rastreio escritas nos registos para verificar a execução.

    ![Visualizador de registo de funções no portal do Azure.](./media/functions-create-scheduled-function/functions-timer-trigger-view-logs2.png)

Agora, altere a agenda da função, para que seja executada a cada hora, em vez de cada minuto. 

## <a name="update-the-timer-schedule"></a>Atualizar a agenda do temporizador

1. Expanda a função e clique em **Integrar**. É aqui que vai definir os enlaces de entrada e saída para a função, bem como a agenda. 

2. Introduza um novo valor por hora para a **Agenda** de `0 0 */1 * * *` e clique em **Guardar**.  

![Agenda de temporizador atualizado de funções no portal do Azure.](./media/functions-create-scheduled-function/functions-timer-trigger-change-schedule.png)

Tem agora uma função que é executada uma vez por hora. 

## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Passos seguintes

Criou uma função que é executada com base numa agenda.

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Para obter mais informações sobre os acionadores de temporizadores, veja [Schedule code execution with Azure Functions](functions-bindings-timer.md) (Agendar a execução de código com as Funções do Azure).
