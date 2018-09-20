---
title: Notas de versão – serviço de visão personalizada
titlesuffix: Azure Cognitive Services
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: conceptual
ms.date: 08/28/2018
ms.author: anroth
ms.openlocfilehash: 61fa0b1d2fc40daed8b3b99a0ab0d36de802ce3d
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46367989"
---
# <a name="custom-vision-service-release-notes"></a>Notas de versão do serviço de visão personalizada

## <a name="august-14-2018"></a>14 de Agosto de 2018
- Foi adicionado widget "Iniciar" para o site de customvision.ai orientar os usuários por meio de treinamento do project. 
- Ainda mais melhorias para o pipeline de machine learning para beneficiar multilabel projetos (nova camada de perda).

## <a name="june-28-2018"></a>28 de Junho de 2018
- Melhorias de Bugfixes & back-end.
- Classificação de várias classes de Enabeled, para projetos em que as imagens têm exatamente uma etiqueta. Somam em previsões para o modo de várias classes, probabilidades para um (todas as imagens são classificadas entre as suas etiquetas especificadas).

## <a name="june-13-2018"></a>13 de junho de 2018
- Atualização da experiência do Usuário, se concentrou na facilidade de utilização e acessibilidade. 
- Melhorias para o pipeline de aprendizagem para se beneficiar multilabel projetos com um grande número de etiquetas.
- Foi corrigido o erro na exportação do TensorFlow. Enabeled exportados de controle de versão do modelo, para que as iterações podem ser exportadas a mais de uma vez. 

## <a name="may-7-2018"></a>7 de maio de 2018
- Introduziu a funcionalidade de Deteção de Objeto de pré-visualização para projetos de Versão de Avaliação Limitada.
- Atualizar para APIs 2.0
- Escalão S0 expandido até 250 etiquetas e 50.000 imagens. 
- Melhorias de back-end significativas para o pipeline de aprendizagem automática para projetos de classificação de imagem. Os projetos que foram preparados depois de 27 de Abril de 2018 irão beneficiar destas atualizações.
- Exportação de modelo adicionado a ONNX, para utilização com o Windows ML.
- Exportação de modelo adicionado ao Dockerfile. Isto permite-lhe transferir os artefactos para criar os seus próprios contentores do Windows ou Linux, incluindo um DockerFile, modelo do TensorFlow e código de serviço. 
- Para modelos recentemente preparados exportados para o TensorFlow nos Domínios Geral (Compacto) e de Ponto de Referência (Compacto), [os Valores Médios são agora (0,0,0)](https://github.com/azure-samples/cognitive-services-android-customvision-sample), para manter a consistência em todos os projetos. 

## <a name="march-1-2018"></a>1 de março de 2018
- Pré-visualização paga introduzida e carregada no Portal do Azure. Os projetos podem agora ser anexados a recursos do Azure com um escalão F0 (Gratuito) ou S0 (Standard). Os projetos de escalão S0 foram introduzidos, o que permite um máximo de 100 etiquetas e 25.000 imagens. 
- Back-end muda para o parâmetro de pipeline/normalização de aprendizagem automática. Isto dará aos clientes um melhor controlo do equilíbrio de revocação de precisão ao ajustar o Limiar de Probabilidade. Como parte destas alterações, o Limiar de Probabilidade predefinido no portal CustomVision.ai foi definido como 50%.

## <a name="december-19-2017"></a>19 de dezembro de 2017

- Exportar para o Android (TensorFlow) adicionado, além da exportação lançada anteriormente para iOS (CoreML.) Isto permite que a exportação de um modelo compacto preparado seja executado offline numa aplicação.
- Domínios "compactos" de Retalho e de Ponto de Referência adicionados para permitir a exportação de modelos para estes domínios.
- Versão de lançamento [1.2 API de Preparação](https://southcentralus.dev.cognitive.microsoft.com/docs/services/f2d62aa3b93843d79e948fe87fa89554/operations/5a3044ee08fa5e06b890f11f) e [1.1 API de Predição](https://southcentralus.dev.cognitive.microsoft.com/docs/services/57982f59b5964e36841e22dfbfe78fc1/operations/5a3044f608fa5e06b890f164). As APIs atualizadas suportam a exportação de modelo, uma nova operação de Predição que não guarda imagens em "Predições" e introduziu operações em lote na API de Preparação.
- Ajustes na experiência do utilizador, incluindo a capacidade de ver qual foi o domínio utilizado para preparar uma iteração.
- [Exemplo e SDK de C#](https://github.com/Microsoft/Cognitive-CustomVision-Windows) atualizado.

