---
title: Escala de transmissão em fluxo pontos finais com o portal do Azure | Microsoft Docs
description: Este tutorial explica os passos de dimensionamento pontos finais de transmissão em fluxo com o portal do Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.assetid: 1008b3a3-2fa1-4146-85bd-2cf43cd1e00e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2017
ms.author: juliako
ms.openlocfilehash: f2a14f2622d78a4222a8518172eb1ce8ed9e6637
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
ms.locfileid: "33790217"
---
# <a name="scale-streaming-endpoints-with-the-azure-portal"></a>Dimensionar os pontos finais de transmissão em fluxo com o Portal do Azure
## <a name="overview"></a>Descrição geral

> [!NOTE]
> Para concluir este tutorial, precisa de uma conta do Azure. Para obter mais detalhes, consulte [Avaliação gratuita do Azure](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

Os pontos finais de transmissão em fluxo **Premium** são adequadas para cargas de trabalho avançadas, ao fornecer uma capacidade de largura de banda dimensionável e dedicada. Por predefinição, os clientes que têm os pontos finais de transmissão em fluxo **Premium** recebem uma unidade de transmissão em fluxo (SU). O ponto final de transmissão em fluxo pode ser dimensionado mediante a adição de mais SUs. Cada SU fornece capacidade de largura de banda adicional à aplicação. Para obter mais informações sobre a transmissão em fluxo de tipos de ponto final e a configuração de CDN, consulte o [descrição geral de ponto final de transmissão em fluxo](media-services-streaming-endpoints-overview.md) tópico.
 
Este tópico mostra como dimensionar um ponto final de transmissão em fluxo.

Para obter informações sobre os detalhes de preços, consulte [Detalhes de Preços dos Media Services](http://go.microsoft.com/fwlink/?LinkId=275107).

## <a name="scale-streaming-endpoints"></a>Dimensionar pontos finais de transmissão em fluxo

Para alterar o número de unidades de transmissão em fluxo, faça o seguinte:

1. No [portal do Azure](https://portal.azure.com/) selecione a sua conta de Serviços de Multimédia do Azure.
2. No **definições** janela, selecione **pontos finais de transmissão em fluxo**.
3. Clique no ponto final da transmissão que pretende dimensionar. 

    > [!NOTE] 
    > Apenas pode dimensionar **Premium** pontos finais de transmissão em fluxo.

4. Mova o controlo de deslize para especificar o número de unidades de transmissão em fluxo.

    ![Ponto final de transmissão em fluxo](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints3.png)

## <a name="next-steps"></a>Passos Seguintes
Rever os percursos de aprendizagem dos Serviços de Multimédia

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Enviar comentários
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

