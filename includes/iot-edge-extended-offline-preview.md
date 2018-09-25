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
ms.openlocfilehash: a8160e677fa99d8cb691db39d7f29ba6eddbd261
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47004684"
---
## <a name="enabling-extended-offline-operation-preview"></a>Ativar a operação offline expandida (pré-visualização)
Começando com o [v1.0.2 versão](https://aka.ms/edge102) de Runtime do Edge, o dispositivo de limite e a jusante dispositivos que se ligam à mesma podem ser configurados para a operação offline expandida. 

Com esta capacidade, os módulos locais ou a jusante dispositivos podem autenticar novamente com o dispositivo de limite, conforme necessário e comunicar entre si através de mensagens e métodos, mesmo quando estiverem desconectados do IoT Hub. Ver isso [mensagem de blogue](https://aka.ms/iot-edge-offline) e [artigo conceito](../articles/iot-edge/offline-capabilities.md) para obter mais detalhes e âmbito desta capacidade.

Para ativar a estendido offline num cenário de gateway estabelece uma relação principal-subordinado entre o dispositivo de limite e downstream dispositivos que irão ligar à mesma.

1. No painel detalhes de borda dispositivo no portal do IoT Hub, clique a **gerir dispositivos de subordinado (pré-visualização)** botão na barra de comando superior.

1. Clique nas **+ adicionar** botão.

1. Na lista de dispositivos, selecione os dispositivos de subordinados e utilize a seta para a direita para escolher aquelas para adicionar como filhos.

1. Clique em **OK** para confirmar.

1. Na **definir módulos** ecrã de detalhes do dispositivo do Edge, clique em **configurar definições de Runtime do Edge avançadas**e, em **Hub do Edge** variáveis de ambiente adicionar uma entrada  **UpstreamProtocol** com o valor **MQTT**. Adicionar a mesma variável de ambiente e o valor para o **agente do Edge** também. 

1. Clique em **salvar** e certifique-se de que a **submeter** as alterações depois de clicar em **seguinte** duas vezes.

O dispositivo de limite e os seus dispositivos filho agora estão ativados para operação offline expandida.  