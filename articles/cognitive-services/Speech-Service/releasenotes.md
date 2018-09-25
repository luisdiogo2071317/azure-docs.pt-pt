---
title: Documentação do SDK de voz de serviços cognitivos | Documentos da Microsoft
description: Notas de versão – o que foi alterado nas versões mais recentes
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: cfc8f973ac9bced9396a081f7fb2ab6a4a1b2f7f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46963088"
---
# <a name="release-notes"></a>Notas de versão

## <a name="cognitive-services-speech-sdk-100-2018-september-release"></a>SDK de voz dos serviços cognitivos 1.0.0: versão de Setembro de 2018

**Novos recursos**

* Suporte para Objective-C no iOS. Confira nosso [início rápido de Objective-C para iOS](quickstart-objectivec-ios.md).
* Suporte para JavaScript no browser. Confira nosso [guia de introdução do JavaScript](quickstart-js-browser.md).

**Alterações recentes**

* Com esta versão é introduzido um número de alterações de última hora.
  Verifique se existem [esta página](https://aka.ms/csspeech/breakingchanges_1_0_0) para obter detalhes.

## <a name="cognitive-services-speech-sdk-060-2018-august-release"></a>SDK de voz dos serviços cognitivos 0.6.0: versão de Agosto de 2018

**Novos recursos**

* Aplicações do UWP criadas com o SDK de voz, agora podem passar o Kit de certificação de aplicações de Windows (WACK).
  Veja a [guia de introdução do UWP](quickstart-csharp-uwp.md).
* Suporte para .NET Standard 2.0 no Linux (Ubuntu 16.04 x64).
* Experimental: Suportar Java 8 (64-bit) do Windows e Linux (Ubuntu 16.04 x64).
  Veja a [início rápido de Java Runtime Environment](quickstart-java-jre.md).

**Alteração funcional**

* Expor informações de detalhes de erro adicionais sobre os erros de ligação.

**Alterações recentes**

* No Java (Android), o `SpeechFactory.configureNativePlatformBindingWithDefaultCertificate` função já não necessita de um parâmetro de caminho. Agora o caminho é detetado automaticamente em todas as plataformas suportadas.
* O acessador get da propriedade `EndpointUrl` em Java e c# foi removido.

**Correções de erros**

* No Java, o resultado de síntese de áudio no reconhecedor de tradução é agora implementado.
* Foi corrigido um erro que pode causar threads Inativas e um número maior de soquetes abertos e não utilizados.
* Foi corrigido um problema, onde um reconhecimento de longa execução foi possível terminar no meio da transmissão.
* Fixo, uma condição de corrida em encerramento do reconhecedor.

## <a name="cognitive-services-speech-sdk-050-2018-july-release"></a>SDK de voz dos serviços cognitivos versão 0.5.0: versão de Julho de 2018

**Novos recursos**

* Plataforma Android de suporte (API 23: Android 6.0 Marshmallow ou superior). Veja a [guia de introdução Android](quickstart-java-android.md).
* Suporta o .NET Standard 2.0 no Windows. Veja a [guia de introdução do .NET Core](quickstart-csharp-dotnetcore-windows.md).
* Experimental: Suporte UWP no Windows (versão 1709 ou posterior).
  * Veja a [guia de introdução do UWP](quickstart-csharp-uwp.md).
  * Nota: As aplicações UWP compiladas com o SDK de voz não ainda passam o Kit de certificação de aplicações de Windows (WACK).
* Suporte a reconhecimento de execução longa com a nova ligação automática.

**Alterações funcionais**

* `StartContinuousRecognitionAsync()` oferece suporte a reconhecimento de execução longa.
* O resultado do reconhecimento contém mais campos. Eles estão deslocamento do início de áudio e a duração (ambos em tiques) do texto reconhecido e valores adicionais que representam o estado de reconhecimento, por exemplo, `InitialSilenceTimeout` e `InitialBabbleTimeout`.
* AuthorizationToken de suporte para a criação de instâncias de fábrica.

**Alterações recentes**

* Eventos de reconhecimento: tipo de evento NoMatch foi intercalado o evento de erro.
* SpeechOutputFormat na linguagem c# foi mudado para OutputFormat para permanecer alinhado com o C++.
* O tipo de retorno de alguns métodos do `AudioInputStream` interface ligeiramente alterado:
   * No Java, o `read` método agora devolve `long` em vez de `int`.
   * No c#, o `Read` método agora devolve `uint` em vez de `int`.
   * No C++, o `Read` e `GetFormat` métodos agora retorno `size_t` em vez de `int`.
* C++: As instâncias de entrada dos fluxos de áudio agora podem ser transmitidas apenas como um `shared_ptr`.

**Correções de erros**

* Fixo de valores de retorno incorreto no resultado quando `RecognizeAsync()` exceder o tempo limite.
* A dependência de bibliotecas de base de dados no Windows foi removida. O SDK utiliza agora as APIs de áudio de núcleo.
* Correção da documentação: adicionado uma [regiões](regions.md) página para descrever as regiões suportadas.

**Problema conhecido**

* O SDK de voz para Android não reporta os resultados de síntese de fala para a tradução. Este problema será corrigido na próxima versão.

## <a name="cognitive-services-speech-sdk-040-2018-june-release"></a>SDK de voz dos serviços cognitivos 0.4.0: versão de Junho de 2018

**Alterações funcionais**

- AudioInputStream

  Um reconhecedor agora, pode utilizar um fluxo como a origem de áudio. Para obter mais informações, consulte o relacionados [guia de procedimentos](how-to-use-audio-input-streams.md).

- Formato de saída detalhada

  Quando cria um `SpeechRecognizer`, pode pedir `Detailed` ou `Simple` formato de saída. O `DetailedSpeechRecognitionResult` contém uma pontuação de confiança, texto reconhecido, formato léxico não processado, formato normalizado e formato normalizado com linguagem inapropriada mascarada.

**Alteração significativa**

- Alterado para `SpeechRecognitionResult.Text` partir `SpeechRecognitionResult.RecognizedText` em c#.

**Correções de erros**

- Foi corrigido um problema de possíveis de retorno de chamada na camada de USP durante o encerramento.

- Se um reconhecedor consumido um ficheiro de entrada de áudio, ele foi manter o identificador de ficheiro maior do que o necessário.

- Remover vários deadlocks entre o bombardeamento de mensagens e o reconhecedor.

- Acionar um `NoMatch` quando a resposta do serviço é excedida.

- As bibliotecas de base de dados no Windows são carregado de atraso. Esta biblioteca é necessária para apenas a entrada do microfone.

- A velocidade de carregamento de dados de áudio é limitada a sobre duas vezes a velocidade de áudio original.

- No Windows, assemblies do .NET c# agora são strong nomeados.

- Correção da documentação: `Region` informações são necessárias para criar um reconhecedor.

Mais exemplos foram adicionados e estão constantemente a ser atualizados. Para o conjunto mais recente de exemplos, consulte a [repositório de GitHub de exemplos do SDK de voz](https://aka.ms/csspeech/samples).

## <a name="cognitive-services-speech-sdk-0212733-2018-may-release"></a>SDK de voz dos serviços cognitivos 0.2.12733: versão de Maio de 2018

Esta versão é a primeira versão de pré-visualização pública do SDK de voz dos serviços cognitivos.
