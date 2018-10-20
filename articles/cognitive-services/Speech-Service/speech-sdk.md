---
title: Sobre o SDK do serviço de voz
titleSuffix: Azure Cognitive Services
description: Uma visão geral dos SDKs disponíveis para o serviço de voz.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: wolfma
ms.openlocfilehash: f071c15ea586f7b146b82a6201adf16dadc65add
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49464816"
---
# <a name="about-the-speech-service-sdk"></a>Sobre o SDK do serviço de voz

A conversão de voz serviço Software Development Kit (SDK) dá-seus aplicativos nativo acesso para as funções do serviço de voz, tornando mais fácil desenvolver software. Atualmente, o SDK fornece acesso ao **conversão de voz em texto**, **tradução por voz**, e **reconhecimento da intenção**.

[!INCLUDE [Speech SDK Platforms](../../../includes/cognitive-services-speech-service-speech-sdk-platforms.md)]

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

## <a name="get-the-sdk"></a>Obtenha o SDK

### <a name="windows"></a>Windows

Para Windows, oferecemos suporte os idiomas seguintes:

* C# (UWP e .NET), C++: pode referenciar e usar a versão mais recente do nosso pacote NuGet do SDK de voz. O pacote inclui bibliotecas de cliente de 32 bits e 64 bits e bibliotecas de gerenciadas (.NET). O SDK pode ser instalado no Visual Studio com o NuGet. Procure **Microsoft.CognitiveServices.Speech**.

* Java: Pode referenciar e usar a versão mais recente do nosso pacote Maven do SDK de voz, que suporta apenas o Windows x64. No projeto Maven, adicione `https://csspeechstorage.blob.core.windows.net/maven/` como um repositório adicional e referência `com.microsoft.cognitiveservices.speech:client-sdk:1.0.1` como uma dependência. 

### <a name="linux"></a>Linux

> [!NOTE]
> Atualmente, suportamos apenas o Ubuntu 16.04 num PC (x86 ou x64 para desenvolvimento em C++ e x64 para .NET Core e Java).

Certifique-se de que tem o compilador necessário e as bibliotecas instaladas ao executar os seguintes comandos de shell:

```sh
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2
```

* C#: Pode referenciar e usar a versão mais recente do nosso pacote NuGet do SDK de voz. Para referenciar o SDK, adicione a seguinte referência de pacote ao seu projeto:

  ```xml
  <PackageReference Include="Microsoft.CognitiveServices.Speech" Version="1.0.1" />
  ```

* Java: Pode referenciar e usar a versão mais recente do nosso pacote Maven do SDK de voz. No projeto Maven, adicione `https://csspeechstorage.blob.core.windows.net/maven/` como um repositório adicional e referência `com.microsoft.cognitiveservices.speech:client-sdk:1.0.1` como uma dependência. 

* C++: Transferir o SDK como um [pacote. tar](https://aka.ms/csspeech/linuxbinary) e Descompacte os arquivos num diretório à sua escolha. A tabela seguinte mostra a estrutura da pasta SDK:

  |Caminho|Descrição|
  |-|-|
  |`license.md`|Licença|
  |`ThirdPartyNotices.md`|Notificações de terceiros|
  |`include`|Arquivos de cabeçalho para C e C++|
  |`lib/x64`|X64 nativo biblioteca para a ligação com a sua aplicação|
  |`lib/x86`|X86 nativo biblioteca para a ligação com a sua aplicação|

  Para criar uma aplicação, copiar ou mover os binários necessários (e as bibliotecas) em seu ambiente de desenvolvimento. Incluí-las conforme necessário no processo de compilação.

### <a name="android"></a>Android

O SDK de Java para Android é empacotado como uma [AAR (biblioteca do Android)](https://developer.android.com/studio/projects/android-library), que inclui as bibliotecas necessárias, bem como permissões Android necessárias para o utilizar. Ele está hospedado num repositório Maven na `https://csspeechstorage.blob.core.windows.net/maven/` como pacote `com.microsoft.cognitiveservices.speech:client-sdk:1.0.1`.

Para consumir o pacote do seu projeto do Android Studio, efetue as seguintes alterações:

* No ficheiro de gradle de nível de projeto, adicione o seguinte para o `repository` secção:

  ```text
  maven { url 'https://csspeechstorage.blob.core.windows.net/maven/' }
  ```

* No ficheiro de gradle de nível de módulo, adicione o seguinte para o `dependencies` secção:

  ```text
  implementation 'com.microsoft.cognitiveservices.speech:client-sdk:1.0.1'
  ```

O SDK do Java também é parte dos [SDK de dispositivos de voz](speech-devices-sdk.md).

[!INCLUDE [Get the samples](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>Passos Seguintes

* [Obter a subscrição de avaliação de Voz](https://azure.microsoft.com/try/cognitive-services/)
* [Veja como a reconhecer a conversão de voz em c#](quickstart-csharp-dotnet-windows.md)
