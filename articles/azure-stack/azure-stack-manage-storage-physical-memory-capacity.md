---
title: Gerir a capacidade de memória física para a pilha do Azure | Microsoft Docs
description: Monitorizar e gerir o espaço de armazenamento disponível para a pilha do Azure.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 84518E90-75E1-4037-8D4E-497EAC72AAA1
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/08/2018
ms.author: mabrigg
ms.reviewer: Thomas.Roettinger
ms.openlocfilehash: 7dc4cfe580246b0f34073113f790ca3b5057995d
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="manage-physical-memory-capacity-for-azure-stack"></a>Gerir a capacidade de memória física para a pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas*

Para aumentar a capacidade de memória total disponível para a pilha do Azure, pode adicionar memória adicional. Na pilha do Azure o servidor físico é também referido como um *nó de unidade de escala*. Todos os nós de unidade de escala que são membros de uma unidade de escala único tem de ter a mesma quantidade de memória.

> [!note]  
> Antes de continuar, consulte a documentação do fabricante de hardware para ver se o fornecedor suporta uma atualização de memória física. O contrato de suporte de fornecedor de hardware do OEM pode necessitar de efetuar o posicionamento de bastidor de servidor físico e a atualização de firmware do dispositivo.

O diagrama de fluxo seguinte mostra o processo geral para adicionar memória para cada nó de unidade de escala.

![Adicione memória em cada nó de unidade de escala](media\azure-stack-manage-storage-physical-capacity\process-to-add-memory-to-scale-unit.png)

## <a name="add-memory-to-an-existing-node"></a>Adicione memória para um nó existente
Os passos seguintes fornecem uma descrição do processo adicionar memória de alto nível. 

> [!Warning]  
Siga estes passos sem referir-se a sua documentação fornecida pelo OEM.

> [!Warning]  
Deve ser encerrada a unidade de escala completa como uma atualização sem interrupção de memória não é suportada.

1. Parar a pilha do Azure utilizando os passos documentados no [início e paragem do Azure pilha](azure-stack-start-and-stop.md) artigo.
2. Atualize a memória em cada computador físico através de documentação do fabricante de hardware.
3. Iniciar a pilha do Azure utilizando os passos a [início e paragem do Azure pilha](azure-stack-start-and-stop.md) artigo.

## <a name="next-steps"></a>Passos Seguintes

 - Para saber como gerir contas de armazenamento na pilha do Azure para localizar, recuperar e recuperar a capacidade de armazenamento com base nas necessidades de negócio, consulte [gerir contas de armazenamento na pilha de Azure](azure-stack-manage-storage-accounts.md).
 - Para saber o operador da nuvem do Azure pilha monitoriza e gere a capacidade de armazenamento da implementação deles pilha do Azure, consulte [gerir a capacidade de pilha do Azure armazenamento](azure-stack-manage-storage-shares.md). 