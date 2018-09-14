---
title: Resolução de problemas - QnAMaker
titlesuffix: Azure Cognitive Services
description: QnAMaker é composto por componentes alojados na conta de utilizador do Azure. Depuração pode exigir que os utilizadores manipular seus recursos do Azure do QnAMaker ou fornecer a equipa de suporte do QnAMaker com informações adicionais sobre sua configuração.
services: cognitive-services
author: nstulasi
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: saneppal
ms.openlocfilehash: 35ae2f5362b8432b94a05f3d86252d35f6f025ad
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45543882"
---
# <a name="qnamaker-troubleshooting"></a>Resolução de problemas do QnAMaker
QnAMaker é composto por componentes alojados na conta de utilizador do Azure. Depuração pode exigir que os utilizadores manipular seus recursos do Azure do QnAMaker ou fornecer a equipa de suporte do QnAMaker com informações adicionais sobre sua configuração.

## <a name="how-to-get-latest-qnamaker-runtime-updates"></a>Como obter as mais recentes atualizações de tempo de execução do QnAMaker
Tempo de execução do QnAMaker faz parte do serviço de aplicações do Azure implementadas quando [criar um serviço de QnAMaker](./set-up-qnamaker-service-azure.md) no portal do Azure. As atualizações são feitas periodicamente o tempo de execução. Para aplicar as atualizações mais recentes para aplicar a sua configuração de QnAMaker, tem de reiniciar o serviço de aplicações.
1. Aceda ao seu serviço de QnAMaker (grupo de recursos) [portal do Azure](https://portal.azure.com)

    ![Grupo de recursos do Azure do QnAMaker](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

2. Clique na aplicação do serviço e abra a secção de descrição geral

     ![Serviço de aplicações do QnAMaker](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

3. Reinicie o serviço de aplicações. Ele deve ser concluído dentro de alguns segundos. Tenha em atenção que seus aplicativos/bots downstream têm por base este serviço QnAMaker estará disponível para os utilizadores finais durante este período de reinício.

    ![Reiniciar o serviço de aplicações QnAMaker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="how-to-get-the-qnamaker-service-hostname"></a>Como obter o nome de anfitrião do serviço do QnAMaker
O nome de anfitrião do QnAMaker serviço é útil para fins de depuração quando contactar o suporte de QnAMaker ou UserVoice. O nome do anfitrião é um URL neste formato: https://*{nome do anfitrião}*. azurewebsites.net.
    
1. Aceda ao seu serviço de QnAMaker (grupo de recursos) [portal do Azure](https://portal.azure.com)

    ![Grupo de recursos do Azure do QnAMaker](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

2. Clique no serviço de aplicações

     ![Serviço de aplicações do QnAMaker](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

3. O URL de nome de anfitrião está disponível na seção de visão geral

    ![Nome de anfitrião do QnAMaker](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-gethostname.png)
    

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Utilizar a API do QnAMaker](./upgrade-qnamaker-service.md)
