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
ms.date: 10/25/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.lastreviewed: 10/25/2018
ms.openlocfilehash: 03e429e1a40f9b23a2a3bedef5ef488d81392d9c
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236909"
---
# <a name="use-mysql-databases-on-microsoft-azure-stack"></a>Utilizar bases de dados MySQL no Microsoft Azure Stack

Bases de dados MySQL são comuns em Web sites e suportam várias plataformas de Web site. Por exemplo, pode criar Web site WordPress através da plataforma de aplicações Web como um suplemento de serviço (PaaS).

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
