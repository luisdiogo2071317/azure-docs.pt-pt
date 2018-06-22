---
title: Utilizar bases de dados MySQL como PaaS na pilha do Azure | Microsoft Docs
description: Saiba como pode implementar o fornecedor de recursos de MySQL e fornecer bases de dados MySQL como um serviço na pilha do Azure.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 24ba595413cde07c420a94de234d7926e0eb0e7f
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309842"
---
# <a name="use-mysql-databases-on-microsoft-azure-stack"></a>Utilizar bases de dados MySQL na pilha do Microsoft Azure

Pode implementar o fornecedor de recursos de MySQL API para utilizar bases de dados MySQL implementados na pilha do Azure. Para obter mais informações sobre a API do fornecedor de recursos, consulte [Windows Azure Pack MySQL recursos fornecedor referência da API REST](https://msdn.microsoft.com/library/dn528442.aspx).

Bases de dados MySQL são comuns em Web sites e suportam várias plataformas de Web site. Por exemplo, pode criar sites WordPress com a plataforma de aplicações Web como um suplemento de serviço (PaaS).

Depois de implementar o fornecedor de recursos, pode:

* Crie servidores MySQL e bases de dados utilizando modelos de implementação Azure Resource Manager.
* Fornece bases de dados MySQL como um serviço.  

## <a name="mysql-resource-provider-adapter-architecture"></a>Arquitetura do adaptador de fornecedor de recursos de MySQL

O fornecedor de recursos tem os seguintes componentes:

* **MySQL recursos fornecedor adaptador máquina virtual (VM)**, que é uma VM do Windows Server que está a executar os serviços do fornecedor.
* **O fornecedor de recursos**, que processa os pedidos de acessos base de dados e recursos.
* **Servidores que alojam o servidor de MySQL**, que fornecem a capacidade para bases de dados que são denominados servidores de alojamento. Pode criar instâncias do MySQL por si ou fornecer acesso a instâncias de MySQL externas. O [Galeria de início rápido do Azure pilha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/mysql-standalone-server-windows) tem um modelo de exemplo que pode utilizar para:

  * Crie um servidor de MySQL para si.
  * Transferir e implementar um servidor de MySQL no Azure Marketplace.

> [!NOTE]
> Alojamento de servidores que estejam instalados na pilha do Azure integrados sistemas tem de ser criados de uma subscrição de inquilino. Estes não podem ser criados da subscrição de fornecedor predefinido. Deve ser criadas no portal de inquilinos ou de uma sessão do PowerShell com um adequado início de sessão. Todos os servidores de alojamento são VMs sujeito a faturação e tem de ter as licenças. O administrador de serviço pode ser o proprietário da subscrição de inquilino.

### <a name="required-privileges"></a>Privilégios necessários

A conta do sistema tem de ter os seguintes privilégios:

* **Base de dados:** criar, remover
* **Início de sessão:** criar, definir, remover, conceder, revogar  

## <a name="next-steps"></a>Passos Seguintes

[Implementar o fornecedor de recursos de MySQL](azure-stack-mysql-resource-provider-deploy.md)
