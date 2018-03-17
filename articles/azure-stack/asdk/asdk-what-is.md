---
title: "Uma introdução ao Azure pilha Development Kit (ASDK) | Microsoft Docs"
description: "Descreve as novidades do ASDK e casos de utilização comuns para avaliar a pilha do Microsoft Azure."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 5a268a29c7a767084049bf56270aa8bc9d2ccc3f
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/17/2018
---
# <a name="what-is-the-azure-stack-development-kit"></a>O que é o Kit de desenvolvimento de pilha do Azure?
[Microsoft Azure pilha integrado sistemas](.\.\azure-stack-poc.md) no intervalo de tamanho de nós de 4-12 e jointly são suportados por um parceiro de hardware e a Microsoft. Utilize sistemas de pilha do Azure integrado para ativar novos cenários para as cargas de trabalho de produção. Se tiver um operador de pilha do Azure que gere a infraestrutura de sistemas integrada e que oferece serviços, consulte a nossa [documentação de operador](https://docs.microsoft.com/azure/azure-stack).

O Kit de desenvolvimento de pilha do Azure (ASDK) é uma implementação de nó único da pilha do Azure que pode transferir e utilizar **gratuitamente**. Todos os componentes ASDK estão instalados em máquinas virtuais em execução num computador de servidor de anfitrião único que tem de cumprir ou exceder o [requisitos mínimos de hardware](asdk-deploy-considerations.md#hardware). O ASDK destina-se para proporcionar um ambiente em que pode avaliar pilha do Azure e desenvolver aplicações modernas utilizando APIs e as ferramentas consistente com o Azure num *não a produção* ambiente. 

> [!IMPORTANT]
> O ASDK não se destina a ser utilizada ou suportado num ambiente de produção.

Porque todos os componentes ASDK são implementados num computador de anfitrião do kit de desenvolvimento único, existem limitados recursos físicos disponíveis. Com as implementações de ASDK, a pilha de Azure VMs de infraestrutura e de inquilino VMs coexistem no mesmo computador do servidor. Esta configuração não se destina a avaliação de escala ou desempenho.

O ASDK foi concebida para fornecer uma experiência de cloud híbrida consistentes com o Azure para:
- **Os administradores** (operadores de pilha do Azure). O ASDK é um ótimo recurso para avaliar e saber mais sobre os serviços do Azure pilha disponíveis.
- **Os programadores**. O ASDK pode ser utilizada para desenvolver híbrido ou aplicações modernas no local (ambientes de desenvolvimento/teste). Isto oferece repetibilidade de experiência de desenvolvimento antes ou em conjunto com as implementações de produção de pilha do Azure. 

Ver este breve vídeo para saber mais sobre o ASDK:

> [!VIDEO https://www.youtube.com/embed/dbVWDrl00MM]


## <a name="asdk-and-multi-node-azure-stack-differences"></a>Diferenças de pilha do Azure ASDK e com vários nós
Implementações de ASDK de nó único diferem das implementações de pilha do Azure com vários nós de algumas formas importantes que deve ter em consideração.

|Descrição|ASDK|Pilha do Azure com vários nós|
|-----|-----|-----|
|**Dimensionamento**|Todos os componentes são instalados num computador de nó único servidor.|Pode variar em tamanho de nós de 4-12.|
|**Resiliência**|Configuração de nó único não fornece elevada disponibilidade|[Disponibilidade elevada](.\.\azure-stack-key-features.md#high-availability-for-azure-stack) capacidades são suportadas.|
|**Redes**|O ASDK utiliza uma VM chamada AzS BGPNAT01 para encaminhar todo o tráfego de rede ASDK. Não existem não requisitos adicionais de comutador.|A VM AzS BGPNAT01 não existe em implementações de vários nós. Mais complexas [infraestrutura de encaminhamento de rede](.\.\azure-stack-network.md#network-infrastructure) é necessário incluindo Top-Of-Rack TOR (), controlador de gestão de placas base (BMC) e comutadores de limite (rede de centro de dados).|
|**Processo de patch e atualização**|Para mover para uma nova versão do ASDK, tem de voltar a implementar o ASDK no computador de anfitrião do kit de desenvolvimento.|[Aplicar o patch e atualizar](.\.\azure-stack-updates.md) processo utilizado para atualizar a versão instalada da pilha do Azure.|
|**Suporte**|Fórum MSDN do Azure pilha. O suporte de serviço de cliente da Microsoft e suportar (CSS) é *não* disponíveis para ambientes de não produção.|[Fórum MSDN do Azure pilha](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStack) e suporte do CSS completas.|
| | |

## <a name="learn-about-available-services"></a>Saiba mais sobre serviços disponíveis
Como um operador de pilha do Azure, é necessário saber quais os serviços pode disponibilizar aos seus utilizadores. Pilha do Azure suporta um subconjunto de serviços do Azure e a lista de serviços suportados irá continuar a evoluir ao longo do tempo.

### <a name="foundational-services"></a>Serviços fundamentais sobre
Por predefinição, a pilha de Azure inclui os seguintes "dos serviços" quando implementa o ASDK:
- Computação
- Armazenamento
- Redes
- Cofre de Chaves

Com estes serviços fundamentais sobre, pode oferecer infraestrutura-como-um-serviço (IaaS) aos seus utilizadores com configuração mínima.

### <a name="additional-services"></a>Serviços adicionais
Atualmente, são suportados os seguintes serviços de plataforma-como-um-serviço (PaaS) adicionais:
- Serviço de Aplicações
- Funções do Azure
- Bases de dados do SQL Server e o MySQL

> [!NOTE]
> Estes serviços requerem configuração adicional antes de poder torná-los disponíveis para os seus utilizadores e não estão disponíveis por predefinição quando instala o ASDK.

## <a name="service-roadmap"></a>Plano de serviço
Pilha do Azure irá continuar a adicionar suporte para serviços do Azure adicionais. Para saber mais sobre as novidades na seguinte com pilha do Azure, consulte o [Roteiro da pilha de Azure](https://azure.microsoft.com/roadmap/?tag=azure-stack). 


## <a name="next-steps"></a>Passos Seguintes
Para começar a avaliar a pilha do Azure, terá de preparar o anfitrião do kit de desenvolvimento de computador do servidor e, em seguida, [instalar o ASDK](asdk-deploy.md). Depois disso, pode inscrever-para os portais de administrador e utilizador para começar a utilizar a pilha do Azure.