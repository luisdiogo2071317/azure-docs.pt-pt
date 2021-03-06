---
title: Otimizar o desempenho do Visual Studio para projetos de malha do Azure Service Fabric | Documentos da Microsoft
description: Otimizar o desempenho do Visual Studio para aplicações de Mesh de recursos de infraestrutura de serviço do Azure
services: service-fabric-mesh
keywords: otimizar o desempenho de depuração
author: tylermsft
ms.author: twhitney
ms.date: 11/29/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: jeconnoc
ms.openlocfilehash: 72e900e6e48d18a721be7d2991428f81a81d1303
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2018
ms.locfileid: "52893531"
---
# <a name="optimize-visual-studio-performance-for-service-fabric-mesh-projects"></a>Otimizar o desempenho do Visual Studio para projetos de malha do Service Fabric

Este artigo mostra-lhe como otimizar o desempenho do Visual Studio para projetos de malha do Service Fabric, para que a sua depuração primeiro executar (F5) é muito mais rápida.  

## <a name="change-visual-studio-settings"></a>Alterar configurações do Visual Studio
 
No Visual Studio, em **ferramentas** > **opções**  > **ferramentas de malha de recursos de infraestrutura do serviço** > **geral**, pode ajustar as seguintes definições:

- **Extrair imagens de Docker necessárias no projeto aberto** torna a sua depuração primeiro executar (F5) mais rapidamente ao iniciar o processo de download da imagem, enquanto o projeto está a carregar.  
- **Implementar a aplicação no projeto aberto** pode tornar a sua depuração primeiro executar (F5) mais rapidamente ao iniciar o processo de implementação quando o projeto é aberto.  
- **Remover a aplicação no projeto fechar** reclama recursos (CPU, RAM) alocados para a aplicação ao remover a aplicação de malha, quando o projeto está fechado.  

Quando vir as mensagens na janela de saída de ferramentas do Service Fabric que o Visual Studio é "solicitar imagens", "aquecer" ou "Remover a aplicação", é no contexto das definições acima. Pode desativar estas definições.

## <a name="next-steps"></a>Passos Seguintes

Leia o [depurar um tutorial de aplicação de malha](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md)