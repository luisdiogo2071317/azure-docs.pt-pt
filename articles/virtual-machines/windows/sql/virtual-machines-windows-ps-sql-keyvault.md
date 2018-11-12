---
title: Integrar o Cofre de chaves com o SQL Server em VMs do Windows no Azure (Resource Manager) | Documentos da Microsoft
description: Saiba como automatizar a configuração de criptografia do SQL Server para utilização com o Azure Key Vault. Este tópico explica como utilizar a integração do Azure Key Vault com as máquinas de virtuais do SQL Server criadas com o Resource Manager.
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
ms.openlocfilehash: d80526768c59bbb746408a026915b3228747e18a
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51251175"
---
# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-virtual-machines-resource-manager"></a>Configurar a integração do Cofre de chaves do Azure para o SQL Server em máquinas virtuais do Azure (Resource Manager)

> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-ps-sql-keyvault.md)
> * [Clássico](../sqlclassic/virtual-machines-windows-classic-ps-sql-keyvault.md)

## <a name="overview"></a>Descrição geral
Existem vários recursos de criptografia do SQL Server, tais como [encriptação de dados transparente (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [encriptação de nível de coluna (CLE)](https://msdn.microsoft.com/library/ms173744.aspx), e [encriptação de cópia de segurança](https://msdn.microsoft.com/library/dn449489.aspx). Esses formulários de criptografia requerem a gerir e armazenar as chaves criptográficas que utilizar para a encriptação. O serviço do Azure Key Vault (AKV) foi concebido para melhorar a segurança e gestão destas chaves numa localização segura e de elevada disponibilidade. O [conector do SQL Server](https://www.microsoft.com/download/details.aspx?id=45344) permite que o SQL Server para utilizar estas chaves do Azure Key Vault.

Se estiver a executar o SQL Server com máquinas no local, existem [etapas que pode seguir para acessar Azure Key Vault a partir do seu computador do SQL Server no local](https://msdn.microsoft.com/library/dn198405.aspx). Mas para o SQL Server em VMs do Azure, pode poupar tempo ao utilizar o *a integração do Azure Key Vault* funcionalidade.

Quando esta funcionalidade está ativada, ele automaticamente instala o conector do SQL Server, configura o fornecedor EKM para aceder ao Azure Key Vault e cria a credencial para que possa aceder ao Cofre de. Se verificasse os passos na documentação do locais mencionado anteriormente, pode ver que esse recurso automatiza os passos 2 e 3. A única coisa que ainda precisava manualmente a fazer é criar o Cofre de chaves e as chaves. A partir daí, a configuração completa da sua VM de SQL é automatizada. Depois desta funcionalidade concluída esta configuração, pode executar instruções T-SQL para começar a encriptar as bases de dados ou cópias de segurança, tal como faria normalmente.

[!INCLUDE [AKV Integration Prepare](../../../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="enabling-and-configuring-akv-integration"></a>Ativar e configurar a integração AKV
Pode ativar a integração AKV durante o aprovisionamento ou configurá-lo para as VMs existentes.

### <a name="new-vms"></a>Novas VMs
Se estiver a aprovisionar uma nova máquina de virtual do SQL Server com o Resource Manager, o portal do Azure fornece uma forma de ativar a integração do Azure Key Vault. A funcionalidade do Azure Key Vault está disponível apenas para o Enterprise, Developer e edições de avaliação do SQL Server.

![Integração do Cofre de Chaves do SQL Azure](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-arm-akv.png)

Para obter instruções detalhadas de aprovisionamento, consulte [aprovisionar uma máquina virtual do SQL Server no Portal do Azure](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>VMs existentes
Para máquinas de virtuais do SQL Server existentes, selecione a máquina virtual do SQL Server. Em seguida, selecione o **configuração do SQL Server** secção a **definições** painel.

![Integração AKV SQL de VMs existentes](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-existing-vms.png)

Na **configuração do SQL Server** painel, clique nas **editar** botão na seção de integração do Cofre de chaves automatizada.

![Configurar a integração AKV SQL de VMs existentes](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-configuration.png)

Quando terminar, clique nas **OK** botão na parte inferior dos **configuração do SQL Server** painel para guardar as alterações.

> [!NOTE]
> O nome da credencial que criámos aqui será mapeado para um início de sessão SQL mais tarde. Isso permite que o início de sessão SQL aceder ao Cofre de chave. 
>
>

> [!NOTE]
> Também pode configurar a integração AKV através de um modelo. Para obter mais informações, consulte [modelo de início rápido do Azure para integração do Azure Key Vault](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update).
> 
> 

[!INCLUDE [AKV Integration Next Steps](../../../../includes/virtual-machines-sql-server-akv-next-steps.md)]

