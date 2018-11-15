---
title: Transmitir ficheiros de vídeo com os Serviços de Multimédia do Azure – .NET | Microsoft Docs
description: Siga os passos deste início rápido para criar uma nova conta dos Serviços de Multimédia do Azure, codificar um ficheiro e transmiti-lo para o Leitor de Multimédia do Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
keywords: serviços de multimédia do azure, transmitir
ms.service: media-services
ms.workload: media
ms.topic: quickstart
ms.custom: mvc
ms.date: 11/11/2018
ms.author: juliako
ms.openlocfilehash: fc8fc1af51332df032e864c84791791a38bc8601
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/13/2018
ms.locfileid: "51612225"
---
# <a name="quickstart-stream-video-files---net"></a>Início Rápido: Transmitir ficheiros de vídeo – .NET

Este início rápido mostra como é fácil codificar e começar a transmitir vídeos numa ampla variedade de browsers e dispositivos com os Serviços de Multimédia do Azure. Um conteúdo de entrada pode ser especificado com URLs HTTPS, URLs SAS ou caminhos de ficheiros localizados no Armazenamento de blobs do Azure.
O exemplo neste tópico codifica o conteúdo que torna acessível através de um URL HTTPS. Note que, atualmente, o AMS v3 não suporta a codificação de transferência segmentada através de URLs HTTPS.

No final do início rápido, será capaz de transmitir um vídeo.  

![Reproduzir o vídeo](./media/stream-files-dotnet-quickstart/final-video.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

- Se não tiver o Visual Studio instalado, poderá obter o [Visual Studio Community 2017](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15).
- Instalar e utilizar a CLI localmente, este artigo requer a versão 2.0 ou posterior da CLI do Azure. Execute `az --version` para localizar a versão atual. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure](/cli/azure/install-azure-cli). 

    Atualmente, nem todos [dos serviços de multimédia v3 CLI](https://aka.ms/ams-v3-cli-ref) comandos trabalham no Azure Cloud Shell. Recomenda-se para utilizar a CLI localmente.

- [Criar uma conta de Media Services](create-account-cli-how-to.md).

## <a name="download-the-sample"></a>Transferir o exemplo

Clone o repositório do GitHub que contém o exemplo de .NET de transmissão para o computador com o comando seguinte:  

 ```bash
 git clone http://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts.git
 ```

O exemplo está localizado na pasta [EncodeAndStreamFiles](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/tree/master/AMSV3Quickstarts/EncodeAndStreamFiles).

O exemplo realiza as seguintes ações:

1. Cria uma Transformação (primeiro, verifica se existe a Transformação especificada). 
2. Cria um Elemento de saída que é utilizado como saída da Tarefa de codificação.
3. Cria a entrada da Tarefa com base num URL HTTPS.
4. Submete a Tarefa de codificação através da entrada e saída criada anteriormente.
5. Verifica o estado da Tarefa.
6. Cria um StreamingLocator.
7. Cria os URLs de transmissão.

Para obter uma explicação sobre o que faz cada função no exemplo, examine o código e observe os comentários [neste ficheiro de origem](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs).

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="run-the-sample-app"></a>Execute a aplicação de exemplo

Quando executar a aplicação, os URLs que podem ser utilizados para reproduzir o vídeo são apresentados através de protocolos diferentes. 

1. Prima Ctrl + F5 para executar a aplicação *EncodeAndStreamFiles*.
2. Escolha o protocolo **HLS** da Apple (termina com *manifest(format=m3u8-aapl)*) e copie o URL de transmissão da consola.

![Saída](./media/stream-files-tutorial-with-api/output.png)

No [código de origem](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs) do exemplo, pode ver como é criado o URL. Para criar o código, terá de concatenar o nome do anfitrião do ponto final da transmissão e o caminho de localizador da transmissão.  

## <a name="test-with-azure-media-player"></a>Testar com o Leitor de Multimédia do Azure

Para testar a transmissão, este artigo utiliza o Leitor de Multimédia do Azure. 

> [!NOTE]
> Se um leitor estiver alojado num site de https, confirme que atualiza o URL para “https”.

1. Abra um browser e navegue para [https://aka.ms/azuremediaplayer/](https://aka.ms/azuremediaplayer/).
2. Na caixa **URL:**, cole um dos valores do URL de transmissão que recebeu quando executou a aplicação. 
3. Prima **Atualizar Leitor**.

O Leitor de Multimédia do Azure pode ser utilizado para fins de teste, mas não deve ser utilizado num ambiente de produção. 

## <a name="clean-up-resources"></a>Limpar recursos

Se já não precisar de nenhum dos recursos presentes no grupo de recursos, incluindo a conta dos Serviços de Multimédia e a conta de armazenamento que criou neste Início Rápido, elimine o grupo de recursos.

Execute o seguinte comando da CLI:

```azurecli
az group delete --name amsResourceGroup
```

## <a name="examine-the-code"></a>Examinar o código

Para obter uma explicação sobre o que faz cada função no exemplo, examine o código e observe os comentários [neste ficheiro de origem](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs).

O tutorial [Upload, encode, and stream files](stream-files-tutorial-with-api.md) (Carregar, codificar e transmitir ficheiros) dá-lhe um exemplo mais avançado de transmissão com explicações detalhadas. 

## <a name="multithreading"></a>Multithreading

Os SDKs dos Serviços de Multimédia do Azure v3 não são seguros para threads. Ao trabalhar com aplicações com vários threads, deve gerar um novo objeto AzureMediaServicesClient por thread.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Tutorial: upload, encode, and stream files](stream-files-tutorial-with-api.md) (Tutorial: carregar, codificar e transmitir ficheiros)
