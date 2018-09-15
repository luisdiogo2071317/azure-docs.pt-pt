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
ms.date: 09/12/2018
ms.author: sethm
ms.reviewer: jeffgo
ms.openlocfilehash: 12310c088777d65bef211747806f942433857e40
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/15/2018
ms.locfileid: "45632353"
---
# <a name="the-azure-stack-marketplace-overview"></a>Visão geral do Azure Stack Marketplace

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

O Marketplace é uma coleção de serviços, aplicações e recursos personalizados para o Azure Stack. Os recursos incluem redes, máquinas virtuais, armazenamento e assim por diante. Os utilizadores aqui vêm para criar novos recursos e implementar novas aplicações. Pense nele como um catálogo de compra em que os utilizadores podem procurar e seleccione os itens que deseja usar. Para utilizar um item do mercado, os utilizadores têm de subscrever uma oferta que lhe concede acesso ao item.

Como um operador do Azure Stack, decidir quais os itens para adicionar (publicar) no Marketplace. Pode publicar coisas como bancos de dados, serviços de aplicações e assim por diante. Publicação torna visível para todos os seus utilizadores. Pode publicar itens personalizados que criou. Também pode publicar itens de crescimento [lista de itens de Azure Marketplace](azure-stack-marketplace-azure-items.md). Quando publica um item no Marketplace, os utilizadores podem vê-lo dentro de cinco minutos.

> [!Caution]  
> Todos os artefactos de item de galeria conhecidos como imagens e ficheiros json podem ser acedidos sem autenticação após disponibilizá-las no mercado do Azure Stack. Para obter mais considerações ao publicar itens do marketplace personalizados, consulte [criar e publicar um item do mercado](azure-stack-create-and-publish-marketplace-item.md).

Para abrir o Marketplace, na consola do administrador selecione **+ criar um recurso**.

![](media/azure-stack-publish-custom-marketplace-item/image1.png)

## <a name="marketplace-items"></a>Itens do Marketplace
Um item do Marketplace do Azure Stack é uma aplicação, serviço ou recurso que os utilizadores podem transferir e utilizar. Todos os itens do Marketplace de pilha do Azure são visíveis para todos os seus utilizadores, incluindo itens administrativos, como os planos e ofertas. Esses itens não necessitam de uma subscrição para o modo de exibição, mas são não funcional para os utilizadores.

Tem a cada item do Marketplace:

* Um modelo do Azure Resource Manager para o aprovisionamento de recursos
* Metadados, como cadeias de caracteres, ícones e outros materiais de marketing
* Informações de formatação para mostrar o item no portal

Todos os itens publicados no Marketplace usa o formato de pacote de galeria do Azure (.azpkg). Adicione recursos de implantação ou tempo de execução (como o código, arquivos zip, com o software ou imagens de máquinas virtuais) para o Azure Stack separadamente, não como parte do item do Marketplace. 

Com a versão 1803 e posterior, o Azure Stack converte imagens para arquivos esparsos transferirem a partir do Azure ou ao carregar as imagens personalizadas. Este processo demore mais tempo ao adicionar uma imagem, mas economiza espaço e acelera a implantação dessas imagens. Conversão aplica-se apenas a novas imagens.  Imagens existentes não foram alteradas. 

## <a name="next-steps"></a>Passos Seguintes
[Transferir itens do Marketplace](azure-stack-download-azure-marketplace-item.md)  
[Criar e publicar um item do mercado](azure-stack-create-and-publish-marketplace-item.md)

