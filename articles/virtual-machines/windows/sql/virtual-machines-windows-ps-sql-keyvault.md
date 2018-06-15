---
title: Integrar o Cofre de chaves com o SQL Server em VMs do Windows no Azure (Gestor de recursos) | Microsoft Docs
description: Saiba como automatizar a configuração de encriptação do SQL Server para utilização com o Cofre de chaves do Azure. Este tópico explica como utilizar a integração do Cofre de chaves do Azure com máquinas virtuais do SQL Server criadas com o Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: rothja
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: cd66dfb1-0e9b-4fb0-a471-9deaf4ab4ab8
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/30/2018
ms.author: jroth
ms.openlocfilehash: 2b398f59aed1610825f495a6089990d393531305
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/03/2018
ms.locfileid: "32774731"
---
# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-virtual-machines-resource-manager"></a>Configurar a integração do Cofre de chaves do Azure para o SQL Server em Virtual Machines do Azure (Gestor de recursos)

> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-ps-sql-keyvault.md)
> * [Clássico](../sqlclassic/virtual-machines-windows-classic-ps-sql-keyvault.md)

## <a name="overview"></a>Descrição geral
Existem várias funcionalidades de encriptação do SQL Server, tais como [encriptação de dados transparente (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [encriptação de nível de coluna (CLE)](https://msdn.microsoft.com/library/ms173744.aspx), e [encriptação de cópias de segurança](https://msdn.microsoft.com/library/dn449489.aspx). Estes formulários da encriptação tem de gerir e armazenar as chaves criptográficas que utiliza para a encriptação. O serviço Cofre de chaves do Azure (AKV) foi concebido para melhorar a segurança e gestão destas chaves numa localização segura e altamente disponível. O [conector do SQL Server](http://www.microsoft.com/download/details.aspx?id=45344) permite que o SQL Server para utilizar estas chaves a partir do Cofre de chaves do Azure.

Se estiver a executar o SQL Server com máquinas no local, existem [passos que pode seguir para aceder ao Cofre de chaves do Azure a partir do seu computador do SQL Server no local](https://msdn.microsoft.com/library/dn198405.aspx). Mas para o SQL Server em VMs do Azure, pode poupar tempo ao utilizar o *integração do Cofre de chaves do Azure* funcionalidade.

Quando esta funcionalidade está ativada, instala o conector do SQL Server, configura automaticamente o fornecedor EKM para aceder ao Cofre de chaves do Azure e cria a credencial para que possa aceder ao seu cofre. Se tem em conta os passos na documentação do mencionadas anteriormente no local, pode ver que esta funcionalidade automatiza os passos 2 e 3. É a única coisa que ainda terá de efetuar manualmente a criação do Cofre de chaves e as chaves. A partir daí, a configuração completa da sua VM do SQL Server é um processo automatizada. Depois desta funcionalidade concluída esta configuração, pode executar as instruções T-SQL para começar a encriptar as cópias de segurança ou bases de dados, como faria normalmente.

[!INCLUDE [AKV Integration Prepare](../../../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="enabling-and-configuring-akv-integration"></a>Ativar e configurar a integração AKV
Pode ativar a integração AKV durante o aprovisionamento ou configurá-la para VMs existentes.

### <a name="new-vms"></a>Novas VMs
Se estiver a aprovisionar uma nova máquina de virtual do SQL Server com o Resource Manager, o portal do Azure fornece uma forma de ativar a integração do Cofre de chaves do Azure. A funcionalidade do Cofre de chaves do Azure está disponível apenas para o Enterprise, Developer e edições de avaliação do SQL Server.

![Integração do Cofre de Chaves do SQL Azure](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-arm-akv.png)

Para obter instruções detalhadas de aprovisionamento, consulte [aprovisionar uma máquina virtual do SQL Server no Portal do Azure](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>VMs existentes
Para máquinas virtuais do SQL Server existentes, selecione a máquina virtual do SQL Server. Em seguida, selecione o **configuração do SQL Server** secção o **definições** painel.

![Integração AKV SQL para VMs existentes](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-existing-vms.png)

No **configuração do SQL Server** painel, clique em de **editar** botão na secção de integração do Cofre de chaves automatizada.

![Configurar a integração AKV SQL para VMs existentes](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-configuration.png)

Quando terminar, clique em de **OK** botão na parte inferior do **configuração do SQL Server** painel para guardar as alterações.

> [!NOTE]
> O nome da credencial que criámos aqui será mapeado para um início de sessão do SQL Server mais tarde. Isto permite que o início de sessão do SQL Server para o Cofre de chaves de acesso. 
>
>

> [!NOTE]
> Também pode configurar a integração AKV através de um modelo. Para obter mais informações, consulte [modelo de início rápido do Azure para a integração do Cofre de chaves do Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update).
> 
> 

[!INCLUDE [AKV Integration Next Steps](../../../../includes/virtual-machines-sql-server-akv-next-steps.md)]

