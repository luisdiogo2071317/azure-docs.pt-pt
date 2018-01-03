---
title: Resolver problemas de limite de IoT do Azure | Microsoft Docs
description: "Resolver problemas comuns e conhecer as competências de resolução de problemas para o limite de IoT do Azure"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 12/15/2017
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 3f61f0bf8234e747ae38146d1a5ea030e3163fa3
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2017
---
# <a name="common-issues-and-resolutions-for-azure-iot-edge"></a>Problemas comuns e de resoluções para limite de IoT do Azure

Se ocorrerem problemas com o Azure IoT Edge no seu ambiente, utilize este artigo como guia para resolução de problemas e a resolução. 

## <a name="standard-diagnostic-steps"></a>Passos de diagnóstico padrão 

Quando ocorrer um problema, saiba mais sobre o estado do seu dispositivo de limite de IoT por rever as mensagens que passar de e para o dispositivo e os registos do contentor. Utilize as ferramentas e comandos nesta secção para recolher informações. 

* Consulte os registos dos contentores para detetar problemas de docker. Começar a utilizar os contentores implementados, em seguida, observar os contentores que compõem o tempo de execução do IoT contorno: o agente de limite e limite Hub. Os registos do agente de limite normalmente fornecem informações sobre o ciclo de vida de cada contentor. Os registos de Hub de limite fornecem informações sobre mensagens e o encaminhamento. 

   ```cmd
   docker logs <container name>
   ```

* Ver as mensagens a ir através do Hub de limite e recolher informações sobre atualizações de propriedades do dispositivo com registos verbosos dos contentores de tempo de execução.

   ```cmd
   iotedgectl setup --runtime-log-level DEBUG
   ```

* Se ocorrerem problemas de conectividade, verifique as variáveis de ambiente de dispositivo de limite, como a cadeia de ligação do dispositivo:

   ```cmd
   docker exec edgeAgent printenv
   ```

Também pode ver as mensagens que está a ser enviadas entre o IoT Hub e os dispositivos de limite de IoT. Visualizar estas mensagens utilizando o [Toolkit do Azure IoT](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) extensão para Visual Studio Code. Para obter mais orientações, consulte [ferramenta útil quando desenvolver com o Azure IoT](https://blogs.msdn.microsoft.com/iotdev/2017/09/01/handy-tool-when-you-develop-with-azure-iot/).

Depois de investigar os registos e as mensagens de informações, também pode tentar reiniciar o tempo de execução do limite do Azure IoT:

   ```cmd
   iotedgectl restart
   ```

## <a name="edge-agent-stops-after-about-a-minute"></a>Agente de contorno para após sobre um minuto

O agente de limite é iniciado e estiver em execução com êxito durante sobre um minuto, em seguida, deixa de. Os registos de indicam que o agente de limite está a tentar estabelecer ligação ao IoT Hub através de AMQP e, em seguida, tentar ligar através de AMQP sobre websocket aproximadamente 30 segundos mais tarde. Quando não conseguir, o agente de limite sai. 

Registos do agente de limite de exemplo:

```
2017-11-28 18:46:19 [INF] - Starting module management agent. 
2017-11-28 18:46:19 [INF] - Version - 1.0.7516610 (03c94f85d0833a861a43c669842f0817924911d5) 
2017-11-28 18:46:19 [INF] - Edge agent attempting to connect to IoT Hub via AMQP... 
2017-11-28 18:46:49 [INF] - Edge agent attempting to connect to IoT Hub via AMQP over WebSocket... 
```

### <a name="root-cause"></a>Causa raiz
Uma configuração de rede na rede do anfitrião está a impedir que o agente de limite atingir a rede. O agente tenta primeiro estabeleça a ligação através de AMQP (porta 5671). Se isto falhar, tenta websockets (porta 443).

O tempo de execução do limite de IoT configura uma rede para cada um dos módulos comunicar em. No Linux, esta rede estiver numa rede de bridge. No Windows, utiliza NAT. Este problema é mais comum nos dispositivos Windows com contentores do Windows que utilizam a rede NAT. 

### <a name="resolution"></a>Resolução
Certifique-se de que existe uma rota para a internet para os endereços IP atribuídos a esta rede bridge/NAT. Por vezes, uma configuração de VPN no anfitrião sobrepõe-se a rede de margem de IoT. 

## <a name="edge-hub-fails-to-start"></a>Hub de limite não consegue iniciar

A falha de Hub de contorno para iniciar e impressões a seguinte mensagem para os registos: 

```
One or more errors occurred. 
(Docker API responded with status code=InternalServerError, response=
{\"message\":\"driver failed programming external connectivity on endpoint edgeHub (6a82e5e994bab5187939049684fb64efe07606d2bb8a4cc5655b2a9bad5f8c80): 
Error starting userland proxy: Bind for 0.0.0.0:443 failed: port is already allocated\"}\n) 
```

### <a name="root-cause"></a>Causa raiz
Outro processo no computador anfitrião tem de vincular a porta 443. O Hub de limite mapeia portas 5671 e 443 para utilizar em cenários de gateway. Este mapeamento de portas falha se outro processo já foi vinculado esta porta. 

### <a name="resolution"></a>Resolução
Localize e parar o processo que está a utilizar a porta 443. Este processo é normalmente um servidor web.

## <a name="edge-agent-cant-access-a-modules-image-403"></a>Limite de agente não consegue aceder à imagem de um módulo (403)
Não for possível executar um contentor e os registos do agente de limite mostram um erro 403. 

### <a name="root-cause"></a>Causa raiz
O agente de limite não tem permissões para aceder à imagem de um módulo. 

### <a name="resolution"></a>Resolução
Tente executar a `iotedgectl login` novamente o comando.

## <a name="next-steps"></a>Passos Seguintes
Pensa que foi encontrado um erro na plataforma de limite de IoT? ., [Submeter um problema](https://github.com/Azure/iot-edge/issues) para que pode continuar a melhorar. 
