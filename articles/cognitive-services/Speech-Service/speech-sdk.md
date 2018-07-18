---
title: Informações sobre o SDK de voz dos serviços cognitivos | Documentos da Microsoft
description: Uma visão geral dos SDKs disponíveis para o serviço de voz.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/17/2018
ms.author: v-jerkin
ms.openlocfilehash: ed523493f456e65f7aa5d3ad33914e3e52cd7044
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39113397"
---
# <a name="about-the-cognitive-services-speech-sdk"></a>Informações sobre o SDK de voz dos serviços cognitivos

O Cognitive Services voz Software Development Kit (SDK) fornece a seus aplicativos nativo acesso às funções do serviço de voz, tornando mais fácil desenvolver software. Atualmente, o SDK fornece acesso ao **conversão de voz em texto**, **tradução por voz**, e **reconhecimento da intenção**.

[!include[Speech SDK Platforms](../../../includes/cognitive-services-speech-service-speech-sdk-platforms.md)]

[!include[License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

## <a name="get-the-windows-sdk"></a>Obtenha o SDK do Windows

A versão do Windows do SDK de voz inclui bibliotecas de cliente de C/C++ de 32 bits e 64 bits, bem como as bibliotecas gerenciadas de (.NET) para utilização com o c#. O SDK pode ser instalado no Visual Studio usando o NuGet; Basta procurar `Microsoft.CognitiveServices.Speech`.

## <a name="get-the-linux-sdk"></a>Obtenha o SDK do Linux

Certifique-se de que tem o compilador necessário e as bibliotecas executando os seguintes comandos de shell:

```sh
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2
```

> [!NOTE]
> Estas instruções partem do princípio de que está a executar o Ubuntu 16.04 num PC (x86 ou x64). Numa versão diferente do Ubuntu ou uma distribuição Linux diferente, adapte estes passos para seu ambiente.

Em seguida, [transferir o SDK](https://aka.ms/csspeech/linuxbinary) e Descompacte os arquivos num diretório à sua escolha. Esta tabela mostra a estrutura de pastas do SDK.

|Caminho|Descrição|
|-|-|
|`license.md`|Licença|
|`third-party-notices.md`|Anúncios de terceiros|
|`include`|Arquivos de cabeçalho para C e C++|
|`lib/x64`|X64 nativo biblioteca para a ligação com a sua aplicação|
|`lib/x86`|X86 nativo biblioteca para a ligação com a sua aplicação|

Para criar uma aplicação, copiar ou mover os binários necessários (e as bibliotecas) para o ambiente de desenvolvimento e incluí-las conforme necessário em seu processo de compilação.

## <a name="get-the-java-sdk"></a>Obtenha o Java SDK

O SDK de Java para Android é empacotado como uma [AAR (biblioteca do Android)](https://developer.android.com/studio/projects/android-library), que inclui as bibliotecas necessárias, bem como permissões Android necessárias para o utilizar.
Ele está hospedado num repositório Maven na `https://csspeechstorage.blob.core.windows.net/maven/` como pacote `com.microsoft.cognitiveservices.speech:client-sdk:0.5.0`.
O consume o pacote do seu projeto do Android Studio, efetue as seguintes alterações:

* No nível do projeto `build.gradle` de ficheiros, adicione o seguinte para o `repository` secção:

  ```text
  maven { url 'https://csspeechstorage.blob.core.windows.net/maven/' }
  ```

* No nível do módulo `build.gradle` de ficheiros, adicione o seguinte para o `dependencies` secção:

  ```text
  implementation 'com.microsoft.cognitiveservices.speech:client-sdk:0.5.0'
  ```

O SDK do Java também é parte dos [SDK de dispositivos de voz](speech-devices-sdk.md).

## <a name="next-steps"></a>Passos Seguintes

* [Obter a subscrição de avaliação de Voz](https://azure.microsoft.com/try/cognitive-services/)
* [Veja como a reconhecer a conversão de voz em c#](quickstart-csharp-dotnet-windows.md)
