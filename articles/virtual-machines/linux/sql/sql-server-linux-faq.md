---
title: "SQL Server em máquinas virtuais do Linux Azure FAQ | Microsoft Docs"
description: "Este artigo fornece respostas às perguntas mais frequentes sobre a execução do SQL Server em VMs do Linux do Azure."
services: virtual-machines-linux
documentationcenter: 
author: rothja
manager: jhubbard
tags: azure-service-management
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: troubleshooting
ms.workload: iaas-sql-server
ms.date: 12/13/2017
ms.author: jroth
ms.openlocfilehash: 8b556b01aa47aeb3588138dfa61e517c00dc44dc
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2017
---
# <a name="frequently-asked-questions-for-sql-server-on-linux-azure-virtual-machines"></a>Perguntas mais frequentes para o SQL Server no Linux Virtual Machines do Azure

> [!div class="op_single_selector"]
> * [Windows](../../windows/sql/virtual-machines-windows-sql-server-iaas-faq.md)
> * [Linux](sql-server-linux-faq.md)

Este artigo fornece respostas a algumas perguntas mais comuns sobre a execução [do SQL Server no Linux Virtual Machines do Azure](sql-server-linux-virtual-machines-overview.md).

> [!NOTE]
> Este artigo foca-se nos problemas específicos ao SQL Server em VMs do Linux. Se estiver a executar o SQL Server em VMs do Windows, consulte o [Windows FAQ](../../windows/sql/virtual-machines-windows-sql-server-iaas-faq.md).

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a id="images"></a>Imagens

1. **As imagens de Galeria de máquina virtual do SQL Server estão disponíveis?**

   Azure mantém imagens da máquina virtual para todas as versões principais suportadas do SQL Server em todas as edições para Linux e Windows. Para obter mais detalhes, consulte a lista completa de [imagens de VM com Linux](sql-server-linux-virtual-machines-overview.md#create) e [imagens da VM do Windows](../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md#payasyougo).

1. **Imagens da galeria da máquina virtual do SQL Server existentes são atualizadas?**

   A cada dois meses, imagens do SQL Server na galeria da máquina virtual são atualizadas com o Linux mais recente e atualizações do Windows. Para imagens de Linux, isto inclui as atualizações mais recentes do sistema. Para as imagens do Windows, isto inclui quaisquer atualizações que estão marcadas como importantes no Windows Update, incluindo atualizações de segurança importantes do SQL Server e service packs. As atualizações cumulativas do SQL Server são processadas de forma diferente para Linux e Windows. Para Linux, atualizações cumulativas do SQL Server também estão incluídas na atualização. Mas neste momento, o VMs do Windows não são atualizadas com atualizações cumulativas do SQL Server ou Windows Server.

1. **O que relacionados com pacotes de SQL Server também são instalados?**

   Para ver os pacotes de SQL Server que são instalados por predefinição em VMs do Linux do SQL Server, consulte [instalados pacotes](sql-server-linux-virtual-machines-overview.md#packages).

1. **Imagens da máquina virtual do SQL Server podem obter removidas da galeria do?**

   Sim. Azure mantém apenas uma imagem por versão principal e a edição. Por exemplo, quando for lançado um novo service pack do SQL Server, o Azure adiciona uma nova imagem para a Galeria para esse pacote de serviço. A imagem do SQL Server para o service pack anterior é imediatamente removida do portal do Azure. No entanto, é ainda disponível para o aprovisionamento a partir do PowerShell para os três meses. Após três meses, a imagem do pacote de serviço anterior já não está disponível. Esta política de remoção seria também se aplicam se uma versão do SQL Server se tornar não suportada quando chegar ao fim do respetivo ciclo de vida.

## <a name="creation"></a>Criação

1. **Como posso criar uma máquina virtual do Azure do Linux com o SQL Server?**

   A solução mais fácil consiste em criar uma Máquina Virtual de Linux que inclui o SQL Server. Para um tutorial de inscrição no Azure e criar uma VM do SQL a partir do portal, consulte [aprovisionar uma máquina virtual do Linux SQL Server no portal do Azure](provision-sql-server-linux-virtual-machine.md). Também tem a opção de instalar manualmente do SQL Server numa VM com o uma edição licenciada livremente (programador ou Express) ou através da reutilização de uma licença no local. Se utilizar a sua própria licença, tem de ter [licença mobilidade através do Software Assurance no Azure](https://azure.microsoft.com/pricing/license-mobility).

1. **Por que motivo não é possível aprovisionar uma RHEL ou SLES VM do SQL Server com uma subscrição do Azure que tenha um limite de gastos?**

   Máquinas virtuais RHEL e SLES requerem uma subscrição com limite de gastos e um método de pagamento verificado (normalmente, um cartão de crédito) associadas à subscrição. Se aprovisionar um RHEL ou SLES VM sem remover o limite de gastos, a sua subscrição será desativada e todas as VMs/serviços parados. Se depare com este estado, voltar a ativar a subscrição [remover o limite de gastos](https://account.windowsazure.com/subscriptions). Os créditos restantes vão ser restaurados para o ciclo de faturação atual, mas um surcharge de imagem RHEL ou SLES VM passará contra o seu cartão de crédito, se optar por voltar a iniciar e continuar a executá-lo.

## <a name="licensing"></a>Licenças

1. **Como instalar a minha cópia licenciada do SQL Server numa VM do Azure?**

   Em primeiro lugar, crie uma máquina de virtual com Linux como só de SO. Em seguida, execute o [passos de instalação do SQL Server](https://docs.microsoft.com/sql/linux/sql-server-linux-setup#platforms) para a distribuição de Linux. A menos que estiver a instalar uma das edições livremente licenciadas do SQL Server, também tem de ter uma licença do SQL Server e [licença mobilidade através do Software Assurance no Azure](https://azure.microsoft.com/pricing/license-mobility/).

1. **Existem imagens da máquina virtual Linux Bring-Your-proprietário-licença (BYOL) para o SQL Server?**

   Neste momento, não existem imagens da máquina virtual não BYOL Linux para o SQL Server. No entanto, pode instalar manualmente do SQL Server numa VM apenas de Linux como explicado em perguntas anterior.

1. **Pode alterar uma VM para utilizar o meu próprio licença do SQL Server se foi criada a partir de uma das imagens gallery pay as you go?**

   Não. Não é possível mudar do licenciamento de pagamento por minuto para a sua própria licença de utilização. Tem de criar uma nova VM do Linux, instalar o SQL Server e migrar os dados. Consulte a pergunta anterior para obter mais detalhes sobre como colocar a sua própria licença.

## <a name="administration"></a>Administração

1. **Pode gerir uma máquina virtual de Linux SQL Server com o SQL Server Management Studio (SSMS)?**

   Sim, mas o SSMS atualmente é uma ferramenta como só de Windows. Deve ligar remotamente a partir de uma máquina Windows para utilizar o SSMS com VMs do Linux SQL Server. Localmente no Linux, a nova [mssql-conf](https://docs.microsoft.com/sql/linux/sql-server-linux-configure-mssql-conf) ferramenta pode executar muitas tarefas administrativas. Para pré-visualizar uma ferramenta de gestão de base de dados de várias plataformas, consulte [(pré-visualização) do SQL Server operações Studio](https://docs.microsoft.com/sql/sql-operations-studio/what-is).

## <a name="updating-and-patching"></a>A atualização e aplicação de patches

1. **Como atualizar para uma nova versão/edição do SQL Server numa VM do Azure?**

   Atualmente, não há nenhuma atualização no local para o SQL Server em execução numa VM do Azure. Criar uma nova máquina virtual do Azure com a versão/edição de SQL Server pretendido e, em seguida, migre as bases de dados para o novo servidor utilizando [técnicas de migração de dados padrão](https://docs.microsoft.com/sql/linux/sql-server-linux-migrate-overview).

## <a name="general"></a>Geral

1. **Soluções de elevada disponibilidade do SQL Server são suportadas em VMs do Azure?**

   Neste momento, não. Grupos de disponibilidade Always e Clustering de ativação pós-falha necessitam uma solução de clustering no Linux, tais como Pacemaker. As distribuições Linux suportadas para o SQL Server não suportam os suplementos de elevada disponibilidade na nuvem.

## <a name="resources"></a>Recursos

**VMs com Linux**:

* [Descrição geral do SQL Server numa VM com Linux](sql-server-linux-virtual-machines-overview.md)
* [Aprovisionar uma VM com Linux do SQL Server](provision-sql-server-linux-virtual-machine.md)
* [SQL Server na documentação do Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)

**VMs do Windows**:

* [Descrição geral do SQL Server numa VM do Windows](../../windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)
* [Aprovisionar um SQL Server do Windows VM](../../windows/sql/virtual-machines-windows-portal-sql-server-provision.md)
* [FAQ (Windows)](../../windows/sql/virtual-machines-windows-sql-server-iaas-faq.md)