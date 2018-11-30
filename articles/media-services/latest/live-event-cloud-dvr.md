---
title: LiveEvent de serviços de multimédia do Azure e um DVR na cloud | Documentos da Microsoft
description: Este artigo explica o que é o LiveOutput e como utilizar um DVR na cloud.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 11/28/2018
ms.author: juliako
ms.openlocfilehash: 5de7496d73ebe1c89ce27ef27df73b197f34e7c7
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/30/2018
ms.locfileid: "52642292"
---
# <a name="using-a-cloud-dvr"></a>Usando um DVR na cloud

R [LiveOutput](https://docs.microsoft.com/rest/api/media/liveoutputs) permite-lhe controlar as propriedades do fluxo em direto saída, como muito da transmissão em fluxo é registrada (por exemplo, a capacidade de DVR na cloud) e se é ou não podem iniciar os visualizadores assistem à transmissão em direto. A relação entre um **LiveEvent** e a respetiva **LiveOutput**relação s é semelhante à transmissão de televisão tradicional, no qual um canal (**LiveEvent**) representa um fluxo constante de vídeo e uma gravação (**LiveOutput**) tem um âmbito para um segmento de tempo específico (por exemplo, noite notícias da 6 17:30, para as 19:00: 00). Pode gravar programas de televisão com um gravador de vídeo Digital (DVR) – a funcionalidade equivalente no LiveEvents é gerenciada por meio da propriedade ArchiveWindowLength. É uma duração de período de tempo de ISO-8601 (por exemplo, PTHH:MM:SS), que especifica a capacidade do DVR e pode ser definida a partir de um mínimo de 3 minutos até um máximo de 25 horas.

## <a name="liveoutput"></a>LiveOutput

O **ArchiveWindowLength** valor determina até que ponto volta no tempo, um Visualizador pode recuar a partir da posição atual em direto.  **ArchiveWindowLength** também determina quanto tempo os manifestos dos clientes podem aumentar.

Suponha que estão a transmissão em fluxo num jogo de futebol americano, e ele tem um **ArchiveWindowLength** de apenas 30 minutos. Um visualizador que começa a observar seu evento 45 minutos depois de iniciado o jogo pode buscar no máximo na marca de 15 minutos. Sua **LiveOutput**s para o jogo vai continuar até a **LiveEvent** estiver parado, mas o conteúdo que está fora do **ArchiveWindowLength** é continuamente descartado do armazenamento e não é recuperável. Neste exemplo, o vídeo entre o início do evento e a marca de 15 minutos seria ter foi removido da sua DVR e do contentor no armazenamento de BLOBs para o [Asset](https://docs.microsoft.com/rest/api/media/assets). O arquivo não é recuperável e é removido do contentor no armazenamento de Blobs do Azure.

Cada **LiveOutput** está associado um **Asset**, que utiliza para gravar o vídeo para o contentor de armazenamento de Blobs do Azure associada. Para publicar o LiveOutput, tem de criar uma **StreamingLocator** para que **Asset**. Depois de criar uma [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators), pode compilar um URL de transmissão em fluxo que pode fornecer aos utilizadores.

R **LiveEvent** suporta até três em execução em simultâneo **LiveOutput**s para que possa criar no máximo de 3 gravações/arquivos de um fluxo em direto. Isto permite publicar e arquivar diferentes partes de um evento, conforme necessário. Suponha que precisa difundir um 24x7 live linear feed e criar "gravações" dos programas diferentes ao longo do dia para oferecer aos clientes como conteúdo sob demanda para visualização catch-up. Para este cenário, crie primeiro um LiveOutput principal, com uma janela de arquivo curtos de 1 hora ou menos – este é o principal em fluxo em direto que os visualizadores têm acesso seriam otimizar em. Criaria uma **StreamingLocator** para que isso **LiveOutput** e publicá-la para a sua aplicação ou site como o feed de "Live". Embora o **LiveEvent** está em execução, pode criar programaticamente uma segundo simultâneas **LiveOutput** no início de um programa (ou 5 minutos desde o início para fornecer alguns identificadores para cortar mais tarde). Este segundo **LiveOutput** podem ser eliminadas de 5 minutos após o programa é concluído. Com este segundo **Asset**, pode criar uma nova **StreamingLocator** para publicar este programa como um recurso de demanda no catálogo da sua aplicação. Pode repetir esse processo várias vezes para outros limites do programa ou destaques que deseja compartilhar, como vídeos a pedido, enquanto "Live" feed da primeira **LiveOutput** continua a difundir o feed linear. 

> [!NOTE]
> **LiveOutput**s início após a criação e param quando eliminado. Ao eliminar a **LiveOutput**, não está a eliminar subjacentes **Asset** e conteúdo no ativo.  

## <a name="next-steps"></a>Passos Seguintes

- [Descrição geral de transmissão em fluxo em direto](live-streaming-overview.md)
- [Tutorial de transmissão em fluxo em direto](stream-live-tutorial-with-api.md)

