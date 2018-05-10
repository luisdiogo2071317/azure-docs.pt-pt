---
title: Guia de resolução de problemas para a transmissão em fluxo em direto | Microsoft Docs
description: Este tópico fornece sugestões sobre como resolver problemas de transmissão em fluxo em direto.
services: media-services
documentationcenter: ''
author: juliako
manager: cfowler
editor: ''
ms.assetid: 3a7f6c1d-ce57-4fa4-a7a6-edb526b3ffbf
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: 84e3e9fc18671d7199eeaf638377a6681cf09fb4
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="troubleshooting-guide-for-live-streaming"></a>Guia de resolução de problemas de transmissão em fluxo em direto
Este artigo fornece sugestões sobre como resolver alguns problemas de transmissão em fluxo em direto.

## <a name="issues-related-to-on-premises-encoders"></a>Problemas relacionados com codificadores no local
Esta secção fornece sugestões sobre como resolver problemas relacionados com codificadores no local que estão configurados para enviar uma transmissão em fluxo para canais de AMS que estão ativados para live encoding.

### <a name="problem-would-like-to-see-logs"></a>Problema: Gostaria de ver registos
* **Potencial problema**: não é possível encontrar o codificador registos que podem ajudar a depurar problemas.
  
  * **Telestream Wirecast**: normalmente, pode encontrar registos em C:\Users\{username} \AppData\Roaming\Wirecast\ 
  * **Em direto elemental**: pode encontrar tem ligações para os registos no portal de gestão. Clique em **estatísticas**, em seguida, **registos**. No **ficheiros de registo** página, verá uma lista de registos de todos os itens LiveEvent; selecione aquele que correspondam a sua sessão atual. 
  * **Codificador em direto do suporte de dados de Flash**: pode encontrar o **diretório de registo...**  ao navegar até o **codificação registo** separador.

### <a name="problem-there-is-no-option-for-outputting-a-progressive-stream"></a>Problema: Não há nenhuma opção para exportar um fluxo de transferência progressiva
* **Potencial problema**: O codificador que está a ser utilizado não deinterlace automaticamente. 
  
    **Passos de resolução de problemas**: procure uma opção Anular interlacing na interface do codificador. Depois de anular interlacing estiver ativada, verifique novamente para as definições de saída de transferência progressiva. 

### <a name="problem-tried-several-encoder-output-settings-and-still-unable-to-connect"></a>Problema: Tentativa de várias definições de saída de codificador e ainda não é possível ligar.
* **Potencial problema**: não foi corretamente repor canal codificação do Azure. 
  
    **Passos de resolução de problemas**: Certifique-se o codificador já não é enviar para AMS, pare e repor o canal. Depois de executar de novo, tente ligar o codificador com as novas definições. Se esta ainda não corrigir o problema, tente criar um novo canal completamente, por vezes canais podem tornar-se danificada após várias tentativas falhadas.  
* **Potencial problema**: tamanho de GOP a ou as definições de fotograma chave não são otimizadas uma vez. 
  
    **Passos de resolução de problemas**: intervalo de tamanho ou keyframe GOP recomendado é dois segundos. Alguns codificadores calcular esta definição no número de fotogramas, ao utilizam outros segundos. Por exemplo: quando exportar 30 fps, o tamanho de GOP seria 60 frames, que é equivalente a 2 segundos.  
* **Potencial problema**: portas fechadas estão a bloquear o fluxo. 
  
    **Passos de resolução de problemas**: quando a transmissão em fluxo através de RTMP, verifique as definições de firewall e/ou de proxy para confirmar que as portas de saída 1935 e 1936 estão abertas. 

> [!NOTE]
> Se depois de seguir os passos de resolução de problemas que ainda não é possível com êxito de fluxo, submeta um pedido de suporte no portal do Azure.
> 
> 

## <a name="media-services-learning-paths"></a>Percursos de aprendizagem dos Media Services
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Enviar comentários
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

