---
title: Reproduzir com leitor de multimédia do Azure - Azure | Documentos da Microsoft
description: Este tópico apresenta uma visão geral do leitor de multimédia do Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 01/03/2018
ms.author: juliako
ms.openlocfilehash: 4677fc9a56db3190073b3f310a51632055510bc2
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2019
ms.locfileid: "54028883"
---
# <a name="play-back-with-azure-media-player"></a>Reproduzir com leitor de multimédia do Azure

[O Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html) é um leitor de vídeo da web criado para reproduzir conteúdo multimédia dos serviços de multimédia do Microsoft Azure numa grande variedade de navegadores e dispositivos. O leitor de multimédia do Azure utiliza padrões da indústria, como HTML5, extensões de origem de mídia (MSE) e Encrypted Media Extensions (EME) para fornecer uma experiência plena de transmissão em fluxo adaptativa. Quando esses padrões não estão disponíveis num dispositivo ou num browser, o leitor de multimédia do Azure utiliza Flash e Silverlight como tecnologia de contingência. Independentemente da tecnologia de reprodução utilizada, os desenvolvedores terão uma interface JavaScript unificada para aceder a APIs. Isso permite a reprodução de conteúdo servido pelos serviços de multimédia do Azure a reprodução numa variedade de toda a de dispositivos e browsers sem qualquer esforço adicional.

Serviços de multimédia do Microsoft Azure permitem que o conteúdo ser atendidas com HLS, DASH, Smooth Streaming formatos de transmissão em fluxo para reproduzir conteúdo de volta. O Azure Media Player leva em conta esses vários formatos e executa automaticamente a ligação melhor, com base nos recursos de plataforma/browser. Serviços de multimédia também permite que a encriptação dinâmica de recursos com encriptação PlayReady ou AES de 128 bits de encriptação de envelope. O leitor de multimédia do Azure permite-lhe PlayReady e AES de 128 bits conteúdo encriptado quando configurado corretamente. 

[Inicie a avaliação gratuita](http://azure.microsoft.com/en-us/pricing/free-trial/)

## <a name="monitor-diagnostics-of-a-video-stream"></a>Monitor de diagnóstico de um fluxo de vídeo

Pode utilizar o [página de demonstração do leitor de multimédia do Azure](http://aka.ms/amp) para monitorizar o diagnóstico de um fluxo de vídeo. 

! [Diagnósticos do leitor de multimédia do azure] [amp_diagnostics]

## <a name="next-steps"></a>Passos Seguintes

- [Inscreva-se para se manter atualizado com a versão mais recente do leitor de multimédia do Azure](http://azuremediaplayerdemo.azurewebsites.net/amp_signup.html)
- [Documentação do leitor de multimédia do Azure](https://aka.ms/ampdocs)
- [Exemplos de leitor de multimédia do Azure](https://aka.ms/ampsamples)
