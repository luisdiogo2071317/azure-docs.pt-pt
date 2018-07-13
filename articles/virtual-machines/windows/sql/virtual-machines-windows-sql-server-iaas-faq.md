---
title: SQL Server em Windows FAQ de máquinas virtuais do Azure | Documentos da Microsoft
description: Este artigo fornece respostas às perguntas mais frequentes sobre a execução do SQL Server em VMs do Azure.
services: virtual-machines-windows
documentationcenter: ''
author: v-shysun
manager: felixwu
editor: ''
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 07/12/2018
ms.author: v-shysun
ms.openlocfilehash: 48df858095cb867954460ec858567e41ed330063
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39009267"
---
# <a name="frequently-asked-questions-for-sql-server-running-on-windows-azure-virtual-machines"></a>Perguntas mais frequentes sobre o SQL Server em execução em máquinas de virtuais do Windows Azure

> [!div class="op_single_selector"]
> * [Windows](virtual-machines-windows-sql-server-iaas-faq.md)
> * [Linux](../../linux/sql/sql-server-linux-faq.md)

Este artigo fornece respostas para algumas das perguntas mais comuns sobre a execução [SQL Server em máquinas de virtuais do Windows Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/).

> [!NOTE]
> Este artigo se concentra em problemas específicos ao SQL Server em VMs do Windows. Se estiver a executar o SQL Server em VMs do Linux, veja a [FAQ de Linux](../../linux/sql/sql-server-linux-faq.md).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a id="images"></a> Imagens

1. **As imagens de Galeria de máquinas virtuais do SQL Server estão disponíveis?**

   O Azure mantém as imagens de máquinas virtuais para todas as versões principais suportadas do SQL Server em todas as edições para Windows e Linux. Para obter mais detalhes, consulte a lista completa de [imagens de VM do Windows](virtual-machines-windows-sql-server-iaas-overview.md#payasyougo) e [imagens de VM do Linux](../../linux/sql/sql-server-linux-virtual-machines-overview.md#create).

1. **Imagens de Galeria de máquinas virtuais do SQL Server existentes são atualizadas?**

   A cada dois meses, imagens do SQL Server na galeria da máquina virtual são atualizadas com o Windows mais recente e atualizações do Linux. Para imagens do Windows, isso inclui todas as atualizações marcadas como importantes no Windows Update, incluindo atualizações de segurança importantes do SQL Server e service packs. Para imagens do Linux, isso inclui as atualizações mais recentes do sistema. As atualizações cumulativas do SQL Server são tratadas de forma diferente para Linux e Windows. Para o Linux, atualizações cumulativas do SQL Server também inclui a atualização. Mas, neste momento, as VMs do Windows não são atualizadas com atualizações cumulativas do SQL Server ou Windows Server.

1. **Imagens de máquina virtual do SQL Server podem ser removidas da Galeria?**

   Sim. O Azure mantém apenas uma imagem por versão principal e de edição. Por exemplo, quando um novo service pack do SQL Server for lançado, o Azure adiciona uma nova imagem na Galeria para esse pacote de serviço. A imagem do SQL Server para o service pack anterior é imediatamente removida do portal do Azure. No entanto, ele ainda está disponível para aprovisionamento a partir do PowerShell dos três próximos meses. Depois de três meses, a imagem do service pack anterior já não está disponível. Esta política de remoção também seria aplicada caso uma versão do SQL Server se torna não suportada quando ela atinge o final do seu ciclo de vida.

1. **Pode criar uma imagem VHD de uma VM do SQL Server?**

   Sim, mas aqui estão algumas considerações. Se implementar este VHD para uma nova VM no Azure, fazer não ge a seção de configuração do SQL Server no portal. Em seguida, tem de gerir as opções de configuração do SQL Server através do PowerShell. Além disso, faturada à tarifa da VM de SQL foi originalmente baseia sua imagem. Isso vale mesmo que remova o SQL Server a partir de VHD antes de implementar. 

1. **É possível definir configurações não apresentadas na galeria da máquina virtual (por exemplo, Windows 2008 R2 + SQL Server 2012)?**

   Não. Para imagens de Galeria de máquinas virtuais que incluem o SQL Server, tem de selecionar uma das imagens fornecidas.

## <a name="creation"></a>Criação

1. **Como posso criar uma máquina virtual do Azure com o SQL Server?**

   A solução mais simples é criar uma Máquina Virtual que inclui o SQL Server. Para um tutorial sobre a inscrição para o Azure e criar uma VM do SQL a partir do portal, consulte [aprovisionar uma máquina virtual do SQL Server no portal do Azure](virtual-machines-windows-portal-sql-server-provision.md). Pode selecionar uma imagem de máquina virtual que utiliza o pagamento-por-segundo licenciamento do SQL Server ou pode utilizar uma imagem que permite que traga a sua própria licença do SQL Server. Tem também a opção de instalar manualmente o SQL Server numa VM com o uma edição gratuita licenciada (desenvolvedor ou Express) ou ao reutilizar uma licença de acesso no local. Se utilizar a sua própria licença, tem de ter [mobilidade de licenças através do Software Assurance no Azure](https://azure.microsoft.com/pricing/license-mobility/). Para obter mais informações, consulte [Pricing guidance for SQL Server Azure VMs (Documentação de orientação sobre preços de VMs do Azure do SQL Server)](virtual-machines-windows-sql-server-pricing-guidance.md).

1. **Como posso migrar meu banco de dados do SQL Server no local para a Cloud?**

   Em primeiro lugar, crie uma máquina virtual do Azure com uma instância do SQL Server. Em seguida, migre as suas bases de dados no local a essa instância. Para estratégias de migração de dados, consulte [migrar uma base de dados do SQL Server para o SQL Server numa VM do Azure](virtual-machines-windows-migrate-sql.md).

## <a name="licensing"></a>Licenças

1. **Como posso instalar minha cópia licenciada do SQL Server numa VM do Azure?**

   Existem duas formas de fazê-lo. Pode aprovisionar um da [imagens de máquinas virtuais que suporta licenças](virtual-machines-windows-sql-server-iaas-overview.md#BYOL), que também é conhecido como bring-your-própria licença (BYOL). Outra opção é copiar a mídia de instalação do SQL Server para uma VM do Windows Server, e, em seguida, instalar o SQL Server na VM. No entanto, se instalar manualmente o SQL Server, não há nenhuma integração do portal e a extensão do agente IaaS do SQL Server não é suportada, por isso, funcionalidades, tais como cópia de segurança automatizada e aplicação de patches automatizada não funcionará neste cenário. Por esse motivo, recomendamos que utilize uma das imagens da Galeria BYOL. Para utilizar o seu próprio suporte de dados do SQL Server ou de BYOL numa VM do Azure, tem de ter [mobilidade de licenças através do Software Assurance no Azure](https://azure.microsoft.com/pricing/license-mobility/). Para obter mais informações, consulte [Pricing guidance for SQL Server Azure VMs (Documentação de orientação sobre preços de VMs do Azure do SQL Server)](virtual-machines-windows-sql-server-pricing-guidance.md).

1. **Posso alterar uma VM para utilizar a minha própria licença do SQL Server, se ele foi criado a partir de uma das imagens de galeria pay as you go?**

   Não. Não é possível mudar do pagamento por segundo de licenciamento para a sua própria licença de utilização. Criar uma nova máquina virtual do Azure através de um da [imagens BYOL](virtual-machines-windows-sql-server-iaas-overview.md#BYOL), e, em seguida, migrar as bases de dados para o novo servidor com o padrão [técnicas de migração de dados](virtual-machines-windows-migrate-sql.md).

1. **Tenho de pagar a licença do SQL Server numa VM do Azure, se está a ser utilizado apenas para o modo de espera/ativação pós-falha?**

   Se tem o Software Assurance e utilizar a mobilidade de licenças, conforme descrito em [FAQ de licenciamento de Máquina Virtual](http://azure.microsoft.com/pricing/licensing-faq/) , em seguida, não tem de pagar o licenciamento do SQL Server a participar como réplica secundária passiva numa implantação de HA. Caso contrário, terá de pagar a licença-lo.


## <a name="administration"></a>Administração

1. **Pode instalar uma segunda instância do SQL Server na mesma VM? Pode alterar as funcionalidades instaladas da instância predefinida?**

   Sim. O suporte de dados de instalação do SQL Server está localizado numa pasta na **C** unidade. Execute **Setup.exe** partir dessa localização para adicionar novas instâncias do SQL Server ou para funcionalidades de alteração outros instalada do SQL Server na máquina. Tenha em atenção que alguns recursos, como a cópia de segurança automatizada, a aplicação de patches automatizada e integração do Cofre de chaves do Azure, só funcionam em relação a instância predefinida.

1. **Pode desinstalar a instância predefinida do SQL Server?**

   Sim, mas aqui estão algumas considerações. Conforme indicado na resposta anterior, funcionalidades que dependem da [extensão de agente IaaS do SQL Server](virtual-machines-windows-sql-server-agent-extension.md) operam apenas na instância predefinida. Se desinstalar a instância predefinida, a extensão continua a procurar por ele e poderá gerar erros de registo de eventos. Estes erros são das duas seguintes origens: **gestão de credenciais do Microsoft SQL Server** e **agente IaaS do Microsoft SQL Server**. Um dos erros pode ser semelhante ao seguinte:

      Ocorreu um erro relacionado com a rede ou específico da instância ao estabelecer uma ligação ao SQL Server. O servidor não foi encontrado ou não está acessível.

   Se optar por desinstalar a instância predefinida, também desinstalar o [extensão de agente IaaS do SQL Server](virtual-machines-windows-sql-server-agent-extension.md) também.

1. **Posso remover do SQL Server completamente de uma VM do SQL?**

   Sim, mas continuará a ser cobrada a VM do SQL conforme descrito em [preços orientações para VMs do Azure do SQL Server](virtual-machines-windows-sql-server-pricing-guidance.md). Se já não precisar do SQL Server, pode implementar uma nova máquina virtual e migrar os dados e aplicativos para a nova máquina virtual. Em seguida, pode remover a máquina virtual do SQL Server.
   
## <a name="updating-and-patching"></a>A atualização e aplicação de patches

1. **Como posso atualizar para uma nova versão/edição do SQL Server numa VM do Azure?**

   Atualmente, não existe nenhuma atualização no local para o SQL Server em execução na VM do Azure. Criar uma nova máquina virtual do Azure com a versão/edição pretendida do SQL Server e, em seguida, migre as suas bases de dados para o novo servidor com o padrão [técnicas de migração de dados](virtual-machines-windows-migrate-sql.md).

1. **Como são atualizações e service packs aplicadas numa VM do SQL Server?**

   Dão de máquinas virtuais-lhe controlo sobre o computador anfitrião, incluindo quando e como aplicar as atualizações. Para o sistema operativo, pode aplicar manualmente as atualizações do windows, ou pode ativar a um serviço de agendamento denominado [aplicação de patches automatizada](virtual-machines-windows-sql-automated-patching.md). A Aplicação de Patches Automatizada instala todas as atualizações marcadas como importantes, incluindo atualizações do SQL Server nessa categoria. As outras atualizações opcionais do SQL Server têm de ser instaladas manualmente.

## <a name="general"></a>Geral

1. **Instâncias de Cluster (FCI) do SQL Server ativação pós-falha são suportados em VMs do Azure?**

   Sim. Pode [criar um Cluster de ativação pós-falha do Windows no Windows Server 2016](virtual-machines-windows-portal-sql-create-failover-cluster.md) e utilizar espaços de armazenamento direto (S2D) para o armazenamento de cluster. Em alternativa, pode utilizar soluções de clustering ou de armazenamento de terceiros conforme descrito em [elevada disponibilidade e recuperação após desastre para o SQL Server em máquinas de virtuais do Azure](virtual-machines-windows-sql-high-availability-dr.md#azure-only-high-availability-solutions).

   > [!IMPORTANT]
   > Neste momento, o [extensão de agente IaaS do SQL Server](virtual-machines-windows-sql-server-agent-extension.md) não é suportada para a FCI do servidor SQL no Azure. Recomendamos que desinstale a extensão de VMs que participam do fci. Esta extensão oferece suporte a recursos, tais como cópia de segurança automatizada e a aplicação de patches e alguns recursos de portal para SQL. Estas funcionalidades não funcionarão para VMs de SQL após a desinstalação do agente.

1. **O que é a diferença entre as VMs do SQL e o serviço de base de dados SQL?**

   Conceitualmente, o SQL Server numa máquina virtual do Azure não, isso é diferente de executar o SQL Server num datacenter remoto. Por outro lado, [base de dados SQL](../../../sql-database/sql-database-technical-overview.md) oferece a base de dados-como-serviço. Base de dados SQL, não tem acesso aos computadores que alojam as bases de dados. Para obter uma comparação completa, consulte [escolha uma opção de SQL Server na nuvem: base de dados SQL do Azure (PaaS) ou o SQL Server em VMs do Azure (IaaS)](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md).

1. **Como posso instalar ferramentas de dados do SQL na minha VM do Azure?**

    Transferir e instalar as ferramentas de dados SQL a partir [Microsoft SQL Server Data Tools – Business Intelligence para Visual Studio 2013](https://www.microsoft.com/en-us/download/details.aspx?id=42313).

## <a name="resources"></a>Recursos

**VMs do Windows**:

* [Descrição geral do SQL Server num VM do Windows](virtual-machines-windows-sql-server-iaas-overview.md).
* [Aprovisionar um Windows do servidor SQL VM](virtual-machines-windows-portal-sql-server-provision.md)
* [Migrar uma base de dados para o SQL Server numa VM do Azure](virtual-machines-windows-migrate-sql.md)
* [Elevada disponibilidade e recuperação após desastre para SQL Server em máquinas virtuais do Azure](virtual-machines-windows-sql-high-availability-dr.md)
* [Melhores práticas de desempenho do SQL Server nas Máquinas Virtuais do Azure](virtual-machines-windows-sql-performance.md)
* [Padrões de Aplicação e Estratégias de Desenvolvimento para o SQL Server em Máquinas Virtuais do Azure](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)

**VMs do Linux**:

* [Descrição geral do SQL Server numa VM do Linux](../../linux/sql/sql-server-linux-virtual-machines-overview.md)
* [Aprovisionar uma VM de Linux do SQL Server](../../linux/sql/provision-sql-server-linux-virtual-machine.md)
* [FAQ (Linux)](../../linux/sql/sql-server-linux-faq.md)
* [Documentação do SQL Server no Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)
