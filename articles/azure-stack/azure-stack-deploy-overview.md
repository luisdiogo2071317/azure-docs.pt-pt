---
title: Avaliar o Kit de desenvolvimento de pilha do Azure | Microsoft Docs
description: "Saiba como implementar o Kit de desenvolvimento de pilha do Azure para fins de avaliação."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 01/22/2018
ms.author: jeffgilb
ms.custom: mvc
ms.openlocfilehash: 4ad2e0a91e2fd5023417722fc0a1a6fae93960d0
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
---
# <a name="quickstart-evaluate-the-azure-stack-development-kit"></a>Início rápido: Avaliar o Kit de desenvolvimento de pilha do Azure

*Aplica-se a: Azure da pilha Kit de desenvolvimento*

O [Kit de desenvolvimento de pilha do Azure](azure-stack-poc.md) é um ambiente de teste e desenvolvimento que pode implementar para avaliar e demonstrar serviços e funcionalidades de pilha do Azure. Para obtê-lo em execução, terá de preparar o hardware de ambiente e executar alguns scripts (esta ação irá demorar várias horas). Depois disso, pode iniciar sessão portais do administrador e utilizador para gerir a pilha do Azure e ofertas de teste. 

1. [**Planear o hardware, software e rede**](azure-stack-deploy.md). O computador que aloja o kit de desenvolvimento (o anfitrião do kit de desenvolvimento) têm de cumprir requisitos de hardware, software e rede. Também tem de escolher entre a utilização do Azure Active Directory ou serviços de Federação do Active Directory. Lembre-se de que estão em conformidade com os pré-requisitos antes de iniciar a implementação para que o processo de instalação seja executado facilmente. 

2. [**Transferir e extraia o pacote de implementação**](azure-stack-run-powershell-script.md#download-and-extract-the-development-kit). Pode transferir o pacote de implementação para o anfitrião do kit de desenvolvimento ou para um outro computador. Os ficheiros extraídos implementação demorar até 60 GB de espaço livre em disco, pelo que utilizar outro computador pode ajudar a reduzir os requisitos de hardware para o anfitrião do kit de desenvolvimento.

3. [**Preparar o anfitrião do kit de desenvolvimento** ](azure-stack-run-powershell-script.md) utilizando o instalador. Após este passo, irá arrancar o anfitrião do kit de desenvolvimento para Cloudbuilder.vhdx (ficheiros de instalação de uma unidade de disco rígida virtual que inclui um sistema operativo de arranque e a pilha do Azure).

4. [**Implementar o kit de desenvolvimento** ](azure-stack-run-powershell-script.md) no anfitrião do kit de desenvolvimento.

5. Se a implementação de pilha do Azure utiliza o Azure Active Directory, tem de [registar pilha do Azure com o Azure](azure-stack-register.md) , de modo a que possa [transferir itens do Azure marketplace](azure-stack-download-azure-marketplace-item.md) à pilha do Azure.

Depois de concluir estes passos, terá um ambiente do kit de desenvolvimento com os portais de administrador e utilizador. Em seguida, pode [ligar e iniciar sessão](azure-stack-connect-azure-stack.md) no portal. Em seguida, pode começar a implementar fornecedores de recursos, criar [oferece](azure-stack-key-features.md#regions-services-plans-offers-and-subscriptions)e para preencher a pilha de Azure [marketplace](azure-stack-marketplace.md).
