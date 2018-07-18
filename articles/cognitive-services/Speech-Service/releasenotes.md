---
title: Documentação do SDK de voz de serviços cognitivos | Documentos da Microsoft
description: Notas de versão – o que foi alterado nas versões mais recentes
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/17/2018
ms.author: wolfma
ms.openlocfilehash: 50a8c183bd7f2711847ce6d0acade4cb498ef2fc
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39116100"
---
# <a name="release-notes"></a>Notas de versão

## <a name="cognitive-services-speech-sdk-050-2018-july-release"></a>SDK de voz dos serviços cognitivos versão 0.5.0: versão de Julho de 2018

**Novos recursos**

* Plataforma Android de suporte (API 23: Android 6.0 Marshmallow ou superior).
  Veja a [guia de introdução Android](quickstart-java-android.md).
* Suporta o .NET Standard 2.0 no Windows.
  Veja a [guia de introdução do .NET Core](quickstart-csharp-dotnetcore-windows.md).
* Experimental: Suporte UWP no Windows (versão 1709 ou posterior)
  * Confira nosso [guia de introdução do UWP](quickstart-csharp-uwp.md).
  * Nota: As aplicações UWP compiladas com o SDK de voz não ainda passam o Kit de certificação de aplicações de Windows (WACK).
* Suporte a reconhecimento de execução longa com o restabelecimento de ligação automática.

**Alterações funcionais**

* `StartContinuousRecognitionAsync()` oferece suporte a reconhecimento de execução longa
* O resultado do reconhecimento contém mais campos: deslocamento de início de áudio e duração (ambos em tiques) do texto reconhecido, valores adicionais que representa o estado de reconhecimento, por exemplo, `InitialSilenceTimeout`, `InitialBabbleTimeout`.
* AuthorizationToken de suporte para a criação de instâncias de fábrica.

**Alterações recentes**

* Eventos de reconhecimento: tipo de evento NoMatch é intercalado com o evento de erro.
* SpeechOutputFormat na linguagem c# foi mudado para OutputFormat manter alinhado com o C++.
* O tipo de retorno de alguns métodos do `AudioInputStream` interface ligeiramente alterada:
   * No Java, o `read` método agora devolve `long` em vez de `int`.
   * No c#, o `Read` método agora devolve `uint` em vez de `int`.
   * No C++, o `Read` e `GetFormat` métodos agora retorno `size_t` em vez de `int`.
* C++: instâncias de fluxos de entrada de áudio podem agora apenas ser passadas como um `shared_ptr`.

**Correções de erros**

* Fixo de valores de retorno incorreto no resultado quando `RecognizeAsync()` exceder o tempo limite.
* A dependência de bibliotecas de base de dados no Windows é removida. O SDK agora está a utilizar APIs de áudio de núcleo.
* Correção da documentação: adicionar uma página de região para descrever o que são as regiões suportadas.

**Problemas conhecidos**

* O SDK de voz para Android não reporta os resultados de síntese de fala para a tradução.
  Isso será corrigido na próxima versão.

## <a name="cognitive-services-speech-sdk-040-2018-june-release"></a>SDK de voz dos serviços cognitivos 0.4.0: versão de Junho de 2018

**Alterações funcionais**

- AudioInputStream

  Um reconhecedor agora pode utilizar um fluxo como a origem de áudio. Para obter informações detalhadas, consulte o relacionados [guia de procedimentos](how-to-use-audio-input-streams.md).

- Formato de saída detalhada

  Ao criar um `SpeechRecognizer`, pode pedir `Detailed` ou `Simple` formato de saída. O `DetailedSpeechRecognitionResult` contém uma pontuação de confiança, texto reconhecido, formato léxico não processado, formato normalizado e formato normalizado com linguagem inapropriada mascarada.

**Alteração significativa**

- Alterar para `SpeechRecognitionResult.Text` partir `SpeechRecognitionResult.RecognizedText` em c#.

**Correções de erros**

- Corrigi um problema de possíveis de retorno de chamada na camada USP durante o encerramento.

- Se um reconhecedor consumido um ficheiro de entrada de áudio, ele foi manter o identificador de ficheiro maior do que o necessário.

- Remover vários deadlocks entre o bombardeamento de mensagens e o reconhecedor.

- Acionar um `NoMatch` quando a resposta do serviço é excedida.

- As bibliotecas de base de dados no Windows são carregado com atraso. Esta biblioteca é apenas necessário para a entrada do microfone.

- A velocidade de carregamento de dados de áudio é limitada a sobre duas vezes a velocidade de áudio original.

- No Windows, os assemblies de c# .NET são agora nome forte.

- Correção da documentação: `Region` informações são necessárias para criar um reconhecedor.

Mais exemplos foram adicionados e estão constantemente a ser atualizados. Para o conjunto mais recente de exemplos, consulte a [repositório do GitHub de exemplo do SDK de voz](https://aka.ms/csspeech/samples).

## <a name="cognitive-services-speech-sdk-0212733-2018-may-release"></a>SDK de voz dos serviços cognitivos 0.2.12733: versão de Maio de 2018

A primeira versão de pré-visualização pública do SDK de voz dos serviços cognitivos.
