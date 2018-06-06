---
title: Criar e gerir a base de dados do Azure para pontos finais de serviço PostgreSQL VNet e regras utilizando o portal do Azure | Microsoft Docs
description: Criar e gerir a base de dados do Azure para pontos finais de serviço PostgreSQL VNet e regras utilizando o portal do Azure
services: postgresql
author: mbolz
ms.author: mbolz
manager: kfile
editor: jasonwhowell
ms.service: postgresql-database
ms.topic: article
ms.date: 06/01/2018
ms.openlocfilehash: 8199bd98b9ae091ff2b27efa8334e8e763879359
ms.sourcegitcommit: 4f9fa86166b50e86cf089f31d85e16155b60559f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34757370"
---
# <a name="create-and-manage-azure-database-for-postgresql-vnet-service-endpoints-and-vnet-rules-by-using-the-azure-portal"></a>Criar e gerir a base de dados do Azure para pontos finais de serviço PostgreSQL VNet e regras de VNet através do portal do Azure
Pontos finais de serviços de rede (VNet) virtuais e as regras de expandem o espaço de endereço privado de uma rede Virtual para a base de dados do Azure para o servidor de PostgreSQL. Para obter uma descrição geral da base de dados do Azure para pontos finais de serviço PostgreSQL VNet, incluindo as limitações, consulte [base de dados do Azure para pontos finais de serviço PostgreSQL servidor VNet](concepts-data-access-and-security-vnet.md). Pontos finais do serviço de VNet estão disponíveis na pré-visualização pública em todas as regiões suportadas para a base de dados do Azure para PostgreSQL.

> [!NOTE]
> Suporte para pontos finais do serviço de VNet é apenas para fins gerais e otimização de memória de servidores.

## <a name="create-a-vnet-rule-and-enable-service-endpoints-in-the-azure-portal"></a>Criar uma regra de VNet e ativar pontos finais de serviço no portal do Azure

1. Na página de servidor PostgreSQL, sob as definições, clique em **ligação segurança** para abrir o painel de segurança de ligação da base de dados do Azure para PostgreSQL. Em seguida, clique em **+ adicionar rede virtual existente**. Se não tiver uma VNet existente, pode clicar em **+ criar nova rede virtual** para criar um. Consulte [início rápido: criar uma rede virtual com o portal do Azure](../virtual-network/quick-create-portal.md)

   ![Portal do Azure – clique em segurança de ligação](./media/howto-manage-vnet-using-portal/1-connection-security.png)

2. Introduza um nome de regra de VNet, selecione a subscrição, a rede Virtual e o nome da sub-rede e, em seguida, clique em **ativar**. Isto permite automaticamente pontos finais do serviço de VNet a sub-rede utilizando o **Microsoft.SQL** tag de serviço.

   ![Portal do Azure – configurar VNet](./media/howto-manage-vnet-using-portal/2-configure-vnet.png)

   > [!IMPORTANT]
   > É altamente recomendado ler este artigo sobre considerações e configurações do ponto final de serviço antes de configurar pontos finais de serviço. **Ponto final de serviço de rede virtual:** A [ponto final do serviço de rede Virtual](../virtual-network/virtual-network-service-endpoints-overview.md) uma sub-rede cujos valores de propriedade incluem um ou mais nomes de tipo de serviço do Azure formal. Pontos finais de serviços de VNet utilizam o nome do tipo de serviço **Microsoft.Sql**, que se refere ao serviço do Azure com o nome da base de dados do SQL Server. Esta etiqueta de serviço também se aplica a SQL Database do Azure, base de dados do Azure para serviços PostgreSQL e MySQL. É importante ter em conta ao aplicar o **Microsoft.Sql** etiquetas de serviço para um ponto final de serviço de VNet configura o tráfego de ponto final de serviço para todos os serviços de base de dados do Azure, incluindo o SQL Database do Azure, base de dados do Azure para PostgreSQL e Base de dados do Azure para servidores MySQL na sub-rede. 
   > 

3. Uma vez ativada, clique em **OK** e verá que a VNet pontos finais de serviço estão ativados, juntamente com uma regra de VNet.

   ![Pontos finais de serviço de VNet ativados e a regra de VNet criada](./media/howto-manage-vnet-using-portal/3-vnet-service-endpoints-enabled-vnet-rule-created.png)

## <a name="next-steps"></a>Passos Seguintes
- Da mesma forma, pode aplicar o script para [VNet ativar pontos finais de serviço e criar uma regra VNET para base de dados do Azure para PostgreSQL utilizando a CLI do Azure](howto-manage-vnet-using-cli.md).
- Para obter ajuda na ligação para uma base de dados do Azure para o servidor de PostgreSQL, consulte [bibliotecas de ligação para base de dados do Azure para PostgreSQL](./concepts-connection-libraries.md)
