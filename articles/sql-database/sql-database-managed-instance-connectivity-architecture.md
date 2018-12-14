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
ms.date: 12/10/2018
ms.openlocfilehash: 964f91f412645e141ca003d511480f6f6eb438a3
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2018
ms.locfileid: "53343310"
---
# <a name="azure-sql-database-managed-instance-connectivity-architecture"></a>Arquitetura de conectividade de instância de gerida de base de dados SQL do Azure

Este artigo fornece a visão geral de comunicação de instância gerida da base de dados SQL do Azure e explica a arquitetura de conectividade, bem como a forma como os diferentes componentes de função para direcionar o tráfego para a instância gerida.  

O Azure SQL Database Managed Instance é colocado dentro da VNet do Azure e a sub-rede dedicada para instâncias geridas. Esta implementação permite que os seguintes cenários: 
- Proteger o endereço IP privado.
- Ligar-se para uma instância gerida diretamente a partir de uma rede no local.
- Ligar uma instância gerida para o servidor ligado ou de outra no local o arquivo de dados.
- A ligar uma instância gerida nos recursos do Azure.

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
> Para melhorar a experiência do cliente e a disponibilidade do serviço, a Microsoft aplica políticas de intenção de rede em elementos de infraestrutura de rede virtual do Azure que podem afetar o funcionamento da instância gerida. Esse é um mecanismo de plataforma para comunicar os requisitos de rede de forma transparente para os utilizadores finais, com o objetivo principal para impedir a configuração incorreta de rede e certifique-se as operações normais de instância gerida. Após a eliminação da instância gerida política de intenção de rede são também removida.

## <a name="virtual-cluster-connectivity-architecture"></a>Arquitetura de conectividade do virtual cluster

Vamos aprofundar na arquitetura de conectividade de instância gerida. O diagrama seguinte mostra o esquema conceitual do virtual cluster.

![cluster virtual do diagrama de arquitetura de conectividade](./media/managed-instance-connectivity-architecture/connectivityarch003.png)

Os clientes ligam a instância gerida com o nome de anfitrião que tenha um formulário `<mi_name>.<dns_zone>.database.windows.net`. Este nome de anfitrião é resolvido para o endereço IP privado, embora ele está registado na zona DNS pública e é resolvível publicamente. O `zone-id` é gerado automaticamente quando o cluster é criado. Se um cluster recém-criado está a alojar uma instância secundária, compartilha sua identificação de zona com o cluster principal. Para obter mais informações, consulte [grupos de ativação pós-falha automática](sql-database-auto-failover-group.md##enabling-geo-replication-between-managed-instances-and-their-vnets)

Este endereço IP privado pertence para o Managed instância carga balanceador interno (ILB) que direciona o tráfego para o Gateway de instância gerida (GW). Como várias instâncias geridas poderia potencialmente executado dentro do mesmo cluster, o GW utiliza o nome de anfitrião da instância gerida para redirecionar o tráfego para o serviço de motor de SQL correto.

Serviços de gestão e implementação ligar através de instância gerida [ponto final de gestão](#management-endpoint) que mapeia para o Balanceador de carga externo. O tráfego é encaminhado para os nós apenas se tiver recebido num conjunto predefinido de portas que são utilizados exclusivamente por componentes de gerenciamento de instância gerida. Firewall interno em nós está configurado para permitir tráfego apenas a partir de intervalos de IP específicos de Microsoft. Todas as comunicações entre os componentes de gestão e plano de gestão é mutuamente autenticado de certificado.

## <a name="management-endpoint"></a>Ponto final de gestão

O cluster virtual de instância gerida da base de dados SQL do Azure contém um ponto final de gestão que a Microsoft utiliza para gerir a instância gerida. O ponto final de gestão está protegido com firewall interno na verificação de certificado mútua e nível de rede no nível de aplicativo. Pode [encontrar o endereço de ip do ponto final de gestão](sql-database-managed-instance-find-management-endpoint-ip-address.md).

Quando as ligações sejam iniciadas a partir de dentro da instância gerida (cópia de segurança, registo de auditoria), parece que origina o tráfego do endereço IP de público de ponto final de gestão. Pode limitar o acesso à serviços públicos de instância gerida através da definição de regras de firewall para permitir apenas o endereço de IP de instância gerida. Encontrar adicionar einformation sobre o método que pode [Verifique se o firewall interno da instância gerida](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md).

> [!NOTE]
> Isso não se aplica à configuração de regras de firewall para serviços do Azure que estão na mesma região que a instância gerida, como a plataforma do Azure tem uma otimização para o tráfego que passa entre os serviços que sejam colocados.

## <a name="network-requirements"></a>Requisitos da rede

É possível implementar a instância gerida numa sub-rede dedicada (sub-rede da instância gerida) dentro da rede virtual que está em conformidade com os seguintes requisitos:
- **Dedicado sub-rede**: A sub-rede de instância gerida não pode conter qualquer outro serviço de cloud associado à mesma, e não pode ser uma sub-rede de Gateway. Não será possível criar uma instância gerida numa sub-rede que contém os recursos que não seja a instância gerida e, posteriormente, pode não adicionar outros recursos na sub-rede.
- **Compatível com o grupo de segurança de rede (NSG)**: Um NSG associado a uma sub-rede de instância gerida tem de conter regras mostradas nas tabelas seguintes (regras de segurança de entrada obrigatória e regras de segurança de saída obrigatórios) à frente de quaisquer outras regras. Pode utilizar um NSG para controlar totalmente o acesso para o ponto final de dados de instância gerida ao filtrar o tráfego na porta 1433. 
- **Tabela de compatíveis rota definida pelo utilizador (UDR)**: A sub-rede de instância gerida tem de ter uma tabela de rotas de utilizador com **Internet de salto seguinte 0.0.0.0/0** como o UDR obrigatório atribuído ao mesmo. Além disso, pode adicionar um UDR que encaminha o tráfego que tem intervalos de IP privados no local como um destino através do gateway de rede virtual ou a aplicação de rede virtual (NVA). 
- **DNS personalizado opcional**: Se não for especificado um DNS personalizado na rede virtual, endereço IP de resolução de recursiva do Azure (por exemplo, 168.63.129.16) tem de ser adicionado à lista. Para obter mais informações, consulte [configurar o DNS de personalizado](sql-database-managed-instance-custom-dns.md). O servidor DNS personalizado tem de ser capaz de resolver nomes de anfitriões para os seguintes domínios e os respetivos subdomínios: *microsoft.com*, *windows.net*, *windows.com*, *msocsp.com*, *digicert.com*, *live.com*, *microsoftonline.com*, e *microsoftonline-p.com*. 
- **Não existem pontos finais de serviço**: A sub-rede de instância gerida não pode ter um ponto de extremidade de serviço associado a si. Certifique-se de que a opção de pontos finais de serviço é desativada quando criar a rede virtual.
- **Endereços IP suficientes**: A sub-rede de instância gerida tem de ter o mínimo de 16 endereços IP (recomendado mínimo é de 32 endereços IP). Para obter mais informações, consulte [determinar o tamanho da sub-rede para instâncias geridas](sql-database-managed-instance-determine-size-vnet-subnet.md). Pode implementar instâncias geridas na [da rede existente](sql-database-managed-instance-configure-vnet-subnet.md) depois de configurá-lo para satisfazer [instância gerida, requisitos de rede](#network-requirements), ou criar um [nova rede e sub-rede](sql-database-managed-instance-create-vnet-subnet.md).

> [!IMPORTANT]
> Não será capaz de implantar uma nova instância gerida, se a sub-rede de destino não é compatível com todos esses requisitos. Quando é criada uma instância gerida, uma *política de intenção de rede* é aplicado na sub-rede para impedir alterações não conformes para configuração de rede. Após a última instância é removida a partir da sub-rede, o *política de intenção de rede* é removido também

### <a name="mandatory-inbound-security-rules"></a>Regras de segurança de entrada obrigatório 

| Nome       |Porta                        |Protocolo|Origem           |Destino|Ação|
|------------|----------------------------|--------|-----------------|-----------|------|
|gestão  |9000, 9003, 1438, 1440, 1452|TCP     |Qualquer              |Qualquer        |Permitir |
|mi_subnet   |Qualquer                         |Qualquer     |SUB-REDE DE MI        |Qualquer        |Permitir |
|health_probe|Qualquer                         |Qualquer     |AzureLoadBalancer|Qualquer        |Permitir |

### <a name="mandatory-outbound-security-rules"></a>Regras de segurança de saída obrigatórios 

| Nome       |Porta          |Protocolo|Origem           |Destino|Ação|
|------------|--------------|--------|-----------------|-----------|------|
|gestão  |80, 443, 12000|TCP     |Qualquer              |Qualquer        |Permitir |
|mi_subnet   |Qualquer           |Qualquer     |Qualquer              |SUB-REDE DE MI  |Permitir |

  > [!Note]
  > Embora as regras de segurança de entrada obrigatório permitem tráfego a partir _qualquer_ origem nas portas 9000, 9003, 1438, 1440, 1452 estas portas são protegidas pelo firewall interno. Isso [artigo](sql-database-managed-instance-find-management-endpoint-ip-address.md) mostra como pode descobrir o endereço IP do ponto final de gestão e verifique se as regras de firewall. 

## <a name="next-steps"></a>Passos Seguintes

- Para uma descrição geral, consulte [o que é uma instância gerida](sql-database-managed-instance.md)
- Saiba como [configurar a nova VNet](sql-database-managed-instance-create-vnet-subnet.md) ou [configurar VNet existente](sql-database-managed-instance-configure-vnet-subnet.md) onde pode implementar instâncias geridas.
- [Calcular o tamanho da sub-rede](sql-database-managed-instance-determine-size-vnet-subnet.md) onde irá implementar instâncias geridas. 
- Para um início rápido, veja como criar a instância gerida:
  - Do [portal do Azure](sql-database-managed-instance-get-started.md)
  - usando [PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/)
  - usando [modelo Azure Resource Manager](https://azure.microsoft.com/resources/templates/101-sqlmi-new-vnet/)
  - usando [modelo Azure Resource Manager (jumpbox com o SSMS incluídos)](https://portal.azure.com/)
