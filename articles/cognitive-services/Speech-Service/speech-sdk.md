---
title: Informações sobre o SDK de voz dos serviços cognitivos
description: Uma visão geral dos SDKs disponíveis para o serviço de voz.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 09/24/2018
ms.author: v-jerkin
ms.openlocfilehash: 169d421ddccf33ac239b69ab78ca7dca0f0b8261
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46958420"
---
# <a name="about-the-cognitive-services-speech-sdk"></a>Informações sobre o SDK de voz dos serviços cognitivos

O Cognitive Services voz Software Development Kit (SDK) fornece a seus aplicativos nativo acesso às funções do serviço de voz, tornando mais fácil desenvolver software. Atualmente, o SDK fornece acesso ao **conversão de voz em texto**, **tradução por voz**, e **reconhecimento da intenção**.

[!INCLUDE [Speech SDK Platforms](../../../includes/cognitive-services-speech-service-speech-sdk-platforms.md)]

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

## <a name="get-the-sdk"></a>Obtenha o SDK

### <a name="windows"></a>Windows

Para Windows, oferecemos suporte a idiomas seguintes:

* C# (UWP e .NET), C++: pode referenciar e usar a versão mais recente do nosso pacote NuGet do SDK de voz.
  O pacote inclui bibliotecas de cliente de 32 bits e 64 bits, bem como bibliotecas de gerenciadas (.NET).
  O SDK pode ser instalado no Visual Studio usando o NuGet; Basta procurar `Microsoft.CognitiveServices.Speech`.

* Java: Pode referenciar e usar a versão mais recente do nosso pacote Maven do SDK de voz, que suporta apenas o Windows x64.
  No projeto Maven, adicione `https://csspeechstorage.blob.core.windows.net/maven/` como um repositório adicional e referência `com.microsoft.cognitiveservices.speech:client-sdk:1.0.0` como uma dependência. 

### <a name="linux"></a>Linux

> [!NOTE]
> Atualmente, apenas suportamos o Ubuntu 16.04 um PC (x86 ou x64 para desenvolvimento em C++, x64 para .NET Core e Java).

Certifique-se de que tem o compilador necessário e as bibliotecas instaladas ao executar os seguintes comandos de shell:

```sh
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2
```

* C#: Pode referenciar e usar a versão mais recente do nosso pacote NuGet do SDK de voz.
  Para referenciar o SDK, adicione a seguinte referência de pacote para o seu projeto:

  ```xml
  <PackageReference Include="Microsoft.CognitiveServices.Speech" Version="1.0.0" />
  ```

* Java: Pode referenciar e usar a versão mais recente do nosso pacote Maven do SDK de voz.
  No projeto Maven, adicione `https://csspeechstorage.blob.core.windows.net/maven/` como um repositório adicional e referência `com.microsoft.cognitiveservices.speech:client-sdk:1.0.0` como uma dependência. 

* C++: Transferir o SDK como um [pacote. tar](https://aka.ms/csspeech/linuxbinary) e Descompacte os arquivos num diretório à sua escolha. A tabela seguinte mostra a estrutura de pastas do SDK.

  |Caminho|Descrição|
  |-|-|
  |`license.md`|Licença|
  |`ThirdPartyNotices.md`|Anúncios de terceiros|
  |`include`|Arquivos de cabeçalho para C e C++|
  |`lib/x64`|X64 nativo biblioteca para a ligação com a sua aplicação|
  |`lib/x86`|X86 nativo biblioteca para a ligação com a sua aplicação|

  Para criar uma aplicação, copiar ou mover os binários necessários (e as bibliotecas) para o ambiente de desenvolvimento e incluí-las conforme necessário em seu processo de compilação.

### <a name="android"></a>Android

O SDK de Java para Android é empacotado como uma [AAR (biblioteca do Android)](https://developer.android.com/studio/projects/android-library), que inclui as bibliotecas necessárias, bem como permissões Android necessárias para o utilizar.
Ele está hospedado num repositório Maven na `https://csspeechstorage.blob.core.windows.net/maven/` como pacote `com.microsoft.cognitiveservices.speech:client-sdk:1.0.0`.
Para consumir o pacote do seu projeto do Android Studio, efetue as seguintes alterações:

* No nível do projeto `build.gradle` de ficheiros, adicione o seguinte para o `repository` secção:

  ```text
  maven { url 'https://csspeechstorage.blob.core.windows.net/maven/' }
  ```

* No nível do módulo `build.gradle` de ficheiros, adicione o seguinte para o `dependencies` secção:

  ```text
  implementation 'com.microsoft.cognitiveservices.speech:client-sdk:1.0.0'
  ```

O SDK do Java também é parte dos [SDK de dispositivos de voz](speech-devices-sdk.md).

[!INCLUDE [Get the samples](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>Passos Seguintes

* [Obter a subscrição de avaliação de Voz](https://azure.microsoft.com/try/cognitive-services/)
* [Veja como a reconhecer a conversão de voz em c#](quickstart-csharp-dotnet-windows.md)
