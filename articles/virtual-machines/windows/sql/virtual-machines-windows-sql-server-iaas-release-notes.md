---
title: SQL Server nas notas de versão VM do Azure | Documentos da Microsoft
description: Saiba mais sobre os novos recursos e aprimoramentos do SQL Server numa VM do Azure
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/13/2018
ms.author: mathoma
ms.openlocfilehash: e3f44181ca1a5ea64815aadf52aa7ea792a21416
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2019
ms.locfileid: "54358580"
---
# <a name="sql-server-on-azure-virtual-machine-release-notes"></a>SQL Server nas notas de versão da Máquina Virtual do Azure

Azure permite-lhe implementar uma máquina virtual com uma imagem do SQL Server incorporado. Este artigo lista os novos recursos e aprimoramentos que pode esperar que a versão mais recente do SQL Server implementado numa máquina virtual do Azure. 

## <a name="december-2018"></a>Dezembro de 2018

| **Alteração** | Detalhes |
| --- | --- |
| **Novo fornecedor de recursos do grupo de cluster do SQL** | Há um novo fornecedor de recursos (Microsoft.SqlVirtualMachine/SqlVirtualMachineGroup) que define os metadados sobre o Cluster de ativação pós-falha do Windows. Associar uma VM do SQL Server para o *SqlVirtualMachineGroup* inicializa o serviço de Cluster de ativação pós-falha do Windows e associa a VM ao cluster.  |
|**Automatizar a configuração de uma implantação de grupo de disponibilidade com modelos de início rápido do Azure** |Agora é possível criar o Cluster de ativação pós-falha do Windows, Junte-se a VMs do SQL Server para o mesmo, criar o serviço de escuta e configurar o Balanceador de carga interno com dois modelos de início rápido do Azure. Para obter mais informações, consulte [criar WSFC, o serviço de escuta e configurar o ILB para um grupo de disponibilidade Always On numa VM do SQL Server com o modelo de início rápido do Azure](virtual-machines-windows-sql-availability-group-quickstart-template.md). | 
| **Registo do fornecedor de recursos de VM do SQL automática** | VMs do SQL Server implementado depois este mês são automaticamente registrados com o novo fornecedor de recursos do SQL Server. VMs do SQL Server implementadas antes deste mês ainda tem de ser registado manualmente. Para obter mais informações, consulte [registar a VM de SQL existente com o novo fornecedor de recursos](virtual-machines-windows-sql-ahb.md#register-existing-sql-server-vm-with-new-resource-provider).|
| &nbsp; | &nbsp; |


## <a name="november-2018"></a>Novembro de 2018

| **Alteração** | Detalhes |
| --- | --- |
| **Fornecedor de recursos de VM do SQL nova** |  Existe um fornecedor de recursos novo para VMs do SQL Server (Microsoft.SqlVirtualMachine) que permite um melhor gerenciamento de sua VM do SQL Server. Para obter mais informações sobre a registar a sua VM, consulte [registar a VM de SQL existente com o novo fornecedor de recursos](virtual-machines-windows-sql-ahb.md#register-existing-sql-server-vm-with-new-resource-provider). |
|**Mudar o modelo de licenciamento** |Agora pode alternar entre o modelo de pagamento por utilização e a trazer a sua própria licença para a VM do SQL através da CLI do Azure ou do Powershell. Para obter mais informações, consulte [como alterar o modelo de licenciamento de uma VM do SQL](virtual-machines-windows-sql-ahb.md). | 
| &nbsp; | &nbsp; |


## <a name="additional-resources"></a>Recursos adicionais

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
