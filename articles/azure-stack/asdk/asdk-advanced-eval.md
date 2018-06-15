---
title: Avançadas tarefas de avaliação de pilha do Azure | Microsoft Docs
description: Este artigo descreve as tarefas de avaliação de Azure pilha avançadas.
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
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: c4bf76aa07ec5025d9e53b5518929199ace27e18
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/17/2018
ms.locfileid: "29975826"
---
# <a name="advanced-azure-stack-development-kit-evaluation-tasks"></a>Tarefas de avaliação do avançados Development Kit pilha do Azure
Depois de ter adquirido familiaridade com as funcionalidades de serviço básico do Kit de desenvolvimento de pilha do Azure (ASDK) e capacidades, pode deepen a compreensão da pilha do Azure ainda mais ao testar os cenários mais avançados. Estas tarefas de avaliação mais avançadas estão totalmente documentadas na documentação do operador de pilha do Azure.

> [!NOTE]
> Embora muitas tarefas de operador são suportadas para ASDK e produção, implementações de pilha do Azure com vários nós, nem todos os cenários de utilização são suportados para implementações de ASDK. Consulte [ASDK e diferenças de pilha do Azure com vários nós](asdk-what-is.md#asdk-and-multi-node-azure-stack-differences) para obter mais informações.

## <a name="delegate-offers-in-azure-stack"></a>Delegar ofertas no Azure Stack
Como operador de pilha do Azure, muitas vezes, pretende colocar outras pessoas responsável pela criação de ofertas e inscrever-se os utilizadores. Por exemplo, se tiver um fornecedor de serviços, poderá revendedores inscrever-se os clientes e geri-los em seu nome. Ou, se a parte de um grupo central de TI de uma empresa, é aconselhável subsidiárias inscrever-se os utilizadores sem a intervenção do utilizador.

[Delegar ofertas na pilha de Azure](.\.\azure-stack-delegated-provider.md) ajuda-o a estas tarefas, tornando a possível aceder e gerir mais utilizadores pode diretamente. 

## <a name="make-sql-databases-available-to-your-azure-stack-users"></a>Disponibilizar bases de dados SQL para os utilizadores de pilha do Azure
Como um operador de pilha do Azure, pode criar ofertas informar os utilizadores (inquilinos) criar bases de dados do SQL Server que podem utilizar com as respetivas aplicações de nuvem nativo, Web sites e cargas de trabalho. Ao fornecer estas bases de dados personalizados a pedido, baseado na nuvem aos seus utilizadores, pode guardá-los tempo e recursos. 

Utilizar a placa de fornecedor de recursos do SQL Server para [disponibilizar bases de dados SQL para os utilizadores do Azure pilha](.\.\azure-stack-tutorial-sql-server.md) como um serviço da pilha do Azure. Depois de instalar o fornecedor de recursos, ligar a um ou mais instâncias do SQL Server.

## <a name="make-web-and-api-apps-available-to-your-azure-stack-users"></a>Certifique-web e API apps disponíveis para os utilizadores de pilha do Azure
Como um operador de pilha do Azure, pode criar ofertas informar os utilizadores (inquilinos) criam aplicações das funções do Azure e web e API. Ao fornecer acesso a estas aplicações a pedido, baseado na nuvem aos seus utilizadores, pode guardá-los tempo e recursos.

Implementar o fornecedor de recursos do serviço de aplicações para [disponibilizar web e API apps aos seus utilizadores de pilha do Azure](.\.\azure-stack-tutorial-app-service.md)

## <a name="next-steps"></a>Passos Seguintes
[Saiba mais sobre a oferta de serviços com os sistemas de pilha do Azure integrada](.\.\azure-stack-offer-services-overview.md)