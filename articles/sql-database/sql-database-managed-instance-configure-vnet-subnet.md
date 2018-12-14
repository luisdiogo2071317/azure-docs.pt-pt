---
title: O Azure SQL Database Managed Instance configurar VNET/sub-rede existente | Documentos da Microsoft
description: Este tópico descreve como configurar uma rede virtual existente (VNet) e a sub-rede onde pode implementar a instância gerida da base de dados SQL do Azure.
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
ms.openlocfilehash: 53aba5192ddf57598965fcfe0db5f2b18423c7e9
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2018
ms.locfileid: "53346609"
---
# <a name="configure-an-existing-vnet-for-azure-sql-database-managed-instance"></a>Configurar uma VNet já existente para a instância gerida da base de dados SQL do Azure

Instância de gerida de base de dados de SQL do Azure tem de ser implementada dentro do Azure [rede virtual (VNet)](../virtual-network/virtual-networks-overview.md) e a sub-rede dedicada apenas para instâncias geridas. Pode utilizar a VNet existente e a sub-rede, se ele é configurado de acordo com o [requisitos de VNet de instância gerida](sql-database-managed-instance-connectivity-architecture.md#network-requirements). 

Se tiver uma nova sub-rede que ainda não está configurada, não estiver certo a sub-rede está alinhada com o [requisitos](sql-database-managed-instance-connectivity-architecture.md#network-requirements), ou se pretender verificar é a sub-rede ainda em conformidade com o [requisitos de rede](sql-database-managed-instance-connectivity-architecture.md#network-requirements) depois algumas alterações que efetuou, poderá validar e modificar a sua rede usando o script explicado nesta secção. 

  > [!Note]
  > Só pode criar uma instância gerida em redes virtuais do Resource Manager. As VNets do Azure implementadas utilizando o modelo de implementação clássica não é suportado. Certifique-se de que calcula o tamanho de sub-rede, seguindo as diretrizes a [determinar o tamanho da sub-rede para instâncias geridas](#determine-the-size-of-subnet-for-managed-instances) secção, porque a sub-rede não pode ser redimensionada depois de implementar os recursos no interior.

## <a name="validate-and-modify-an-existing-virtual-network"></a>Validar e modificar uma rede virtual existente 

Se quiser criar uma instância gerida dentro de uma sub-rede existente, recomendamos o seguinte script do PowerShell para preparar a sub-rede:
```powershell
$scriptUrlBase = 'https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/manage/azure-sql-db-managed-instance/prepare-subnet'

$parameters = @{
    subscriptionId = '<subscriptionId>'
    resourceGroupName = '<resourceGroupName>'
    virtualNetworkName = '<virtualNetworkName>'
    subnetName = '<subnetName>'
    }

Invoke-Command -ScriptBlock ([Scriptblock]::Create((iwr ($scriptUrlBase+'/prepareSubnet.ps1?t='+ [DateTime]::Now.Ticks)).Content)) -ArgumentList $parameters
```
Preparação de sub-rede é feita em três passos simples:

1. Validar - rede virtual selecionada e a sub-rede são validadas para a instância gerida, requisitos de rede.
2. Confirmar - o utilizador é apresentado um conjunto de alterações que precisam ser feitas para preparar a sub-rede para a implementação de instância gerida e solicitado consentimento.
3. Preparar - rede Virtual e sub-rede estão configurados corretamente.

## <a name="next-steps"></a>Passos Seguintes

- Para uma descrição geral, consulte [o que é uma instância gerida](sql-database-managed-instance.md)
- Para obter um tutorial que mostra como criar uma VNet, criar uma instância gerida e restaurar uma base de dados a partir de uma cópia de segurança da base de dados, consulte [criar um Azure SQL Database Managed Instance](sql-database-managed-instance-get-started.md).
- Para problemas DNS, consulte [configurando um DNS personalizado](sql-database-managed-instance-custom-dns.md).
