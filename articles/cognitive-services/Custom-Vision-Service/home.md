---
title: Descrição geral da aprendizagem automática do Serviço de Visão Personalizada - Serviços Cognitivos do Azure | Microsoft Docs
description: O Serviço de Visão Personalizada é um Serviço Cognitivo da Microsoft que lhe permite criar classificadores de imagem personalizada na plataforma do Azure.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: overview
ms.date: 05/02/2018
ms.author: anroth
ms.openlocfilehash: de45fc399470a806fb7456cbbe936cecf659ee7c
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/28/2018
ms.locfileid: "37087800"
---
# <a name="what-is-the-custom-vision-service"></a>O que é o Serviço de Visão Personalizada?

O Serviço de Visão Personalizada é um Serviço Cognitivo da Microsoft que lhe permite criar classificadores de imagem personalizada. Este serviço facilita a rápida criação, implementação e melhoria de um classificador de imagem. O Serviço de Visão Personalizada oferece uma API REST e uma interface Web para carregar as suas imagens e preparar o classificador.

## <a name="what-does-custom-vision-service-do-well"></a>O que o Serviço de Visão Personalizada faz bem?

O Serviço de Visão Personalizada funciona melhor quando o item que está a tentar classificar é proeminente na sua imagem. 

Algumas imagens são precisas para criar um classificador ou detetor. São suficientes 50 imagens por classe para iniciar o seu protótipo. Os métodos que o Serviço de Visão Personalizada utiliza são robustos às diferenças, o que lhe permite iniciar a criação de protótipos com poucos dados. Ou seja, o Serviço de Visão Personalizada não é adequado a cenários em que pretende detetar diferenças sutis. Por exemplo, pequenas diferenças em cenários de garantia de qualidade.

## <a name="release-notes"></a>Notas de Versão

### <a name="may-7-2018"></a>7 de maio de 2018
- Introduziu a funcionalidade de Deteção de Objeto de pré-visualização para projetos de Versão de Avaliação Limitada.
- Atualizar para APIs 2.0
- Escalão S0 expandido até 250 etiquetas e 50.000 imagens. 
- Melhorias de back-end significativas para o pipeline de aprendizagem automática para projetos de classificação de imagem. Os projetos que foram preparados depois de 27 de Abril de 2018 irão beneficiar destas atualizações.
- Exportação de modelo adicionado a ONNX, para utilização com o Windows ML.
- Exportação de modelo adicionado ao Dockerfile. Isto permite-lhe transferir os artefactos para criar os seus próprios contentores do Windows ou Linux, incluindo um DockerFile, modelo do TensorFlow e código de serviço. 
- Para modelos recentemente preparados exportados para o TensorFlow nos Domínios Geral (Compacto) e de Ponto de Referência (Compacto), [os Valores Médios são agora (0,0,0)](https://github.com/azure-samples/cognitive-services-android-customvision-sample), para manter a consistência em todos os projetos. 

### <a name="march-1-2018"></a>1 de março de 2018
- Pré-visualização paga introduzida e carregada no Portal do Azure. Os projetos podem agora ser anexados a recursos do Azure com um escalão F0 (Gratuito) ou S0 (Standard). Os projetos de escalão S0 foram introduzidos, o que permite um máximo de 100 etiquetas e 25.000 imagens. 
- Back-end muda para o parâmetro de pipeline/normalização de aprendizagem automática. Isto dará aos clientes um melhor controlo do equilíbrio de revocação de precisão ao ajustar o Limiar de Probabilidade. Como parte destas alterações, o Limiar de Probabilidade predefinido no portal CustomVision.ai foi definido como 50%.

### <a name="december-19-2017"></a>19 de dezembro de 2017

- Exportar para o Android (TensorFlow) adicionado, além da exportação lançada anteriormente para iOS (CoreML.) Isto permite que a exportação de um modelo compacto preparado seja executado offline numa aplicação.
- Domínios "compactos" de Retalho e de Ponto de Referência adicionados para permitir a exportação de modelos para estes domínios.
- Versão de lançamento [1.2 API de Preparação](https://southcentralus.dev.cognitive.microsoft.com/docs/services/f2d62aa3b93843d79e948fe87fa89554/operations/5a3044ee08fa5e06b890f11f) e [1.1 API de Predição](https://southcentralus.dev.cognitive.microsoft.com/docs/services/57982f59b5964e36841e22dfbfe78fc1/operations/5a3044f608fa5e06b890f164). As APIs atualizadas suportam a exportação de modelo, uma nova operação de Predição que não guarda imagens em "Predições" e introduziu operações em lote na API de Preparação.
- Ajustes na experiência do utilizador, incluindo a capacidade de ver qual foi o domínio utilizado para preparar uma iteração.
- [Exemplo e SDK de C#](https://github.com/Microsoft/Cognitive-CustomVision-Windows) atualizado.

## <a name="next-steps"></a>Passos seguintes

[Saiba como compilar um classificador](getting-started-build-a-classifier.md)
