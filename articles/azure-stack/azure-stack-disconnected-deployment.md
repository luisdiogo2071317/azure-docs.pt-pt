---
title: Decisões de implementação desligado do Azure para a pilha do Azure integrado sistemas | Microsoft Docs
description: Determine as decisões de implementações de vários nós ligado do Azure de pilha do Azure de planeamento de implementação.
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
ms.date: 04/09/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: 8fbc489bacb3c8592ee99fc5a4486fd864f498ee
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="azure-disconnected-deployment-planning-decisions-for-azure-stack-integrated-systems"></a>Decisões sobre a pilha do Azure de planeamento de implementação desligada do Azure integrado sistemas
Depois de tiver decidido [como irá integrar pilha do Azure para o seu ambiente de nuvem híbrida](azure-stack-connection-models.md), em seguida, pode finalizar as suas decisões de implementação de pilha do Azure.

Com o desligado da opção de implementação do Azure, pode implementar e utilizar o Azure pilha sem uma ligação à Internet. No entanto, uma implementação de desligado, está limitado a um arquivo de identidade do AD FS e o modelo de faturação baseada na capacidade. 

Escolha esta opção se a:
- Tem de segurança ou outras restrições que requerem a implementação do Azure pilha num ambiente que não está ligado à Internet.
- Pretende bloquear os dados (incluindo dados de utilização) de que está a ser enviados para o Azure.
- Pretende utilizar o Azure pilha puramente como uma solução de nuvem privada que é implementada a sua Intranet empresarial e não é interessado em cenários híbridos.

> [!TIP]
> Por vezes, este tipo de ambiente também é referido como "cenário submarine".

Uma implementação desligada não estritamente significa que não é possível ligar mais tarde a sua instância de pilha do Azure para o Azure para cenários VM do inquilino híbridos. Significa que não tem conectividade para o Azure durante a implementação ou não pretender utilizar o Azure Active Directory como o arquivo de identidade.

## <a name="features-that-are-impaired-or-unavailable-in-disconnected-deployments"></a>Funcionalidades que são debilitadas ou não está disponíveis em implementações desligadas 
Pilha do Azure foi concebida para funcionar melhor quando ligado ao Azure, pelo que é importante ter em atenção que existem algumas funcionalidades de que estão completamente indisponível no modo desligado ou afetada. 

|Funcionalidade|Impacto no modo desligado|
|-----|-----|
|Implementação da VM com a extensão de DSC para configurar a implementação da VM|Danificada - extensão de DSC na procura à Internet para o WMF mais recente.|
|Implementação da VM com a extensão de Docker para executar comandos de Docker|Danificada – Docker irá verificar a Internet para a versão mais recente e esta verificação irá falhar.|
|Ligações de documentação no Portal de pilha do Azure|Indisponível – ligações, tais como enviar comentários, ajuda, início rápido, etc., que utilizam um URL de Internet não funcionará.|
|Alerta remediação/mitigação que referencia um guia de remediação online|Indisponível – remediações de alerta ligações que utilizam que um URL de Internet não funcionará.|
|Sindicação Marketplace – a capacidade para seleccionar e adicionar pacotes de galeria diretamente a partir do Azure Marketplace|Danificada – quando implementar o Azure pilha num modo desligado (sem qualquer acesso à internet), não é possível transferir itens do marketplace utilizando o portal de pilha do Azure. No entanto, pode utilizar o [ferramenta de sindicação do marketplace](https://docs.microsoft.com/azure/azure-stack/azure-stack-download-azure-marketplace-item#download-marketplace-items-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity) para transferir os itens do marketplace para uma máquina que tenha acesso à internet e, em seguida, transfere-os para o seu ambiente de pilha do Azure.|
|Utilizar contas de Federação do Active Directory do Azure para gerir uma implementação de pilha do Azure|Indisponível – esta funcionalidade necessita de conectividade para o Azure. Tem de ser utilizado em vez disso, o AD FS com uma instância local do Active Directory.|
|Fornecedores de recursos, tais como WebApps e SQL Server|Não disponível - fornecedores de recursos, tais como WebApps e SQL requerem acesso à Internet para o conteúdo.|
|Interface de Linha de Comandos (CLI)|Danificada – CLI foi reduzido funcionalidade em termos de aprovisionamento dos princípios de serviço e autenticação.|
|Visual Studio – da Cloud discovery|Danificada – da Cloud Discovery ou irá detetar nuvens diferentes ou não irão funcionar.|
|Visual Studio – do AD FS|Danificada – só suporta o Visual Studio Enterprise AD FS.
Telemetria|Indisponível – os dados telemétricos para a pilha do Azure como bem como quaisquer pacotes de Galeria de terceiros que dependem de dados de telemetria.|
|Certificados|Indisponível – ligação à Internet é necessário para os serviços de lista de revogação de certificados (CRL) e protocolo de estado de certificado Online (OSCP) no contexto de HTTPS.|
|Cofre de chaves|Danificada – um caso de utilização comuns para o Cofre de chaves é ter uma aplicação de ler os segredos no tempo de execução. Para tal, a aplicação tem um principal de serviço no diretório. No Azure Active Directory, os utilizadores regulares (não administradores) são por predefinição permitida para adicionar principais de serviço. No AD (utilizando o AD FS) não são. Isto coloca uma hurdle a experiência de ponto a ponto, porque um sempre tiver de passar por um administrador de diretório para adicionar qualquer aplicação.| 

## <a name="learn-more"></a>Obter mais informações
- Para obter informações sobre casos de utilização, aquisição, parceiros e fornecedores de hardware do OEM, consulte o [Azure pilha](https://azure.microsoft.com/overview/azure-stack/) página de produto.
- Para obter informações sobre o plano e a georreplicação disponibilidade pilha do Azure integrados sistemas, consulte o documento técnico: [pilha do Azure: uma extensão do Azure](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/). 
- Para saber mais sobre o Microsoft Azure pilha empacotamento e preços [transferir. o PDF](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf). 

## <a name="next-steps"></a>Passos Seguintes
[Integração de rede do Centro de dados](azure-stack-network.md)