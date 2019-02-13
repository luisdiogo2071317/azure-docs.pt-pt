---
title: Advanced tarefas de avaliação do Azure Stack | Documentos da Microsoft
description: Este artigo descreve as tarefas de avaliação do Azure Stack avançadas.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2019
ms.author: jeffgilb
ms.reviewer: misainat
ms.lastreviewed: 10/16/2018
ms.openlocfilehash: d00eaf0ab24fbd754ba5ad2c79e9630d69d28eb7
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56200518"
---
# <a name="advanced-azure-stack-development-kit-evaluation-tasks"></a>Tarefas de avaliação avançadas do Kit de desenvolvimento do Azure Stack
Depois de ter de obteve familiaridade com as capacidades e funcionalidades básicas do serviço do Azure Stack Development Kit (ASDK), podem aprofundar sua compreensão do Azure Stack ainda mais ao testar os cenários mais avançados. Estas tarefas de avaliação mais avançadas estão documentadas totalmente na documentação do operador do Azure Stack.

> [!NOTE]
> Enquanto muitas tarefas de operador são suportadas para ASDK e produção, implementações do Azure Stack com vários nós, nem todos os cenários de utilização são suportados para implementações de ASDK. Ver [ASDK e com vários nós do Azure Stack diferenças](asdk-what-is.md#asdk-and-multi-node-azure-stack-differences) para obter mais informações.

## <a name="delegate-offers-in-azure-stack"></a>Delegar ofertas no Azure Stack
Como o operador de pilha do Azure, que, muitas vezes, pretende colocar outras pessoas responsável pela criação de ofertas e inscrever-se os utilizadores. Por exemplo, se for um fornecedor de serviços, poderá revendedores Inscreva-se os clientes e geri-los em seu nome. Ou, se é membro de um grupo de TI central numa empresa, pode desejar subsidiárias para se inscrever utilizadores sem a intervenção do utilizador.

[Delegar ofertas no Azure Stack](../azure-stack-delegated-provider.md) ajuda-o com estas tarefas, tornando possível aceder e gerir os utilizadores de mais do que diretamente.

## <a name="make-sql-databases-available-to-your-azure-stack-users"></a>Disponibilizar bases de dados SQL para os utilizadores do Azure Stack
Como um operador de pilha do Azure, pode criar ofertas que permita que os utilizadores (inquilinos) criar bases de dados do SQL que eles podem usar com as suas cargas de trabalho, Web sites e aplicações nativas da cloud. Ao fornecer esses bancos de dados personalizados, sob demanda, com base na cloud aos seus utilizadores, pode guardá-los tempo e recursos.

Utilizar o adaptador do fornecedor de recursos do SQL Server para [disponibilizar bases de dados SQL para os utilizadores do Azure Stack](../azure-stack-tutorial-sql-server.md) como um serviço do Azure Stack. Depois de instalar o fornecedor de recursos ligá-la para uma ou mais instâncias do SQL Server.

## <a name="make-web-and-api-apps-available-to-your-azure-stack-users"></a>Disponibilizar aplicações de API web e aos seus utilizadores do Azure Stack
Como um operador de pilha do Azure, pode criar ofertas que permita que os utilizadores (inquilinos) criem aplicações de funções do Azure e da web e de API. Ao fornecer acesso a estas aplicações a pedido, com base na cloud aos seus utilizadores, pode guardá-los tempo e recursos.

Implementar o fornecedor de recursos do serviço de aplicações para [disponibilizar aplicações de API web e aos seus utilizadores do Azure Stack](../azure-stack-tutorial-app-service.md)

## <a name="next-steps"></a>Passos Seguintes

[Saiba mais sobre a oferta de serviços com os sistemas integrados do Azure Stack](../azure-stack-offer-services-overview.md)
