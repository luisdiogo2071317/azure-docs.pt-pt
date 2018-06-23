---
title: Sobre o reconhecimento de voz serviços cognitivos SDK | Microsoft Docs
description: Uma descrição geral dos SDKs disponíveis para o serviço de reconhecimento de voz.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: b9b7b8af5ce3d75788fd2c4f5e0309b5ca561a8f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355646"
---
# <a name="about-the-cognitive-services-speech-sdk"></a>Sobre o SDK de voz serviços cognitivos

O cognitivos serviços voz Software Development Kit (SDK) fornece as suas aplicações nativo acesso para as funções do serviço de reconhecimento de voz, tornando mais fácil desenvolver software. Atualmente, o SDK fornece acesso ao **reconhecimento de voz para texto**, **tradução de reconhecimento de voz**, e **intenção reconhecimento**.

A tabela lista os sistemas operativos e atualmente linguagens de programação.

|Sistema operativo suportado|Linguagem de programação|
|-|-|
|Windows|C/C++, C#|
|Linux|C/C++|
|Dispositivos|Java\*|

\* *O SDK de Java faz parte o [SDK de dispositivos de reconhecimento de voz](speech-devices-sdk.md).*

## <a name="get-the-windows-sdk"></a>Obter o Windows SDK

A versão do Windows do SDK de reconhecimento de voz inclui bibliotecas de cliente do C/C++ 32 bits e 64 bits, bem como geridas (.NET) bibliotecas para utilização com c#. O SDK pode ser instalado no Visual Studio com NuGet; Basta procurar `Microsoft.CognitiveServices.Speech`.

## <a name="get-the-linux-sdk"></a>Obter o SDK Linux

Certifique-se de que o compilador necessária e bibliotecas executando os seguintes comandos shell:

```sh
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2
```

> [!NOTE]
> Estas instruções partem do princípio de que está a executar Ubuntu 16.04 num PC (x86 ou x64). Uma versão diferente do Ubuntu, ou uma distribuição Linux diferente, adapte estes passos para o seu ambiente.

Em seguida, [transferir o SDK](https://aka.ms/csspeech/linuxbinary) e descompactar os ficheiros para um diretório à sua escolha. Esta tabela mostra a estrutura da pasta SDK.

|Caminho|Descrição|
|-|-|
|`license.md`|Licença|
|`third-party-notices.md`|Anúncios de terceiros|
|`include`|Ficheiros de cabeçalho para C e C++|
|`lib/x64`|X64 nativo biblioteca para ligar com a sua aplicação|
|`lib/x86`|X86 nativo biblioteca para ligar com a sua aplicação|

Para criar uma aplicação, copiar ou mover os binários necessários (e as bibliotecas) para o seu ambiente de desenvolvimento e inclui-las como necessários para o processo de compilação.

## <a name="get-the-java-sdk"></a>Obter o Java SDK

O SDK de Java faz parte o [SDK de dispositivos de reconhecimento de voz](speech-devices-sdk.md).

## <a name="next-steps"></a>Passos Seguintes

* [Obter a sua subscrição de avaliação de reconhecimento de voz](https://azure.microsoft.com/try/cognitive-services/)
* [Ver como reconhecer voz em c#](quickstart-csharp-windows.md)
