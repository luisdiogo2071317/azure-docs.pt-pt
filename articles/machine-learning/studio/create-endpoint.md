---
title: Criação de pontos finais de serviço da Web
titleSuffix: Azure Machine Learning Studio
description: Criação de pontos finais de serviço da Web no Azure Machine Learning. Cada ponto de extremidade no serviço Web independente é resolvido, limitado e gerido.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: article
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 02/12/2019
ms.openlocfilehash: a6945ac7bfb750916e229ae04376f895f2b3d506
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/14/2019
ms.locfileid: "56267280"
---
# <a name="creating-endpoints-for-deployed-azure-machine-learning-studio-web-services"></a>Criação de pontos de extremidade dos serviços de web do Azure Machine Learning Studio implementados

> [!NOTE]
> Este tópico descreve técnicas aplicáveis a uma **clássico** serviço Web do Machine Learning.

Depois de implementar um serviço Web, é criado um ponto final predefinido para esse serviço. O ponto final predefinido pode ser chamado através da respetiva chave de API. Pode adicionar mais pontos finais com suas próprias chaves a partir do portal dos serviços Web.
Cada ponto de extremidade no serviço Web independente é resolvido, limitado e gerido. Cada ponto de extremidade é uma URL exclusiva com uma chave de autorização que pode distribuir aos seus clientes.

## <a name="adding-endpoints-to-a-web-service"></a>Adicionar pontos finais para um serviço Web

Pode adicionar um ponto final a um serviço Web através do portal do Azure Machine Learning Web Services. Assim que o ponto final for criado, pode consumi-lo através de APIs síncronas, APIs, do batch e folhas de cálculo do excel.

> [!NOTE]
> Se tiver adicionado pontos finais adicionais para o serviço Web, não é possível eliminar o ponto final predefinido.

1. No Machine Learning Studio, na coluna de navegação esquerda, clique em serviços da Web.
2. Na parte inferior do dashboard do serviço Web, clique em **gerir pontos finais**. O portal de serviços da Web do Azure Machine Learning abre-se para a página de pontos de extremidade para o serviço Web.
3. Clique em **Novo**.
4. Escreva um nome e descrição para o novo ponto final. Nomes de ponto final tem de ser 24 carateres ou menos de comprimento e devem ser constituídos por letras do alfabeto em minúsculas ou números. Selecione o nível de registo e se os dados de exemplo estão ativados. Para obter mais informações sobre o registo, consulte [ativar o registo para os serviços Web do Machine Learning](web-services-logging.md).

## <a name="next-steps"></a>Passos Seguintes

[Como consumir um serviço Web do Azure Machine Learning](consume-web-services.md).
