---
title: Processamento de suporte de dados de escala no portal do Azure | Microsoft Docs
description: Este tutorial explica os passos de dimensionamento suportes de dados no portal do Azure de processamento.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.assetid: e500f733-68aa-450c-b212-cf717c0d15da
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/04/2017
ms.author: juliako
ms.openlocfilehash: 51973916c97282ac93032ab833402d9d1356647e
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
ms.locfileid: "33790315"
---
# <a name="change-the-reserved-unit-type"></a>Alterar tipo de unidade reservada
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-encoding-units.md)
> * [Portal](media-services-portal-scale-media-processing.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

> [!NOTE]
> Para obter a versão mais recente do Java SDK e começar a programar com o Java, veja [Introdução ao SDK do cliente de Java dos Serviços de Multimédia](https://docs.microsoft.com/azure/media-services/media-services-java-how-to-use). <br/>
> Para transferir o SDK mais recente do PHP para os Serviços de Multimédia, procure a versão 0.5.7 do pacote Microsoft/WindowAzure no [repositório Packagist](https://packagist.org/packages/microsoft/windowsazure#v0.5.7).  

## <a name="overview"></a>Descrição geral

As contas dos Serviços de Multimédia estão associadas a um Tipo de Unidade Reservada, que determina a velocidade do processamento das suas tarefas de processamento de mulitmédia. Pode escolher de entre os tipos de unidades reservadas **S1**, **S2** ou **S3**. Por exemplo, a mesma tarefa de trabalho de codificação é executada mais depressa se utilizar o tipo de unidade reservada **S2** em comparação com o tipo **S1**.

Para além de especificar o tipo de unidade reservada, pode especificar o aprovisionamento da sua conta com **Unidades Reservadas** (RUs). O número de RUs aprovisionadas determina o número de tarefas de multimédia que podem ser processadas em simultâneo numa determinada conta.

>[!NOTE]
>As RUs destinam-se a paralelizar todos os processamentos de multimédia, incluindo trabalhos de indexação com o Azure Media Indexer. No entanto, ao contrário da codificação, os trabalhos de indexação não são processados mais depressa com unidades reservadas mais rápidas.

> [!IMPORTANT]
> Certifique-se de que revê o [descrição geral](media-services-scale-media-processing-overview.md) tópico para obter mais informações sobre dimensionamento processamento tópico de suporte de dados.
> 
> 

## <a name="scale-media-processing"></a>Dimensionar processamento de multimédia
Para alterar o tipo de unidade reservada e o número de unidades reservadas, efetue o seguinte:

1. No [portal do Azure](https://portal.azure.com/) selecione a sua conta de Serviços de Multimédia do Azure.
2. No **definições** janela, selecione **unidades reservadas de multimédia**.
   
    Para alterar o número de unidades reservadas para o tipo de unidade reservada selecionado, utilize o **suporte de dados fornecido unidades** controlo de deslize na parte superior do ecrã.
   
    Para alterar o **reservado um tipo de unidade**, clique em de **velocidade de unidades de processamento reservado** barra. Em seguida, selecione o escalão de preço é necessário: S1, S2 ou S3.
   
3. Prima o botão GUARDAR para guardar as alterações.
   
    As unidades novos reservadas são alocadas quando prime guardar.

## <a name="next-steps"></a>Passos Seguintes
Rever os percursos de aprendizagem dos Serviços de Multimédia

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Enviar comentários
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

