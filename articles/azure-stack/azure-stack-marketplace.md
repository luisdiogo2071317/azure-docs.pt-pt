---
title: Publicar um item do mercado personalizado no Azure Stack (operador da cloud) | Documentos da Microsoft
description: Como um operador do Azure Stack, saiba como publicar um item do mercado personalizado no Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 60871cbb-eed2-433c-a76d-d605c7aec06c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 01/09/2019
ms.openlocfilehash: 6c1750e6a523828400b3d06d4e1c22fb34f8273f
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55247872"
---
# <a name="azure-stack-marketplace-overview"></a>Descrição geral do Azure Marketplace de pilha

*Aplica-se a: Integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

A pilha do Azure Marketplace é uma coleção de serviços, aplicações e recursos personalizados para o Azure Stack. Os recursos incluem redes, máquinas virtuais, armazenamento e muito mais. Utilize o Marketplace para criar novos recursos e implementar aplicações de novas; ou procure e selecione os itens que pretende utilizar. Para utilizar um item do mercado, os utilizadores têm de subscrever uma oferta que lhe concede acesso ao item.

Como um operador do Azure Stack, decidir quais os itens para adicionar (publicar) no Marketplace. Pode publicar itens, tais como bases de dados, serviços de aplicações e assim por diante. Publicação torna visível para todos os seus utilizadores. Pode publicar itens personalizados que criou ou publicar itens de crescimento [lista de itens de Azure Marketplace](azure-stack-marketplace-azure-items.md). Quando publica um item no Marketplace, os utilizadores podem vê-lo dentro de cinco minutos.

> [!CAUTION]  
> Todos os artefactos de item de galeria, incluindo imagens e ficheiros JSON, podem ser acedidos sem autenticação após disponibilizá-las no Azure Stack Marketplace. Para obter mais considerações ao publicar itens do marketplace personalizados, consulte [criar e publicar um item do mercado](azure-stack-create-and-publish-marketplace-item.md).

Para abrir o Marketplace, no, selecione portal administrador **+ criar um recurso**.

![Marketplace](media/azure-stack-publish-custom-marketplace-item/image1.png)

## <a name="marketplace-items"></a>Itens do Marketplace

Um item do Marketplace do Azure Stack é uma aplicação, serviço ou recurso que os utilizadores podem transferir e utilizar. Todos os itens do Marketplace de pilha do Azure são visíveis para todos os seus utilizadores, incluindo itens administrativos, como os planos e ofertas. Esses itens não necessitam de uma subscrição para o modo de exibição, mas são não funcional para os utilizadores.

Tem a cada item do Marketplace:

* Um modelo do Azure Resource Manager para o aprovisionamento de recursos.
* Metadados, como cadeias de caracteres, ícones e outros materiais de marketing.
* Informações de formatação para mostrar o item no portal.

Todos os itens publicados no Marketplace usa o formato de pacote de galeria do Azure (.azpkg). Adicione recursos de implantação ou tempo de execução (código, arquivos zip, com o software ou imagens de máquinas virtuais) para o Azure Stack separadamente, não como parte do item do Marketplace.

Com a versão 1803 e posterior, o Azure Stack converte imagens para arquivos esparsos transferirem a partir do Azure ou ao carregar as imagens personalizadas. Este processo demore mais tempo ao adicionar uma imagem, mas economiza espaço e acelera a implantação dessas imagens. Conversão aplica-se apenas a novas imagens. Imagens existentes não foram alteradas.

## <a name="next-steps"></a>Passos Seguintes

* [Transferir itens do Marketplace](azure-stack-download-azure-marketplace-item.md)  
* [Criar e publicar um item do mercado](azure-stack-create-and-publish-marketplace-item.md)
