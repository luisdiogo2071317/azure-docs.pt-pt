---
title: Serviços cognitivos documentação do SDK de reconhecimento de voz | Microsoft Docs
description: Notas de versão - o que mudou nas versões mais recentes
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: 0b1559d288380cf3d0c180a225278cc13d22a5d0
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "35356303"
---
# <a name="release-notes"></a>Notas de versão

## <a name="cognitive-services-speech-sdk-040-2018-june-release"></a>Cognitivos SDK dos serviços de reconhecimento de voz 0.4.0: versão de Junho de 2018

**Alterações funcionais**

- AudioInputStream

  Um reconhecedor agora pode consumir uma transmissão em fluxo como a origem de áudio. Para obter informações detalhadas, consulte o relacionados [guia de procedimentos](how-to-use-audio-input-streams.md).

- Formato de saída detalhada

  Ao criar um `SpeechRecognizer`, pode pedir `Detailed` ou `Simple` formato de saída. O `DetailedSpeechRecognitionResult` contém uma pontuação de confiança, texto reconhecido, formato lexical não processado, formulário normalizado e formulário normalizado com profanity mascarada.

**Alteração de última hora**

- Alterar para `SpeechRecognitionResult.Text` de `SpeechRecognitionResult.RecognizedText` em c#.

**Correções de erros**

- Corrigir um problema de chamada de retorno possíveis na camada USP durante o encerramento.

- Se um reconhecedor consumido um ficheiro de entrada de áudio, foi premir para o identificador de ficheiro maior do que o necessário.

- Remover impasses várias entre bomba de mensagem e reconhecedor.

- Acionar um `NoMatch` resultar quando a resposta do serviço é excedida.

- As bibliotecas de foundation de suporte de dados no Windows são carregadas em atraso. Esta biblioteca é apenas necessário para a entrada do microfone.

- A velocidade de carregamento de dados de áudio está limitada a sobre duas vezes a velocidade de áudio original.

- No Windows, as assemblagens c# .NET são agora com nome seguro.

- Correção de documentação: `Region` informações são necessárias para criar um reconhecedor.

Mais exemplos foram adicionados e estão constantemente a ser atualizados. Para o conjunto mais recente de exemplos, consulte o [repositório do GitHub de exemplo do SDK de reconhecimento de voz](https://aka.ms/csspeech/samples).

## <a name="cognitive-services-speech-sdk-0212733-2018-may-release"></a>Cognitivos SDK dos serviços de reconhecimento de voz 0.2.12733: versão de Maio de 2018

A primeira versão de pré-visualização pública do SDK de reconhecimento de voz serviços cognitivos.
