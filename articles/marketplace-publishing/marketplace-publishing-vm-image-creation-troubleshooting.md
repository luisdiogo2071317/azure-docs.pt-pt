---
title: Como resolver problemas comuns durante a criação do VHD | Documentos da Microsoft
description: Respostas a perguntas de resolução de problemas comuns e problemas durante a criação do VHD.
services: Azure Marketplace
documentationcenter: ''
author: HannibalSII
manager: ''
editor: ''
ms.assetid: e39563d8-8646-4cb7-b078-8b10ac35b494
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 09/26/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: c4e88a9fbb15dd90d619b159ae1065dfacc1907f
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39713402"
---
# <a name="how-to-troubleshoot-common-issues-encountered-during-vhd-creation"></a>Como resolver problemas comuns encontrados durante a criação do VHD
Este artigo é fornecido para o ajudar a um publicador do Azure Marketplace e/ou o coadministrador que pode ocorrer um problema ou ter perguntas comuns ao publicar ou gerir as suas soluções de máquina virtual.

1. Como posso alterar o nome do anfitrião?
   
    Assim que a VM é criada, os utilizadores não é possível atualizar o nome do anfitrião.
2. Como repor o serviço de ambiente de trabalho remoto ou a palavra-passe de início de sessão?
   
   * [Referência para a Windows VM](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-reset-rdp/)
   * [Referência para a VM do Linux](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)
3. Como gerar novo ssh certificados?
   
   Veja a ligação: [https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-classic-reset-access/)
4. Como configurar um certificado VPN open?
   
   Veja a ligação: [https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/](https://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/)
5. O que é a política de suporte para executar o software de servidor Microsoft no ambiente de máquina virtual do Microsoft Azure (infraestrutura-como-serviço)?
   
   Veja a ligação: [https://support.microsoft.com/kb/2721672](https://support.microsoft.com/kb/2721672)
6. Máquinas virtuais têm qualquer identificador exclusivo?
   
   Azure codifica o ID exclusivo da VM do Azure em cada VM. Consulte os detalhes neste blogue e documentação.
7. Numa VM como posso gerir a extensão de script personalizado na tarefa de arranque?
   
   Veja a ligação: [https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/)
8. Como criar uma VM a partir do portal do Azure com o VHD que é carregado para o armazenamento premium?
   
   Podemos ainda não suporta esta funcionalidade.
9. Uma aplicação de 32 bits é suportada no Azure Marketplace?
   
   Veja o link para obter detalhes sobre a política de suporte: [https://support.microsoft.com/kb/2721672](https://support.microsoft.com/kb/2721672)
10. Sempre que estou a tentar criar uma imagem de meus VHDs, recebo a mensagem de erro ". VHD já está registado no repositório de imagens como o recurso"no PowerShell. Eu não criei nenhuma imagem antes nem foi encontrado qualquer imagem com este nome no Azure. Como posso resolver isto?
    
    Normalmente, isto acontecer se o utilizador aprovisionado uma VM a partir deste VHD e não existe um bloqueio nesse VHD. Verifique que não existe nenhuma VM alocada a partir deste VHD. Se o erro ainda persistirem, em seguida, emita um pedido de suporte utilizando a seguinte hiperligação ou da publicação o portal relativamente a isso (os detalhes são fornecidos na resposta de pergunta 11).