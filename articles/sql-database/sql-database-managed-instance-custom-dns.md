---
title: "Base de dados SQL do Azure gerida no DNS do instância personalizado | Microsoft Docs"
description: "Este tópico descreve opções de configuração para um DNS personalizado com uma instância de gerido da base de dados do Azure SQL."
services: sql-database
author: srdjan-bozovic
manager: craigg
ms.service: sql-database
ms.custom: managed instance
ms.topic: article
ms.date: 03/07/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: 993ff518a1987581ad7de4797611f104080c2c2f
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="configuring-a-custom-dns-for-azure-sql-database-managed-instance"></a>Configurar um DNS personalizado para a base de dados SQL do Azure gerida instância

Tem de ser implementada uma instância de geridos de base de dados no Azure SQL (pré-visualização) dentro de um Azure [rede virtual (VNet)](../virtual-network/virtual-networks-overview.md). Existem alguns cenários, os servidores ligados para outras instâncias SQL no seu ambiente na nuvem ou híbrida, que requerem nomes de anfitrião privada para ser resolvido a partir de instância geridos. Neste caso, terá de configurar um DNS personalizado no interior do Azure. Uma vez que a instância geridos utiliza o mesmo DNS para os trabalhos internos, a configuração de DNS de rede virtual tem de ser compatível com a instância geridos. 

Para efetuar uma configuração de DNS personalizada compatível com a instância geridos, tem de concluir os seguintes passos: 
- Configurar o DNS personalizado para reencaminhar pedidos ao DNS do Azure 
- Configurar o DNS personalizado como principal e o DNS do Azure como secundária para a VNet 
- Registar o DNS personalizado como principal e o DNS do Azure como secundário

## <a name="configure-custom-dns-to-forward-requests-to-azure-dns"></a>Configurar o DNS personalizado para reencaminhar pedidos ao DNS do Azure 

Para configurar o reencaminhamento de DNS no Windows Server 2016, utilize estes passos: 

1. No **Gestor de servidor**, clique em **ferramentas**e, em seguida, clique em **DNS**. 

   ![DNS](./media/sql-database-managed-instance-custom-dns/dns.png) 

2. Faça duplo clique em **reencaminhadores**.

   ![Reencaminhadores](./media/sql-database-managed-instance-custom-dns/forwarders.png) 

3. Clique em **Editar**. 

   ![Forwarders-list](./media/sql-database-managed-instance-custom-dns/forwarders-list.png) 

4. Introduza recursiva resoluções endereço IP do Azure, tais como 168.63.129.16.

   ![Endereço de ip de resoluções recursiva](./media/sql-database-managed-instance-custom-dns/recursive-resolvers-ip-address.png) 
 
## <a name="set-up-custom-dns-as-primary-and-azure-dns-as-secondary"></a>Configurar DNS personalizado como principal e o DNS do Azure como secundário 
 
Configuração de DNS numa VNet do Azure requer que introduza os endereços IP, por isso, configurar a VM do Azure que aloja o servidor DNS com um endereço IP estático, utilizar os seguintes passos: 

1. No portal do Azure, abra a interface de rede de VM de DNS personalizada.

   ![network-interface](./media/sql-database-managed-instance-custom-dns/network-interface.png) 

2. Na secção de configurações de IP. Seleccione a configuração de IP 

   ![configuração de IP](./media/sql-database-managed-instance-custom-dns/ip-configuration.png) 


3. Definir o endereço IP privado como estático. Anote o endereço IP (10.0.1.5 nesta captura de ecrã) 

   ![estático](./media/sql-database-managed-instance-custom-dns/static.png) 


## <a name="register-custom-dns-as-primary-and-azure-dns-as-secondary"></a>Registar DNS personalizado como principal e o DNS do Azure como secundário 

1. No portal do Azure, encontrar a opção DNS personalizado para a sua VNet.

   ![opção de dns personalizado](./media/sql-database-managed-instance-custom-dns/custom-dns-option.png) 

2. Mude para personalizada e introduza o seu endereço IP de servidor DNS personalizado, bem como recursiva resoluções endereço IP do Azure, tais como 168.63.129.16. 

   ![opção de dns personalizado](./media/sql-database-managed-instance-custom-dns/custom-dns-server-ip-address.png) 

   > [!IMPORTANT]
   > Lista não definir o resolvedor recursivo do Azure no DNS faz com que a instância de gerido para o estado defeituoso. Recuperar a partir de que estado poderá exigir o criar uma nova instância numa VNet com as políticas de rede compatível, criar dados de nível de instância e restaurar as bases de dados. Consulte [configuração de VNet](sql-database-managed-instance-vnet-configuration.md).

## <a name="next-steps"></a>Passos Seguintes

- Para obter uma descrição geral, consulte [o que é uma instância geridos](sql-database-managed-instance.md)
- Para obter um exemplo de criar uma VNet e, em seguida, criar uma instância geridos, consulte [criação de uma instância geridos](sql-database-managed-instance-tutorial-portal.md).
- Para configurar a VNet, consulte [configuração de VNet para as instâncias geridas](sql-database-managed-instance-vnet-configuration.md)
