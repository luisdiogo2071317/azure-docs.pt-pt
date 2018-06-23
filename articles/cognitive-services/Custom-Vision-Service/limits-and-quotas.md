---
title: Limites e quotas para o serviço de visão personalizada - serviços cognitivos do Azure | Microsoft Docs
description: Saiba mais sobre os limites e quotas Cognitives serviços personalizada visão do serviço Azure.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 03/16/2018
ms.author: anroth
ms.openlocfilehash: 44666d5d7f2a51e4017c704205d21b1f6d06908c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355052"
---
# <a name="limits-and-quotas"></a>Limites e quotas

Existem três camadas de chaves para o serviço de visão personalizada. Recursos F0 e S0 são obtidos através do portal do Azure. Detalhes sobre preços e transações definições estão no [página de preços](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/).  Projetos de F0 podem ser atualizados para projetos de S0.

Recursos de projeto de avaliação limitados estão ligados ao início de sessão personalizada visão (ou seja, uma conta do AAD conta escolar ou MSA.) São se destina a ser utilizado para avaliações abreviadas do serviço.  As contas criadas durante a pré-visualização gratuita antigo, antes da introdução de pré-visualizações do Azure (1 de Março de 2018) irão manter as quotas anteriores para avaliações limitado. 

||**Avaliação limitada**|**F0 (Azure)**|**S0 (Azure)**|
|-----|-----|-----|-----|
|Projetos|2|2|100|
|Imagens de formação por projeto|5.000|5.000|50,000|
|Predições / mês|10,000 |10,000|Ilimitado|
|Etiquetas / projeto|50|50|250|
|Iterações |10|10|10|
|Mínimo com a etiqueta de imagens por etiqueta de classificação (50 + recomendado) |5|5|5|
|Mínimo com a etiqueta de imagens por etiqueta, deteção de objeto (50 + recomendado)|15|15|15|
|Predição quanto as imagens armazenadas|30 dias|30 dias|30 dias|
|[Predição](https://go.microsoft.com/fwlink/?linkid=865445) operações com armazenamento (transações por segundo)|2|2|10|
|[Predição](https://go.microsoft.com/fwlink/?linkid=865445) operações sem armazenamento (transações por segundo)|2|2|20|
|[TrainProject](https://go.microsoft.com/fwlink/?linkid=865446) (chamadas da API por segundo)|2|2|10|
|[Outras chamadas de API](https://go.microsoft.com/fwlink/?linkid=865446) (transações por segundo)|10|10|10|
|Tamanho máximo de imagem (carregamento da imagem de formação) |6MB|6MB|6MB|
|Tamanho máximo de imagem (predição)|4MB|4MB|4MB|

Limitações sobre *imagens de formação # por projeto* e *# etiquetas/projeto* devem ser aumentada ao longo do tempo para projetos de S0. 
