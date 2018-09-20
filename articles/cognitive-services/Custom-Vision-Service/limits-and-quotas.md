---
title: Limites e quotas - serviço de visão personalizada
titlesuffix: Azure Cognitive Services
description: Saiba mais sobre os limites e quotas para o serviço de visão personalizada.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: anroth
ms.openlocfilehash: 53884cfc2683832d9df2b215c92b6b683ba29d2c
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46363468"
---
# <a name="limits-and-quotas"></a>Limites e quotas

Existem três camadas de chaves para o serviço de visão personalizada. Recursos F0 e S0 são obtidos através do portal do Azure. Detalhes sobre as definições de preços e as transações são sobre o [página de preços](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/).  F0 projetos podem ser atualizados para projetos de S0.

Recursos de projeto de avaliação limitados estão ligados ao seu início de sessão de visão personalizada (ou seja, uma conta do AAD ou conta MSA.) Eles se destinam a ser utilizado para avaliações abreviadas do serviço.  As contas criadas durante a pré-visualização gratuita antecipada, antes da introdução das pré-visualizações do Azure (1 de Março de 2018) irão manter as respetivas quotas anteriores para as versões de avaliação limitado. 

||**Versão de avaliação limitada**|**F0 (Azure)**|**S0 (Azure)**|
|-----|-----|-----|-----|
|Projetos|2|2|100|
|Imagens de formação por projeto|5.000|5.000|50,000|
|Predições / mês|10,000 |10,000|Ilimitado|
|Com as etiquetas /Project|50|50|250|
|Iterações |10|10|10|
|Mínimo rotulado como imagens por etiqueta de classificação (mais de 50 recomendado) |5|5|5|
|Mínimo rotulado como imagens por marca, deteção de objetos (mais de 50 recomendado)|15|15|15|
|O tempo em que as imagens de predição são armazenadas|30 dias|30 dias|30 dias|
|[Predição](https://go.microsoft.com/fwlink/?linkid=865445) operações com o armazenamento (transações por segundo)|2|2|10|
|[Predição](https://go.microsoft.com/fwlink/?linkid=865445) operações sem armazenamento (transações por segundo)|2|2|20|
|[TrainProject](https://go.microsoft.com/fwlink/?linkid=865446) (chamadas à API por segundo)|2|2|10|
|[Outras chamadas de API](https://go.microsoft.com/fwlink/?linkid=865446) (transações por segundo)|10|10|10|
|Tamanho máximo de imagem (carregamento de imagens de formação) |6MB|6MB|6MB|
|Tamanho máximo de imagem (predição)|4MB|4MB|4MB|

Limitações sobre *imagens n. º de formação por projeto* e *# etiquetas/projeto* devem ser aumentados ao longo do tempo para projetos de S0. 
