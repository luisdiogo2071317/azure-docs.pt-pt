---
title: "Descrição Geral do SQL Server em Máquinas Virtuais do Azure do Windows | Microsoft Docs"
description: "Saiba como executar edições completas do SQL Server em Máquinas Virtuais do Azure."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
tags: azure-service-management
ms.assetid: c505089e-6bbf-4d14-af0e-dd39a1872767
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 12/14/2017
ms.author: jroth
ms.openlocfilehash: 95a18e4b2437ca628e84f6d6f580e12f2f26e111
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/16/2017
---
# <a name="what-is-sql-server-on-azure-virtual-machines-windows"></a>O que é o SQL Server nas Máquinas Virtuais do Azure? (Windows)

> [!div class="op_single_selector"]
> * [Windows](virtual-machines-windows-sql-server-iaas-overview.md)
> * [Linux](../../linux/sql/sql-server-linux-virtual-machines-overview.md)

O [SQL Server em máquinas virtuais do Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/) permite-lhe utilizar versões completas do SQL Server na nuvem sem ter de gerir qualquer hardware no local. As VMs do SQL Server também simplificam os custos de licenciamento quando paga em pay as you go.

As máquinas virtuais do Azure executam em [regiões geográficas](https://azure.microsoft.com/regions/) muito diferentes em todo o mundo. Também oferece uma variedade de [tamanhos de máquina](../sizes.md). A galeria de imagens da máquina virtual permite-lhe criar uma VM do SQL Server com a versão, edição e sistema operativo corretos. Isto faz com que as máquinas virtuais sejam uma boa opção para várias cargas de trabalho do SQL Server.

## <a name="automated-updates"></a>Atualizações automáticas

As VMs do SQL Server do Azure podem utilizar a [ Aplicação Automatizada de Patches](virtual-machines-windows-sql-automated-patching.md) para agendar uma janela de manutenção para a instalação automática de atualizações importantes do Windows e do SQL Server.

## <a name="automated-backups"></a>Cópias de segurança automatizadas

As VMs do SQL Server do Azure podem tirar partido da [Cópia de Segurança Automatizada](virtual-machines-windows-sql-automated-backup-v2.md), que cria regularmente cópias de segurança da sua base de dados para o armazenamento de blobs. Pode também utilizar esta técnica manualmente. Para obter mais informações, veja [Use Azure Storage for SQL Server Backup and Restore (Utilizar o Armazenamento do Azure para o Restauro e a Cópia de Segurança do SQL Server)](virtual-machines-windows-use-storage-sql-server-backup-restore.md).

## <a name="high-availability"></a>Elevada disponibilidade

Se necessitar de elevada disponibilidade, considere configurar Grupos de Disponibilidade do SQL Server. Isto envolve várias VMs do SQL Server do Azure numa rede virtual. Pode configurar a sua solução de elevada disponibilidade manualmente ou pode utilizar modelos no portal do Azure para a configuração automática. Para uma vista geral de todas as opções de elevada disponibilidade, consulte o artigo [Elevada Disponibilidade e Recuperação Após Desastre para o SQL Server em Virtual Machines do Azure](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="performance"></a>Desempenho

As máquinas virtuais do Azure oferecem tamanhos de máquina diferentes para satisfazer vários pedidos de carga de trabalho. As VMs de SQL também fornecer configuração de armazenamento automática, que está otimizada para os seus requisitos de desempenho. Para obter mais informações sobre como configurar o armazenamento para as VMs SQL, veja [Configuração de armazenamento para VMs de SQL Server](virtual-machines-windows-sql-server-storage-configuration.md). Para desempenho bem ajustado, veja [Melhores práticas de desempenho do SQL Server nas Máquinas Virtuais do Azure (Performance best practices for SQL Server in Azure Virtual Machines)](virtual-machines-windows-sql-performance.md).

## <a name="get-started-with-sql-vms"></a>Introdução às VMs SQL

Para começar a utilizar, escolha uma imagem de máquina virtual do SQL Server com a versão, edição e sistema operativo necessários. As secções seguintes fornecem ligações diretas para o portal do Azure para as imagens da galeria das máquinas virtuais do SQL Server.

> [!TIP]
> Para compreender os preços da VM e do SQL destas imagens, consulte [Pricing guidance for SQL Server Azure VMs (Documentação de orientação sobre preços das VMs do Azure do SQL Server)](virtual-machines-windows-sql-server-pricing-guidance.md).

> [!TIP]
> Para compreender a política de atualização e o ciclo de vida para imagens de galeria da máquina virtual do SQL Server, veja as [FAQ de VMs do SQL Server](virtual-machines-windows-sql-server-iaas-faq.md#images).

### <a id="payasyougo"></a> Pay as you go
A tabela seguinte fornece uma matriz de imagens do SQL Server pay as you go.

| Versão | Sistema Operativo | Edição |
| --- | --- | --- |
| **SQL Server 2017** |Windows Server 2016 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseWindowsServer2016), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonWindowsServer2016), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonWindowsServer2016), [Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonWindowsServer2016), [Programador](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonWindowsServer2016) |
| **SQL Server 2016 SP1** |Windows Server 2016 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1EnterpriseWindowsServer2016), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1StandardWindowsServer2016), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1WebWindowsServer2016), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1ExpressWindowsServer2016), [Programador](https://portal.azure.com/#create/Microsoft.SQLServer2016SP1DeveloperWindowsServer2016) |
| **SQL Server 2014 SP2** |Windows Server 2012 R2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2014SP2ExpressWindowsServer2012R2) |
| **SQL Server 2012 SP3** |Windows Server 2012 R2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3ExpressWindowsServer2012R2) |
| **SQL Server 2008 R2 SP3** |Windows Server 2008 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3EnterpriseWindowsServer2008R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3StandardWindowsServer2008R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3WebWindowsServer2008R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3ExpressWindowsServer2008R2) |

Para ver as imagens de máquina virtual do SQL Server do Linux disponíveis, veja [Overview of SQL Server on Azure Virtual Machines (Linux) (Descrição Geral do SQL Server em Máquinas Virtuais do Azure (Linux))](../../linux/sql/sql-server-linux-virtual-machines-overview.md).

### <a id="BYOL"></a> Traga a sua própria licença
Também pode trazer a sua licença (BYOL). Neste cenário, apenas paga a VM sem quaisquer taxas adicionais para o licenciamento do SQL Server.  Colocar a sua própria licença poderá poupar dinheiro ao longo do tempo para cargas de trabalho de produção contínua. Para saber sobre os requisitos necessários para utilizar esta opção, veja [Pricing guidance for SQL Server Azure VMs (Documentação de orientação sobre preços de VMs do Azure do SQL Server)](virtual-machines-windows-sql-server-pricing-guidance.md).

| Versão | Sistema operativo | Edição |
| --- | --- | --- |
| **SQL Server 2017** |Windows Server 2016 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2017EnterpriseWindowsServer2016), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2017StandardonWindowsServer2016) |
| **SQL Server 2016 SP1** |Windows Server 2016 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1EnterpriseWindowsServer2016), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1StandardWindowsServer2016) |
| **SQL Server 2014 SP2** |Windows Server 2012 R2 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP2EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP2StandardWindowsServer2012R2) |
| **SQL Server 2012 SP2** |Windows Server 2012 R2 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3StandardWindowsServer2012R2) |

No portal, é adicionado o prefixo **{BYOL}** aos nomes destas imagens.

### <a name="connect-to-the-vm"></a>Ligar à VM
Depois de criar a VM do SQL Server, ligue-se à mesma a partir de aplicações ou ferramentas, tais como o SQL Server Management Studio (SSMS). Para instruções, veja [Ligar a uma Máquina Virtual do SQL Server no Azure](virtual-machines-windows-sql-connect.md).

### <a name="migrate-your-data"></a>Migrar os dados
Se tiver uma base de dados existente, deverá movê-la para a VM do SQL recentemente aprovisionada. Para obter uma lista de opções de migração e orientações, consulte o artigo [Migrar uma Base de Dados para o SQL Server numa VM do Azure](virtual-machines-windows-migrate-sql.md).

## <a name="customer-experience-improvement-program-ceip"></a>Programa de melhoramento da experiência do cliente (PMEC)
O Programa de Melhoramento da Experiência do Cliente (PMEC) está ativado por predefinição. Esta ação envia relatórios periodicamente para a Microsoft, para ajudar a melhorar o SQL Server. Não é precisa nenhuma tarefa de gestão com PMEC, a menos que queira desativá-la após o aprovisionamento. Pode personalizar ou desativar o PMEC ligando-se à VM com o ambiente de trabalho remoto. Em seguida, execute o utilitário **Relatórios de Utilização e Erros do SQL Server**. Siga as instruções para desativar os relatórios. Para obter mais informações sobre a recolha de dados, veja a [Declaração de Privacidade do SQL Server](https://www.microsoft.com/EN-US/privacystatement/SQLServer/Default.aspx).

## <a name="related-products-and-services"></a>Produtos e serviços relacionados
### <a name="windows-virtual-machines"></a>Máquinas Virtuais do Windows
* [Descrição Geral das Máquinas Virtuais](../overview.md)

### <a name="storage"></a>Armazenamento
* [Introdução ao Armazenamento do Microsoft Azure](../../../storage/common/storage-introduction.md)

### <a name="networking"></a>Redes
* [Descrição Geral da Rede Virtual](../../../virtual-network/virtual-networks-overview.md)
* [Endereços IP do Azure](../../../virtual-network/virtual-network-ip-addresses-overview-arm.md)
* [Create a Fully Qualified Domain Name in the Azure portal](../portal-create-fqdn.md) (Criar um Nome de Domínio Completamente Qualificado no portal do Azure)

### <a name="sql"></a>SQL
* [Documentação do SQL Server](https://docs.microsoft.com/sql/index)
* [Comparação da Base de Dados SQL do Azure](../../../sql-database/sql-database-paas-vs-sql-server-iaas.md)

## <a name="next-steps"></a>Passos seguintes

Introdução ao SQL Server em máquinas virtuais do Azure:

* [Criar uma VM do SQL Server no portal do Azure](quickstart-sql-vm-create-portal.md)

Obtenha respostas às perguntas mais comuns sobre as VMs do SQL:

* [FAQ do SQL Server nas Máquinas Virtuais do Azure](virtual-machines-windows-sql-server-iaas-faq.md)
