---
title: Utilizar bases de dados MySQL como PaaS no Azure Stack | Documentos da Microsoft
description: Saiba como pode implementar o fornecedor de recursos do MySQL e oferecem bases de dados MySQL como serviço no Azure Stack.
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
ms.date: 02/12/2019
ms.author: jeffgilb
ms.reviewer: quying
ms.lastreviewed: 10/25/2018
ms.openlocfilehash: e610f946868940f9fde66932bedec1dc998f390b
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56203884"
---
# <a name="use-mysql-databases-on-microsoft-azure-stack"></a>Utilizar bases de dados MySQL no Microsoft Azure Stack

Bases de dados MySQL são comumente usadas com os Web sites e suportam várias plataformas de Web site. Por exemplo, pode criar Web site WordPress com o suplemento de fornecedor (PaaS) de recursos de serviços de aplicações.

Depois de implementar o fornecedor de recursos, pode:

* Crie servidores MySQL e bases de dados com modelos de implementação Azure Resource Manager.
* Oferecem bases de dados MySQL como serviço.  

## <a name="mysql-resource-provider-adapter-architecture"></a>Arquitetura de adaptador do fornecedor de recursos do MySQL

O fornecedor de recursos tem os seguintes componentes:

* **Máquina de virtual o adaptador de fornecedor de recursos MySQL (VM)**, que é uma VM do Windows Server que está a executar os serviços do fornecedor.
* **O fornecedor de recursos**, que processa os pedidos e recursos da base de dados de acessos.
* **Servidores que alojam o servidor MySQL**, que proporcionam a capacidade para bases de dados que são chamados de servidores de alojamento. Pode criar instâncias de MySQL por conta própria ou fornecer acesso a instâncias externas do MySQL. O [Galeria de início rápido do Azure Stack](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/mysql-standalone-server-windows) tem um modelo de exemplo que pode utilizar para:

  * Crie um servidor MySQL para.
  * Transferir e implementar um servidor MySQL no Azure Marketplace.

> [!NOTE]
> Alojar servidores que estão instalados no Azure Stack sistemas integrados tem de ser criados de uma subscrição de inquilino. Eles não não possível criar a partir da subscrição do fornecedor predefinido. Eles devem ser criados no portal inquilino ou a partir de uma sessão do PowerShell com um início de sessão-in adequado. Todos os servidores de alojamento são VMs cobrar e tem de ter licenças. O administrador de serviços pode ser o proprietário da subscrição do inquilino.

### <a name="required-privileges"></a>Privilégios necessários

A conta do sistema tem de ter os seguintes privilégios:

* **Base de dados:** criar, remover
* **Início de sessão:** criar, configurar, remover, conceder, revogar  

## <a name="next-steps"></a>Passos Seguintes

[Implementar o fornecedor de recursos do MySQL](azure-stack-mysql-resource-provider-deploy.md)
