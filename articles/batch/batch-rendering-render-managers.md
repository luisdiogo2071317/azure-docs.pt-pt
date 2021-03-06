---
title: Suporte para o Gestor - de composição do Azure Batch
description: Utilizar o Azure para processar a utilizar a integração de Gestor de composição do Azure Batch
services: batch
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: conceptual
ms.openlocfilehash: 4eeece4946b4f957d9f864da7c46d77d119863b5
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/17/2018
ms.locfileid: "53539927"
---
# <a name="using-azure-batch-with-render-farm-managers"></a>Utilizar o Azure Batch com os gestores de farm de composição

Se estiver a utilizar um existente no local compor farm, em seguida, é altamente provável que um Gerenciador de composição controla a capacidade de farm de composição e processar tarefas.

O Azure fornece suporte interno ou suplementos para gestores de composição populares. Em seguida, pode adicionar e remover VMs do Azure, incluindo VMs com o licenciamento da aplicação de pagamento para utilização e de VMs de baixa prioridade.

Os gerentes de composição seguintes são suportados:

* [PipelineFX Qube!](https://www.pipelinefx.com/)
* [Composição Royal](http://www.royalrender.de/)
* [Prazo de Thinkbox](https://deadline.thinkboxsoftware.com/)

## <a name="using-azure-with-pipelinefx-qube"></a>Utilizar o Azure com PipelineFX Qube

VMs a serem usadas como Qube trabalhadores estão no agrupamento de scripts e instruções para ativar o Azure Batch [do repositório do GitHub](https://github.com/Azure/azure-qube).

## <a name="using-azure-with-royal-render"></a>Utilizar o Azure com composição Royal

Composição Royal tem total integração do Azure e o Azure Batch incorporada, permitindo-lhe expandir um farm de composição com VMs baseadas no Azure. Para obter um resumo, consulte [os arquivos da ajuda](http://www.royalrender.de/help8/index.html?Cloudrendering.html).

Para obter um exemplo de um cliente de renderizar Royal através da integração do Azure, consulte a [história de cliente da Jellyfish Pictures](https://customers.microsoft.com/story/jellyfishpictures).

## <a name="using-azure-with-thinkbox-deadline"></a>Utilizar o Azure com o prazo de Thinkbox

VMs a serem usadas como subordinados do prazo estão no agrupamento de scripts e instruções para ativar o Azure Batch [do repositório do GitHub](https://github.com/Azure/azure-deadline).

## <a name="next-steps"></a>Passos Seguintes

Experimente a integração do Azure Batch para o seu Gestor de composição, usando o plug-in e as instruções no GitHub, onde aplicável.