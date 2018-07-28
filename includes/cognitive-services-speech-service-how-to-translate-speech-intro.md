---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: 4b823a8dc15d33e0537fae348b34c3a3fee84558
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39330922"
---
<!-- N.B. no header, language-agnostic -->

O [SDK de voz](~/articles/cognitive-services/speech-service/speech-sdk.md) fornece a forma mais simples de utilizar a tradução de voz na sua aplicação.
O SDK fornece todas as funcionalidades do serviço.
O processo básico para a execução de tradução de voz inclui os seguintes passos:

1. Criar uma fábrica de voz, fornecendo uma chave de subscrição do serviço de voz e um [região](~/articles/cognitive-services/speech-service/regions.md).
   Também é configurar os idiomas de tradução de origem e destino, bem como especificar se pretende que a saída de texto ou de voz.

1. Obtenha um reconhecedor de tradução de fábrica de voz.
   Existem vários tipos de reconhecedores de tradução com base na origem de áudio que estiver a utilizar.

1. Obstruir os eventos para a operação assíncrona, se assim o desejar.
   O reconhecedor, em seguida, chama seus manipuladores de eventos quando tem resultados intermediárias e finais.
   Caso contrário, a aplicação irá receber um resultado final transcrição.

1. Inicie o reconhecimento e a tradução.

Veja a seguir, vamos mostrar vários fragmentos de código para cenários de tradução de voz com o SDK de voz.

[!include[Get a Subscription Key](cognitive-services-speech-service-get-subscription-key.md)]
