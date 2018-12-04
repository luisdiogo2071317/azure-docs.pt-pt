---
title: Arquitetura de conectividade de instância de gerida de base de dados SQL do Azure | Documentos da Microsoft
description: Este artigo fornece a visão geral de comunicação de instância gerida da base de dados SQL do Azure e explica a arquitetura de conectividade, bem como a forma como os diferentes componentes de função para direcionar o tráfego para a instância gerida.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: bonova, carlrab
manager: craigg
ms.date: 08/16/2018
ms.openlocfilehash: d0747e9e54a48ecccb7051ad4fe9998d86599ffe
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52840746"
---
# <a name="azure-sql-database-managed-instance-connectivity-architecture"></a>Arquitetura de conectividade de instância de gerida de base de dados SQL do Azure 

Este artigo fornece a visão geral de comunicação de instância gerida da base de dados SQL do Azure e explica a arquitetura de conectividade, bem como a forma como os diferentes componentes de função para direcionar o tráfego para a instância gerida.  

## <a name="communication-overview"></a>Descrição geral da comunicação 

O diagrama seguinte mostra as entidades que ligar à instância gerida, bem como os recursos que a instância gerida tem de entrar em contacto para funcionar corretamente. 

![entidades de arquitetura de conectividade](./media/managed-instance-connectivity-architecture/connectivityarch001.png)

Comunicação que é descrita na parte inferior do diagrama representa a aplicativos de cliente e ferramentas de ligação para a instância gerida como origem de dados.  

Como a instância gerida é como-um-serviços de plataforma (PaaS) a oferta, a Microsoft gere este serviço usando agentes automatizados (gestão, implementação e manutenção) com base nos fluxos de dados de telemetria. Como o gerenciamento de instância gerida é unicamente responsabilidade de Microsoft, os clientes não são capazes de aceder a máquinas de virtuais de cluster de instância gerida através de RDP. 

Algumas operações do SQL Server iniciadas pelas aplicações ou os utilizadores finais podem exigir a instância gerida interagir com a plataforma. Um caso é a criação de uma instância gerida da base de dados - um recurso que é exposto por meio do portal, o PowerShell e a CLI do Azure. 

Instância gerida depende de outros serviços do Azure para o seu funcionamento adequado (como o armazenamento do Azure para cópias de segurança, Service bus do Azure para a telemetria, Azure AD para autenticação, o Azure Key Vault para TDE e assim por diante) e inicia ligações aos mesmos em conformidade. 

Todas as comunicações, indicadas acima, são criptografadas e assinados com certificados. A certificar-se de que as partes da comunicação são confiáveis, instância gerida constantemente verifica se estes certificados ao contactar a autoridade de certificação. Se os certificados são revogados ou instância gerida não foi possível verificá-las, ele fecha as ligações para proteger os dados. 

## <a name="high-level-connectivity-architecture"></a>Arquitetura de alto nível de conectividade 

Num alto nível, a instância gerida é um conjunto de componentes do serviço, hospedado num conjunto dedicado de isolado máquinas virtuais que são executados dentro da sub-rede de rede virtual de cliente e formar um cluster virtual. 

Várias instâncias geridas podem ser alojadas de cluster virtual único. O cluster é automaticamente expandido ou contratado se for necessário quando o cliente é alterado o número de instâncias aprovisionados na sub-rede. 

Aplicações de cliente foi possível ligar a bases de dados de instância gerida, consulta e atualização apenas se eles são executados dentro da rede virtual ou em modo de peering de rede virtual ou VPN / Express Route ligado através do ponto final com o endereço IP privado de rede.  

![Diagrama de arquitetura de conectividade](./media/managed-instance-connectivity-architecture/connectivityarch002.png)

Executam serviços de gerenciamento e a implantação da Microsoft fora da rede virtual, para que a ligação entre a instância gerida e serviços Microsoft são enviados pelos pontos de extremidade com endereços IP públicos. Quando a instância gerida cria a ligação de saída, na extremidade de recebimento parece que ele está vindo este IP público devido a tradução de endereços de rede (NAT). 

Fluxos de tráfego de gestão através da rede virtual do cliente. Isso significa que afetam a elementos da infraestrutura de rede virtual e pode potencialmente o tráfego de gestão de danos a causar a instância entrar em estado com falhas e fique indisponível. 

> [!IMPORTANT]
> Para melhorar a disponibilidade de expirience e o serviço de cliente, a Microsoft aplica políticas de intenção de rede em elementos de infraestrutura de rede virtual do Azure que podem afetar o funcionamento da instância gerida. Esse é um mecanismo de plataforma para comunicar os requisitos de rede de forma transparente para os utilizadores finais, com o objetivo principal para impedir a configuração incorreta de rede e certifique-se as operações normais de instância gerida. Após a eliminação da instância gerida política de intenção de rede são também removida. 

## <a name="virtual-cluster-connectivity-architecture"></a>Arquitetura de conectividade do virtual cluster 

Vamos aprofundar na arquitetura de conectividade de instância gerida. O diagrama seguinte mostra o esquema conceitual do virtual cluster. 

![cluster virtual do diagrama de arquitetura de conectividade](./media/managed-instance-connectivity-architecture/connectivityarch003.png)

Os clientes ligam-se para a instância gerida utilizando o nome de anfitrião que tenha um formulário < mi_name >. <clusterid>. database.windows.net. Este nome de anfitrião é resolvido para o endereço IP privado, embora ele está registado na zona DNS pública e é resolvível publicamente. 

Este endereço IP privado pertence para o Managed instância carga balanceador interno (ILB) que direciona o tráfego para o Gateway de instância gerida (GW). Como várias instâncias geridas poderia potencialmente executado dentro do mesmo cluster, o GW utiliza o nome de anfitrião da instância gerida para redirecionar o tráfego para o serviço de motor de SQL correto. 

Serviços de gestão e implementação ligar através de instância gerida [ponto final de gestão](sql-database-managed-instance-management-endpoint.md) que mapeia para o Balanceador de carga externo. O tráfego é encaminhado para os nós apenas se tiver recebido num conjunto predefinido de portas que são utilizados exclusivamente por componentes de gerenciamento de instância gerida. Firewall interno em nós está configurado para permitir tráfego apenas a partir de intervalos de IP específicos de Microsoft. Todas as comunicações entre os componentes de gestão e plano de gestão é mutuamente autenticado de certificado. 

## <a name="next-steps"></a>Passos Seguintes 

- Para uma descrição geral, consulte [o que é uma instância gerida](sql-database-managed-instance.md) 
- Para obter mais informações sobre a configuração de VNet, veja [configuração de VNet de instância gerida](sql-database-managed-instance-vnet-configuration.md). 
- Para um início rápido, veja como criar a instância gerida: 
  - Do [portal do Azure](sql-database-managed-instance-get-started.md) 
  - usando [PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/) 
  - usando [modelo Azure Resource Manager](https://azure.microsoft.com/resources/templates/101-sqlmi-new-vnet/) 
  - usando [modelo Azure Resource Manager (jumpbox com o SSMS incluídos)](https://portal.azure.com/) 

