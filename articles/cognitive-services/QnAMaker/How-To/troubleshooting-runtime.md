---
title: Resolução de problemas de QnAMaker | Microsoft Docs
titleSuffix: Azure
description: Como resolver problemas com o tempo de execução QnAMaker
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 06/04/2018
ms.author: saneppal
ms.openlocfilehash: 23847c81a39ea392b6e64575406c9dc338b852de
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "35356039"
---
# <a name="qnamaker-troubleshooting"></a>Resolução de problemas de QnAMaker
Compreende QnAMaker dos componentes alojados na conta do Azure do utilizador. Depuração poderá exigir que os utilizadores manipular os seus recursos QnAMaker Azure ou forneça QnAMaker a equipa de suporte com informações adicionais sobre a respetiva configuração.

## <a name="how-to-get-latest-qnamaker-runtime-updates"></a>Como obter as atualizações mais recentes do tempo de execução QnAMaker
Tempo de execução QnAMaker faz parte o App Service do Azure implementadas quando [criar um serviço QnAMaker](./set-up-qnamaker-service-azure.md) no portal do Azure. As atualizações que são efetuadas periodicamente para o tempo de execução. Para aplicar as atualizações mais recentes para aplicar a sua configuração QnAMaker, tem de reiniciar o serviço de aplicações.
1. Aceda ao seu serviço QnAMaker (grupo de recursos) a [portal do Azure](https://portal.azure.com)

    ![Grupo de recursos do QnAMaker Azure](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

2. Clique no serviço de aplicações e abra a secção de descrição geral

     ![QnAMaker do serviço de aplicações](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

3. Reinicie o serviço de aplicações. Deve efetuar a dentro de alguns segundos. Tenha em atenção que as aplicações a jusante/bots criar neste serviço QnAMaker não estará disponível para os utilizadores finais durante este período de reinício.

    ![Reiniciar o serviço de aplicações QnAMaker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)

## <a name="how-to-get-the-qnamaker-service-hostname"></a>Como obter o nome de anfitrião do serviço de QnAMaker
Nome do serviço anfitrião QnAMaker é útil para fins de depuração quando contactar o suporte de QnAMaker ou UserVoice. O nome do anfitrião é um URL deste formulário: https://*{hostname}*. azurewebsites.net.
    
1. Aceda ao seu serviço QnAMaker (grupo de recursos) a [portal do Azure](https://portal.azure.com)

    ![Grupo de recursos do QnAMaker Azure](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-resourcegroup.png)

2. Clique no serviço de aplicações

     ![QnAMaker do serviço de aplicações](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-appservice.png)

3. O URL de nome de anfitrião que está disponível na secção Descrição geral

    ![Nome de anfitrião QnAMaker](../media/qnamaker-how-to-troubleshoot/qnamaker-azure-gethostname.png)
    

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Utilizar QnAMaker API](./upgrade-qnamaker-service.md)
