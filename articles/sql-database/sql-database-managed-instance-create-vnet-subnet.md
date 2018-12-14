---
title: VNet de criação de instância gerida de base de dados SQL do Azure | Documentos da Microsoft
description: Este tópico descreve como criar uma rede virtual (VNet) onde pode implementar uma instância de gerida de base de dados do Azure SQL.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: bonova, carlrab
manager: craigg
ms.date: 09/20/2018
ms.openlocfilehash: ebdfc80d3802ad8eb7da6fb7f152efdaee8d777d
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2018
ms.locfileid: "53346233"
---
# <a name="configure-a-vnet-for-azure-sql-database-managed-instance"></a>Configurar uma VNet para a instância gerida de base de dados SQL do Azure

Este tópico explica como criar uma VNet e sub-rede onde pode implementar instâncias geridas da base de dados SQL do Azure válido.

Instância de gerida de base de dados de SQL do Azure tem de ser implementada dentro do Azure [rede virtual (VNet)](../virtual-network/virtual-networks-overview.md). Esta implementação permite que os seguintes cenários: 
- Proteger o endereço IP privado.
- Ligar-se para uma instância gerida diretamente a partir de uma rede no local 
- Ligar uma instância gerida para o servidor ligado ou de outra-arquivo de dados no local 
- Ligar uma instância gerida nos recursos do Azure  

  > [!Note]
  > Deve [determinar o tamanho da sub-rede da instância gerida](sql-database-managed-instance-determine-size-vnet-subnet.md) antes de implementar a primeira instância porque o sunet não pode ser redimensionado depois de colocar os recursos no interior.
  > Se planeia utilizar uma rede virtual existente, terá de modificar essa configuração de rede para acomodar a sua instância gerida. Para obter mais informações, consulte [modificar a rede virtual existente para a instância gerida](sql-database-managed-instance-configure-vnet-subnet.md). 

## <a name="create-a-new-virtual-network"></a>Crie uma nova rede virtual

A maneira mais fácil para criar e configurar a rede virtual é usar o modelo de implementação Azure Resource Manager.

1. Inicie sessão no Portal do Azure.

2. Uso **implementar no Azure** botão para implementar uma rede virtual na cloud do Azure:

  <a target="_blank" href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-sql-managed-instance-azure-environment%2Fazuredeploy.json" rel="noopener" data-linktype="external"> <img src="http://azuredeploy.net/deploybutton.png" data-linktype="external"> </a>

  Este botão abre um formulário que pode utilizar para configurar o ambiente de rede onde pode implementar a instância gerida.

  > [!Note]
  > Este modelo do Azure Resource Manager irá implementar uma rede virtual com duas sub-redes. Uma sub-rede denominada **ManagedInstances** está reservado para instâncias geridas e configurou previamente tabela de rotas, enquanto a outra sub-rede denominada **predefinido** é utilizada para outros recursos que devem acessar gerida Instância (por exemplo, máquinas virtuais do Azure). Pode remover **predefinido** sub-rede, se não precisa dela.

3. Configure o ambiente de rede. No formulário seguinte, é possível configurar parâmetros do seu ambiente de rede:

![Configurar a rede do azure](./media/sql-database-managed-instance-vnet-configuration/create-mi-network-arm.png)

Pode alterar os nomes de VNet e sub-redes e ajustar os intervalos IP associados aos recursos da sua rede. Assim que pressionar o botão "Adquirir", esse formulário irá criar e configurar o seu ambiente. Se não precisar de duas sub-redes, pode eliminar predefinido. 

## <a name="next-steps"></a>Passos Seguintes

- Para uma descrição geral, consulte [o que é uma instância gerida](sql-database-managed-instance.md).
- Saiba mais sobre [arquitetura de conectividade na instância gerida](sql-database-managed-instance-connectivity-architecture.md).
- Limpar como [modificar a rede virtual existente para a instância gerida](sql-database-managed-instance-configure-vnet-subnet.md)
- Para obter um tutorial que mostra como criar uma VNet, criar uma instância gerida e restaurar uma base de dados a partir de uma cópia de segurança da base de dados, consulte [criar um Azure SQL Database Managed Instance](sql-database-managed-instance-get-started.md).
- Para problemas DNS, consulte [configurar um DNS personalizado](sql-database-managed-instance-custom-dns.md)
