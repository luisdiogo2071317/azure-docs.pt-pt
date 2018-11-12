---
title: incluir ficheiro
description: IOT edge
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: kgremban
ms.openlocfilehash: be4d82577584e83e29f2511d51256fda0970e917
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51264230"
---
## <a name="enabling-extended-offline-operation-preview"></a>Ativar a operação offline expandida (pré-visualização)
Com o [a v1.0.4 versão](https://github.com/Azure/azure-iotedge/releases/tag/1.0.4) de Runtime do Edge, o dispositivo de limite e a jusante dispositivos que se ligam à mesma podem ser configurados para a operação offline expandida. 

Com esta capacidade, os módulos locais ou a jusante dispositivos podem autenticar novamente com o dispositivo de limite, conforme necessário e comunicar entre si através de mensagens e métodos, mesmo quando estiverem desconectados do IoT Hub. Ver isso [mensagem de blogue](https://aka.ms/iot-edge-offline) e [artigo conceito](../articles/iot-edge/offline-capabilities.md) para obter mais detalhes e âmbito desta capacidade.

Para ativar a estendido offline num cenário de gateway estabelece uma relação principal-subordinado entre o dispositivo de limite e downstream dispositivos que irão ligar à mesma.

1. No painel detalhes de borda dispositivo no portal do IoT Hub, clique a **gerir dispositivos de subordinado (pré-visualização)** botão na barra de comando superior.

1. Clique nas **+ adicionar** botão.

1. Na lista de dispositivos, selecione os dispositivos de subordinados e utilize a seta para a direita para escolher aquelas para adicionar como filhos.

1. Clique em **OK** para confirmar.

O dispositivo de limite e os seus dispositivos filho agora estão ativados para operação offline expandida.  