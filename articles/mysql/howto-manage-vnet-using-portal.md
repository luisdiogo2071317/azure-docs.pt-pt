---
title: Criar e gerir a base de dados para pontos finais de serviço de MySQL VNet e regras no portal do Azure | Documentos da Microsoft
description: Criar e gerir a base de dados para pontos finais de serviço de MySQL VNet e regras no portal do Azure
services: mysql
author: mbolz
ms.author: mbolz
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 10/22/2018
ms.openlocfilehash: 73655cf55a93a758ecb29b3408cdd0d803060397
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/24/2018
ms.locfileid: "49956647"
---
# <a name="create-and-manage-azure-database-for-mysql-vnet-service-endpoints-and-vnet-rules-by-using-the-azure-portal"></a>Criar e gerir a base de dados para pontos finais de serviço de MySQL VNet e regras de VNet com o portal do Azure
Pontos finais de serviços de rede (VNet) virtual e regras de estendem o espaço de endereços privados de uma rede Virtual à sua base de dados do Azure para o servidor MySQL. Para uma visão geral da base de dados do Azure para MySQL VNet pontos finais de serviço, incluindo as limitações, consulte [base de dados do Azure para pontos finais de serviço do MySQL Server VNet](concepts-data-access-and-security-vnet.md). Pontos finais de serviço de VNet estão disponíveis em todas as regiões suportadas para a base de dados do Azure para MySQL.

> [!NOTE]
> Suporte para pontos finais de serviço da VNet é apenas para fins gerais e memória otimizada de servidores.

## <a name="create-a-vnet-rule-and-enable-service-endpoints-in-the-azure-portal"></a>Criar uma regra de VNet e ativar os pontos finais de serviço no portal do Azure

1. Na página do servidor MySQL, sob as configurações, clique em **segurança de ligação** para abrir o painel de segurança da ligação da base de dados do Azure para MySQL. Em seguida, clique em **+ adicionar rede virtual existente**. Se não tiver uma VNet existente pode clicar **+ criar nova rede virtual** para criar um. Consulte [início rápido: criar uma rede virtual com o portal do Azure](../virtual-network/quick-create-portal.md)

   ![Portal do Azure - clique em segurança de ligação](./media/howto-manage-vnet-using-portal/1-connection-security.png)

2. Introduza um nome de regra de VNet, selecione a subscrição, a rede Virtual e o nome da sub-rede e, em seguida, clique em **ativar**. Isto permite automaticamente pontos finais de serviço de VNet no sub-rede com o **Microsoft. SQL** etiqueta de serviço.

   ![Portal do Azure - configurar VNet](./media/howto-manage-vnet-using-portal/2-configure-vnet.png)

   A conta tem de ter as permissões necessárias para criar uma rede virtual e o ponto final de serviço.

   Pontos finais de serviço podem ser configurados em redes virtuais de forma independente por um utilizador com acesso de escrita para a rede virtual.
    
   Para proteger os recursos de serviço do Azure a uma VNet, o utilizador tem de ter permissão para "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/" para as sub-redes que está a ser adicionadas. Esta permissão está incluída por predefinição nas funções incorporadas de administrador de serviço e podem ser modificadas mediante a criação de funções personalizadas.
    
   Saiba mais sobre [funções incorporadas](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) e a atribuição de permissões específicas a [funções personalizadas](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles).
    
   As VNets e os recursos de serviço do Azure podem pertencer às mesmas subscrições ou a subscrições diferentes. Se os recursos de serviço de VNet e o Azure estão em subscrições diferentes, os recursos devem existir no mesmo inquilino do Active Directory (AD).

   > [!IMPORTANT]
   > É altamente recomendado para ler este artigo sobre considerações e configurações de ponto final de serviço antes de configurar pontos finais de serviço. **Ponto final de serviço de rede virtual:** uma [ponto final de serviço de rede Virtual](../virtual-network/virtual-network-service-endpoints-overview.md) é uma sub-rede cujos valores de propriedade incluem um ou mais nomes de tipo de serviço do Azure formal. Pontos de extremidade de serviços de VNet utilizam o nome do tipo de serviço **Microsoft. SQL**, que faz referência ao serviço do Azure com o nome da base de dados SQL. Esta etiqueta de serviço também se aplica a SQL Database do Azure, base de dados do Azure para PostgreSQL e MySQL serviços. É importante ter em conta ao aplicar a **Microsoft. SQL** etiqueta de serviço para um ponto de extremidade do serviço de VNet configura o tráfego de ponto final de serviço para todos os serviços de base de dados do Azure, incluindo o SQL Database do Azure, base de dados do Azure para PostgreSQL e Base de dados do Azure para MySQL servidores na sub-rede. 
   > 

3. Uma vez ativada, clique em **OK** e verá que os pontos finais de serviço de VNet estão ativados, juntamente com uma regra de VNet.

   ![Pontos finais de serviço de VNet ativados e a regra de VNet criada](./media/howto-manage-vnet-using-portal/3-vnet-service-endpoints-enabled-vnet-rule-created.png)

## <a name="next-steps"></a>Passos Seguintes
- Da mesma forma, pode criar scripts para [VNet ativar pontos finais de serviço e criar uma regra VNET para a base de dados do Azure para MySQL através da CLI do Azure](howto-manage-vnet-using-cli.md).
- Para obter ajuda na conexão com uma base de dados do Azure para o servidor MySQL, consulte [bibliotecas de ligação para base de dados do Azure para MySQL](./concepts-connection-libraries.md)
