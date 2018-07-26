---
title: Base de dados SQL do Azure gerido no DNS do instância personalizado | Documentos da Microsoft
description: Este tópico descreve opções de configuração para um DNS personalizado com uma instância de gerida de base de dados do Azure SQL.
services: sql-database
author: srdan-bozovic-msft
manager: craigg
ms.service: sql-database
ms.custom: managed instance
ms.topic: conceptual
ms.date: 04/10/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: d5bb2f2f4b79c4b03e631fc844a712f76fc69109
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/26/2018
ms.locfileid: "39258172"
---
# <a name="configuring-a-custom-dns-for-azure-sql-database-managed-instance"></a>Configurar um DNS personalizado para a base de dados SQL do Azure a instância gerida

Uma instância de gerida de base de dados no Azure SQL (pré-visualização) deve ser implementada numa do Azure [rede virtual (VNet)](../virtual-network/virtual-networks-overview.md). Existem alguns cenários, servidores vinculados para as outras instâncias SQL no seu ambiente na nuvem ou híbrida, que requerem nomes de anfitrião privada para ser resolvido a partir da instância gerida. Neste caso, terá de configurar um DNS personalizado no Azure. Uma vez que a instância gerida utiliza o DNS do mesmo para seus funcionamentos, a configuração de DNS de rede virtual tem de ser compatível com a instância gerida. 

Para tornar uma configuração DNS personalizada compatível com a instância gerida, terá de concluir os seguintes passos: 
- Configurar DNS personalizado para reencaminhar pedidos ao DNS do Azure 
- Configurar o DNS personalizado como principal e o DNS do Azure como secundário para a VNet 
- Registar o DNS personalizado como principal e o DNS do Azure como secundário

## <a name="configure-custom-dns-to-forward-requests-to-azure-dns"></a>Configurar DNS personalizado para reencaminhar pedidos ao DNS do Azure 

Para configurar o reencaminhamento de DNS no Windows Server 2016, utilize estes passos: 

1. Na **Gestor de servidores**, clique em **ferramentas**e, em seguida, clique em **DNS**. 

   ![DNS](./media/sql-database-managed-instance-custom-dns/dns.png) 

2. Faça duplo clique em **reencaminhadores**.

   ![Reencaminhadores](./media/sql-database-managed-instance-custom-dns/forwarders.png) 

3. Clique em **Editar**. 

   ![Lista de reencaminhadores](./media/sql-database-managed-instance-custom-dns/forwarders-list.png) 

4. Introduza recursiva resoluções endereço IP do Azure, por exemplo, 168.63.129.16.

   ![Endereço de ip de resoluções recursivas](./media/sql-database-managed-instance-custom-dns/recursive-resolvers-ip-address.png) 
 
## <a name="set-up-custom-dns-as-primary-and-azure-dns-as-secondary"></a>Configurar DNS personalizado como principal e o DNS do Azure como secundário 
 
Configuração de DNS numa VNet do Azure requer que introduza os endereços IP, por isso, configure a VM do Azure que aloja o servidor DNS com um endereço IP estático usando os seguintes passos: 

1. No portal do Azure, abra a interface de rede de VM de DNS personalizada.

   ![network-interface](./media/sql-database-managed-instance-custom-dns/network-interface.png) 

2. Na secção de configurações de IP. Selecione a configuração de IP 

   ![configuração de IP](./media/sql-database-managed-instance-custom-dns/ip-configuration.png) 


3. Definir o endereço IP privado como estático. Anote o endereço IP (10.0.1.5N nesta captura de ecrã) 

   ![estático](./media/sql-database-managed-instance-custom-dns/static.png) 


## <a name="register-custom-dns-as-primary-and-azure-dns-as-secondary"></a>Registar o DNS personalizado como principal e o DNS do Azure como secundário 

1. No portal do Azure, encontre opções de DNS personalizados para a sua VNet.

   ![opção de dns personalizado](./media/sql-database-managed-instance-custom-dns/custom-dns-option.png) 

2. Mude para personalizado e introduza o seu endereço IP do servidor DNS personalizado, bem como as recursiva resoluções endereço IP do Azure, por exemplo, 168.63.129.16. 

   ![opção de dns personalizado](./media/sql-database-managed-instance-custom-dns/custom-dns-server-ip-address.png) 

   > [!IMPORTANT]
   > Lista não definir o resolvedor recursivo do Azure no DNS faz com que a instância gerida a entrar em estado com falhas. Recuperar a partir de que o estado pode exigir a criação de nova instância numa VNet com as políticas de rede em conformidade, criar dados de nível de instância e restaurar as bases de dados. Ver [configuração da VNet](sql-database-managed-instance-vnet-configuration.md).

## <a name="next-steps"></a>Passos Seguintes

- Para uma descrição geral, consulte [o que é uma instância gerida](sql-database-managed-instance.md)
- Para obter um tutorial que mostra como criar uma nova instância gerida, veja [criar uma instância gerida](sql-database-managed-instance-create-tutorial-portal.md).
- Para obter informações sobre como configurar uma VNet para uma instância gerida, consulte [configuração de VNet para instâncias geridas](sql-database-managed-instance-vnet-configuration.md)
