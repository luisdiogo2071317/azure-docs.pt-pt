---
title: Resolver problemas do Azure IoT Edge | Microsoft Docs
description: "Resolver problemas comuns e conhecer as competências de resolução de problemas do Azure IoT Edge"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 12/15/2017
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 5de069eb35e88c1dce6dcfa5a1661e8ab87302b1
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="common-issues-and-resolutions-for-azure-iot-edge"></a>Problemas comuns e resoluções do Azure IoT Edge

Se ocorrerem problemas com o Azure IoT Edge no seu ambiente, utilize este artigo como guia para a resolução de problemas. 

## <a name="standard-diagnostic-steps"></a>Passos de diagnóstico padrão 

Quando ocorrer um problema, saiba mais sobre o estado do seu dispositivo IoT Edge ao rever as mensagens e registos do contentor que passam de e para o dispositivo. Utilize as ferramentas e comandos nesta secção para recolher informações. 

* Veja os registos dos contentores do docker para detetar problemas. Comece a utilizar os contentores implementados e, em seguida, veja os contentores que compõem o runtime do IoT Edge: Agente do Edge e Hub do Edge. Os registos do Agente do Edge dispõem normalmente de informações sobre o ciclo de vida de cada contentor. Os registos do Hub do Edge dispõem de informações sobre mensagens e encaminhamento. 

   ```cmd
   docker logs <container name>
   ```

* Veja as mensagens que passam pelo Hub do Edge e recolha informações sobre atualizações de propriedades do dispositivo com registos verbosos dos contentores do runtime.

   ```cmd
   iotedgectl setup --runtime-log-level DEBUG
   ```

* Se ocorrerem problemas de conectividade, verifique as variáveis de ambiente de dispositivo do edge, como a cadeia de ligação do dispositivo:

   ```cmd
   docker exec edgeAgent printenv
   ```

Também pode verificar as mensagens que são enviadas entre o Hub IoT e os dispositivos do IoT Edge. Veja estas mensagens com a extensão [Toolkit IoT do Azure](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) para o Visual Studio Code. Para obter mais documentação de orientação, veja [Ferramenta útil ao programar com o Azure IoT](https://blogs.msdn.microsoft.com/iotdev/2017/09/01/handy-tool-when-you-develop-with-azure-iot/).

Depois de investigar as informações dos registos e das mensagens, também pode tentar reiniciar o runtime do Azure IoT Edge:

   ```cmd
   iotedgectl restart
   ```

## <a name="edge-agent-stops-after-about-a-minute"></a>O Agente do Edge para após cerca de um minuto

O Agente do Edge é iniciado e executado com êxito durante cerca de um minuto e, em seguida, para. Os registos indicam que o Agente do Edge está a tentar ligar ao Hub IoT através de AMQP e, em seguida, tenta ligar através de AMQP sobre websocket aproximadamente 30 segundos mais tarde. Quando ocorre uma falha, o Agente do Edge sai. 

Registos do Agente do Edge de exemplo:

```output
2017-11-28 18:46:19 [INF] - Starting module management agent. 
2017-11-28 18:46:19 [INF] - Version - 1.0.7516610 (03c94f85d0833a861a43c669842f0817924911d5) 
2017-11-28 18:46:19 [INF] - Edge agent attempting to connect to IoT Hub via AMQP... 
2017-11-28 18:46:49 [INF] - Edge agent attempting to connect to IoT Hub via AMQP over WebSocket... 
```

### <a name="root-cause"></a>Causa raiz
Uma configuração de rede na rede do anfitrião está a impedir que o Agente do Edge alcance a rede. O agente primeiro tenta ligar através de AMQP (porta 5671). Se não conseguir, tenta websockets (porta 443).

O runtime do IoT Edge configura uma rede para comunicar em cada um dos módulos. No Linux, esta rede é uma rede de ponte. No Windows, utiliza NAT. Este problema é mais comum nos dispositivos Windows com contentores do Windows que utilizam a rede NAT. 

### <a name="resolution"></a>Resolução
Certifique-se de que existe uma rota para a Internet para os endereços IP atribuídos a esta rede de ponte/NAT. Por vezes, uma configuração de VPN no anfitrião sobrepõe-se à rede do IoT Edge. 

## <a name="edge-hub-fails-to-start"></a>O Hub do Edge falha ao iniciar

O Hub do Edge falha ao iniciar e apresenta a seguinte mensagem nos registos: 

```output
One or more errors occurred. 
(Docker API responded with status code=InternalServerError, response=
{\"message\":\"driver failed programming external connectivity on endpoint edgeHub (6a82e5e994bab5187939049684fb64efe07606d2bb8a4cc5655b2a9bad5f8c80): 
Error starting userland proxy: Bind for 0.0.0.0:443 failed: port is already allocated\"}\n) 
```

### <a name="root-cause"></a>Causa raiz
Outro processo no computador anfitrião vinculou a porta 443. O Hub do Edge mapeia as portas 5671 e 443 para utilizar em cenários de gateway. Este mapeamento de portas falha se outro processo já tiver vinculado esta porta. 

### <a name="resolution"></a>Resolução
Encontre e pare o processo que está a utilizar a porta 443. Este processo é normalmente um servidor Web.

## <a name="edge-agent-cant-access-a-modules-image-403"></a>O Agente do Edge não consegue aceder a uma imagem do módulo (403)
A execução de um contentor falha e os registos do Agente mostram um erro 403. 

### <a name="root-cause"></a>Causa raiz
O Agente do Edge não tem permissões para aceder à imagem de um módulo. 

### <a name="resolution"></a>Resolução
Tente executar o comando `iotedgectl login` novamente.

## <a name="next-steps"></a>Passos seguintes
Encontrou um erro na plataforma do IoT Edge? [Submeta um problema](https://github.com/Azure/iot-edge/issues) para que possamos continuar a melhorar. 
