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
ms.openlocfilehash: 44aee447c7f935cd67ca55902c53c5f6f9eb6fda
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/28/2018
ms.locfileid: "52500418"
---
# <a name="sql-server-on-azure-virtual-machine-release-notes"></a>SQL Server nas notas de versão da Máquina Virtual do Azure

Azure permite-lhe implementar uma máquina virtual com uma imagem do SQL Server incorporado. Este artigo lista os novos recursos e aprimoramentos que pode esperar que a versão mais recente do SQL Server implementado numa máquina virtual do Azure. 


## <a name="november-2018"></a>Novembro de 2018
- **Fornecedor de recursos do novo SQL**: existe um fornecedor de recursos novo para VMs de SQL que permitem a melhor gerenciamento da sua VM. Para obter mais informações sobre a registar a sua VM, consulte [registar herdados VM do SQL Server com o novo fornecedor de recursos](virtual-machines-windows-sql-ahb.md#register-legacy-sql-vm-with-new-resource-provider).
- **Mudar o modelo de licenciamento**: agora pode alternar entre o modelo de pagamento por utilização e a trazer a sua própria licença para a VM do SQL através da CLI do Azure ou do Powershell. Para obter mais informações, consulte [como alterar o modelo de licenciamento de uma VM do SQL](virtual-machines-windows-sql-ahb.md)



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
