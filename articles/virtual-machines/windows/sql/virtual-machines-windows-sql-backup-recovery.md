---
title: Cópia de segurança e restaurar para o SQL Server em VMs do Azure | Microsoft Docs
description: Descreve as considerações de cópia de segurança e restauro de bases de dados do SQL Server em execução em máquinas de virtuais do Azure.
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: craigg
editor: ''
tags: azure-resource-management
ms.assetid: 95a89072-0edf-49b5-88ed-584891c0e066
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/04/2018
ms.author: mikeray
ms.openlocfilehash: 29abd62b0602686c912f022ec8870dcb6f1b39eb
ms.sourcegitcommit: 0a7c9629dea6f5eb52cc45a1e31112def210dfc2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34730888"
---
# <a name="backup-and-restore-for-sql-server-in-azure-virtual-machines"></a>Cópia de Segurança e Restauro para SQL Server em Máquinas Virtuais do Azure

Este artigo fornece orientação sobre as cópia de segurança e restauro as opções disponíveis para o SQL Server em execução no Windows Virtual Machines do Azure. Armazenamento do Azure mantém três cópias de cada disco da VM do Azure para garantir a proteção contra perda de dados ou danos nos dados físico. Assim, ao contrário no local, não precisa de para se focarem em falhas de hardware. No entanto, deve ainda efetuar cópias de segurança das bases de dados do SQL Server para proteger contra erros de aplicações ou utilizadores, tais como inserções dados inadvertida ou eliminações. Nesta situação, é importante ser capaz de restaurar para um ponto específico no tempo.

A primeira parte deste artigo fornece uma descrição geral da cópia de segurança disponível e as opções de restauro. Isto é seguido de secções fornecem mais informações sobre cada estratégia.

## <a name="backup-and-restore-options"></a>Opções de cópia de segurança e restauro

A tabela seguinte fornece informações sobre várias opções de cópia de segurança e restauro para o SQL Server em execução em VMs do Azure:

| Estratégia de | Versões do SQL Server | Descrição |
|---|---|---|---|
| [Cópia de Segurança Automatizada](#automated) | 2014<br/> 2016<br/> 2017 | Cópia de segurança automatizada permite-lhe agendar cópias de segurança regulares para todas as bases de dados uma VM do SQL Server. As cópias de segurança são armazenadas no armazenamento do Azure para 30 dias. A partir do SQL Server 2016, a cópia de segurança automatizada v2 oferece opções adicionais, tais como a configuração manual de agendamento e a frequência de completo e cópias de segurança do registo. |
| [Backup do Azure para as VMs do SQL Server](#azbackup) | 2012<br/> 2014<br/> 2016<br/> 2017 | Cópia de segurança do Azure fornece uma capacidade de cópia de segurança de classe empresarial para o SQL Server em execução em VMs do Azure. Com este serviço, pode gerir centralmente as cópias de segurança para vários servidores e milhares de bases de dados. Bases de dados podem ser restauradas para um ponto específico no tempo no portal. Oferece uma política de retenção personalizáveis que pode manter as cópias de segurança para anos. Esta funcionalidade está atualmente em pré-visualização pública. |
| [Cópia de segurança manual](#manual) | Todos | Dependendo da versão do SQL Server, existem várias técnicas manualmente de cópia de segurança e restaurar o SQL Server em execução numa VM do Azure. Neste cenário, é responsável pela forma como as bases de dados são uma cópia de segurança e a localização de armazenamento e gestão destas cópias de segurança. |

As secções seguintes descrevem cada opção em mais detalhe. A secção final deste artigo fornece um resumo sob a forma de uma matriz de funcionalidade.

## <a id="autoamted"></a> Cópia de segurança automatizada

Cópia de segurança automatizada fornece um serviço de cópia de segurança automático para SQL Server Standard e Enterprise edições em execução na VM do Windows Azure. Este serviço é fornecido pelo [extensão de agente do SQL Server IaaS](virtual-machines-windows-sql-server-agent-extension.md), que é automaticamente instalada nas imagens de máquina virtual do SQL Server do Windows no portal do Azure.

Todas as bases de dados são cópias de segurança para uma conta de armazenamento do Azure que configurar. As cópias de segurança podem ser encriptadas e mantidas até 30 dias.

SQL Server 2016 e VMs superiores oferecem mais opções de personalização com cópia de segurança automatizada v2. Estes melhoramentos incluem:

- Cópias de segurança do sistema da base de dados
- Agenda de cópia de segurança manual e a janela de tempo
- Frequência de cópia de segurança de ficheiro completo e de registo

Para restaurar uma base de dados, tem de localizar os ficheiros de cópia de segurança necessários na conta de armazenamento e efetue um restauro na sua VM do SQL Server utilizando o SQL Server Management Studio (SSMS) ou por comandos de Transact-SQL.

Para obter mais informações sobre como configurar a cópia de segurança automatizada para VMs de SQL, consulte um dos seguintes artigos:

- **SQL Server 2016/2017**: [automatizada v2 cópia de segurança para máquinas virtuais do Azure ](virtual-machines-windows-sql-automated-backup-v2.md)
- **SQL Server 2014**: [cópia de segurança automatizada para máquinas virtuais do SQL Server 2014](virtual-machines-windows-sql-automated-backup.md)

## <a id="azbackup"></a> Backup do Azure para as VMs do SQL Server

[Cópia de segurança do Azure](/azure/backup/) proporciona uma capacidade de cópia de segurança de classe empresarial para o SQL Server em execução em VMs do Azure. Todas as cópias de segurança são armazenadas e geridas num cofre dos serviços de recuperação. Existem várias vantagens que fornece esta solução, especialmente para empresas:

- **Cópia de segurança da infraestrutura de zero**: não é necessário que gerir servidores de cópia de segurança ou localizações de armazenamento.
- **Escala**: proteger várias VMs de SQL e milhares de bases de dados.
- **Pay as you go**: esta capacidade é um serviço separado fornecido pela cópia de segurança do Azure, mas tal como acontece com todos os serviços do Azure, apenas paga a utiliza.
- **Monitorização e gestão centralizada**: gerir centralmente todas as cópias de segurança, incluindo outras cargas de trabalho que suporte a cópia de segurança do Azure, de um único dashboard no Azure.
- **Política suscitada pelo departamento de cópia de segurança e retenção**: criar políticas de cópia de segurança padrão para cópias de segurança regulares. Estabelece políticas de retenção para manter as cópias de segurança para anos.
- **Suporte para o SQL Always On**: detetar e proteger uma configuração de SQL Server Always On e honrar a preferência de cópia de segurança do grupo de disponibilidade cópia de segurança.
- **o objetivo de ponto de recuperação de 15 minutos (RPO)**: cópias de segurança do SQL Server configurar transação registo até a cada 15 minutos.
- **Ponto de restauro de tempo**: utilizar o portal para recuperar bases de dados para um ponto específico no tempo sem ter de restaurar manualmente os vários completo, diferencial e as cópias de segurança de registo.
- **Consolidados alertas por e-mail para falhas**: Configurar consolidados notificações por e-mail para eventuais falhas.
- **Controlo de acesso baseado em funções**: determinar que pode gerir a cópia de segurança e restaurar as operações através do portal.

Para uma descrição geral de como funciona, juntamente com uma demonstração, veja o vídeo seguinte:

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2dNbw]

Esta solução de cópia de segurança do Azure para as VMs de SQL está atualmente em pré-visualização pública. Para obter mais informações, consulte [cópia de segurança da base de dados do SQL Server para o Azure](../../../backup/backup-azure-sql-database.md).

## <a id="manual"></a> Cópia de segurança manual

Se pretender gerir cópia de segurança e restaurar as operações em VMs do SQL Server manualmente, existem várias opções consoante a versão do SQL Server estiver a utilizar. Para obter uma descrição geral da cópia de segurança e restauro, consulte um dos seguintes artigos com base na sua versão do SQL Server:

- [Cópia de segurança e restauro do SQL Server 2016 e posterior](https://docs.microsoft.com/en-us/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases)
- [Cópia de segurança e restauro para o SQL Server 2014](https://msdn.microsoft.com/en-us/library/ms187048%28v=sql.120%29.aspx)
- [Cópia de segurança e restauro para o SQL Server 2012](https://msdn.microsoft.com/library/ms187048%28v=sql.110%29.aspx)
- [Cópia de segurança e restauro do SQL Server SQL Server 2008 R2](https://msdn.microsoft.com/library/ms187048%28v=sql.105%29.aspx)
- [Cópia de segurança e restauro para o SQL Server 2008](https://msdn.microsoft.com/library/ms187048%28v=sql.100%29.aspx)

As secções seguintes descrevem várias cópia de segurança manual e opções mais detalhado de restauro.

### <a name="backup-to-attached-disks"></a>Cópia de segurança em discos ligados

Para o SQL Server em execução em VMs do Azure, pode utilizar a cópia de segurança nativa e restaurar técnicas utilizando discos ligados na VM para o destino dos ficheiros de cópia de segurança. No entanto, há um limite para o número de discos pode anexar a uma máquina virtual do Azure, com base no [tamanho da máquina virtual](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Também é o overhead inerente à gestão de discos a ter em consideração.

Para obter um exemplo de como criar manualmente uma cópia de segurança completa da base de dados utilizando o SQL Server Management Studio (SSMS) ou Transact-SQL, consulte [criar uma base de dados de cópia de segurança completa](https://docs.microsoft.com/sql/relational-databases/backup-restore/create-a-full-database-backup-sql-server).

### <a name="backup-to-url"></a>Cópia de segurança para URL

A partir do SQL Server 2012 SP1 CU2, pode criar cópias de segurança e restaurar diretamente ao armazenamento de Blobs do Microsoft Azure, que também é conhecido como cópia de segurança para URL. SQL Server 2016 também introduziu os seguintes melhoramentos para esta funcionalidade:

| Melhoramento de 2016 | Detalhes |
| --- | --- |
| **Striping** |Quando a cópia de segurança para armazenamento de Blobs do Microsoft Azure, o SQL Server 2016 suporta a cópia de segurança em vários blobs para ativar a cópia de segurança de bases de dados grandes, até um máximo de 12.8 TB. |
| **Cópia de segurança de instantâneos** |Através da utilização de instantâneos do Azure, a cópia de segurança do instantâneo de ficheiro do SQL Server fornece quase instantânea cópias de segurança e restauros rápidos para ficheiros de base de dados armazenados com o serviço de armazenamento de Blobs do Azure. Esta capacidade permite-lhe simplificar a cópia de segurança e restaurar as políticas. Instantâneo de ficheiro de cópia de segurança também suporta ponto de restauro de tempo. Para obter mais informações, consulte [cópias de segurança de instantâneos de ficheiros de base de dados no Azure](https://docs.microsoft.com/en-us/sql/relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure). |

Para obter mais informações, consulte um dos seguintes artigos com base na sua versão do SQL Server:

- **SQL Server 2016/2017**: [cópia de segurança do SQL Server para o URL](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service)
- **SQL Server 2014**: [cópia de segurança do SQL Server 2014 para o URL](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx)
- **SQL Server 2012**: [cópia de segurança do SQL Server 2012 para o URL](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx)

### <a name="managed-backup"></a>Cópia de segurança gerida

A partir do SQL Server 2014, cópia de segurança gerida automatiza a criação de cópias de segurança para o armazenamento do Azure. Nos bastidores, a cópia de segurança gerida permite a utilização de cópia de segurança a funcionalidade de URL, descrita na secção anterior deste artigo. Cópia de segurança gerida, também é a funcionalidade subjacente que suporta o serviço do SQL Server VM cópia de segurança automatizada.

A partir do SQL Server 2016, a cópia de segurança gerida obteve opções adicionais para agendamento, base de dados do sistema completa e cópia de segurança e a frequência de cópia de segurança do registo.

Para obter mais informações, consulte um dos seguintes artigos com base na sua versão do SQL Server:

- [Gerido cópia de segurança para o Microsoft Azure para o SQL Server 2016 e posterior](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure)
- [Cópia de segurança gerida no Microsoft Azure para o SQL Server 2014](https://msdn.microsoft.com/library/dn449496%28v=sql.120%29.aspx)

## <a name="decision-matrix"></a>Matriz de decisão

A tabela seguinte resume as capacidades de cada opção de cópia de segurança e restauro de máquinas virtuais do SQL Server no Azure.

|| **Cópia de Segurança Automatizada** | **Backup do Azure para as VMs do SQL Server** | **Cópia de segurança manual** |
|---|---|---|---|
| Requer que o serviço do Azure adicional |   | ![Sim](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Configurar a política de cópia de segurança no portal do Azure | ![Sim](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Sim](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Restaurar bases de dados no portal do Azure |   | ![Sim](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Gerir vários servidores num único dashboard |   | ![Sim](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Restauro para um ponto anterior no tempo | ![Sim](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Sim](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Sim](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |
| o objetivo de ponto de recuperação de 15 minutos (RPO) | ![Sim](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Sim](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Sim](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |
| Política de retenção de cópias de segurança de curta duração (dias) | ![Sim](./media/virtual-machines-windows-sql-backup-recovery/yes.png) | ![Sim](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Política de retenção de cópias de segurança de longa duração (meses, anos) |   | ![Sim](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Suporte incorporado para SQL Server Always On |   | ![Sim](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Alertas de e-mail consolidados para falhas |   | ![Sim](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Controlo de acesso baseado em funções |   | ![Sim](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Cópia de segurança para o Cofre dos serviços de recuperação |   | ![Sim](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   |
| Cópia de segurança para as contas de armazenamento do Azure | ![Sim](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   | ![Sim](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |
| Cópia de segurança em discos ligados na VM |   |   | ![Sim](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |
| Restaurar bases de dados com scripts SSMS ou Transact-SQL | ![Sim](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |   | ![Sim](./media/virtual-machines-windows-sql-backup-recovery/yes.png) |

## <a name="next-steps"></a>Passos Seguintes

Se estiver a planear a implementação do SQL Server numa VM do Azure, pode encontrar orientações aprovisionamento no seguinte guia: [como aprovisionar uma máquina virtual do Windows SQL Server no portal do Azure](virtual-machines-windows-portal-sql-server-provision.md).

Apesar de cópia de segurança e restauro podem ser utilizados para migrar os dados, existem caminhos de migração de dados potencialmente mais fácil para o SQL Server numa VM do Azure. Para ver um debate completo de opções de migração e as recomendações, consulte [migrar uma base de dados para o SQL Server numa VM do Azure](virtual-machines-windows-migrate-sql.md).