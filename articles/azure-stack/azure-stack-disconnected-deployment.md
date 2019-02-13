---
title: Sistemas integrados de decisões de implementação desconectado do Azure para o Azure Stack | Documentos da Microsoft
description: Determine decisões para implementações de ligada ao Azure Stack Azure com vários nós de planejamento da implantação.
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
ms.reviewer: wfayed
ms.lastreviewed: 12/11/2018
ms.openlocfilehash: 4481bcf7a794423f98f45e4a21a139dbe4c32b4f
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56210786"
---
# <a name="azure-disconnected-deployment-planning-decisions-for-azure-stack-integrated-systems"></a>Sistemas integrados do Azure decisões para o Azure Stack de planejamento da implantação desligada
Depois de decidir [como irá integrar o Azure Stack para seu ambiente de cloud híbrida](azure-stack-connection-models.md), poderão finalizar suas decisões de implementação do Azure Stack.

Pode implementar e utilizar o Azure Stack sem uma ligação à internet. No entanto, com uma implementação de desligado, está limitado a um repositório de identidades do AD FS e o modelo de faturação com base na capacidade. Como a arquitetura "multitenancy" requer a utilização do Azure Active Directory (Azure AD), arquitetura "multitenancy" não é suportada para implementações desligadas. 

Escolha esta opção se:
- Ter segurança ou outras restrições que requerem a implementação do Azure Stack num ambiente que não está ligado à Internet.
- Deseja bloquear dados (incluindo dados de utilização) de ser enviado para o Azure.
- Pretende utilizar o Azure Stack puramente como uma solução de nuvem privada que é implementada a sua Intranet empresarial e não está interessado em cenários híbridos.

> [!TIP]
> Às vezes, esse tipo de ambiente é também referido como um *cenário submarino*.

Uma implementação desligada não o restringe de mais tarde se ligar a instância do Azure Stack para o Azure para cenários VM do inquilino híbrido. Isso significa que não tem ligação para o Azure durante a implementação ou não quiser utilizar o Azure AD como o armazenamento de identidade.

## <a name="features-that-are-impaired-or-unavailable-in-disconnected-deployments"></a>Funcionalidades que são reduzidas ou não está disponíveis em implementações desligadas 
O Azure Stack foi concebido para funcionam melhor quando ligado ao Azure, pelo que é importante observar que existem alguns recursos e funcionalidades que são totalmente indisponível no modo desligado ou reduzida. 

|Funcionalidade|Impacto no modo desligado|
|-----|-----|
|Implementação da VM com a extensão de DSC para configurar a implementação da VM|Com deficiências visuais - extensão DSC procura para a Internet para o WMF mais recente.|
|Implementação da VM com a extensão Docker para executar comandos do Docker|Com deficiências visuais – Docker irá verificar a Internet para a versão mais recente e esta verificação irá falhar.|
|Ligações de documentação no Portal do Azure Stack|Indisponível – ligações, como enviar comentários, ajuda, guia de introdução, etc. que utilizam um URL de Internet não funcionará.|
|Alerta remediação/atenuação que faz referência a um guia de atualização online|Indisponível – qualquer remediação do alerta contém ligações que utilizam que um URL de Internet não funcionará.|
|Marketplace, a capacidade para seleccionar e adicionar pacotes de galeria diretamente a partir do Azure Marketplace|Com deficiências visuais – quando implementar o Azure Stack num modo desligado (sem qualquer conectividade de Internet), não é possível transferir itens do marketplace com o portal do Azure Stack. No entanto, pode utilizar o [ferramenta de distribuição de mercado](https://docs.microsoft.com/azure/azure-stack/azure-stack-download-azure-marketplace-item) transferir os itens do marketplace para uma máquina que tenha conectividade à internet e, em seguida, transferi-las ao seu ambiente do Azure Stack.|
|Utilizar contas de Federação do Azure Active Directory para gerir uma implementação do Azure Stack|Indisponível – esta funcionalidade necessita de conectividade para o Azure. Tem de ser utilizado em vez disso, o AD FS com uma instância local do Active Directory.|
|Serviços Aplicacionais|Com deficiências visuais - WebApps podem exigir acesso à Internet para o conteúdo atualizado.|
|Interface de Linha de Comandos (CLI)|Com deficiências visuais – CLI tem funcionalidades reduzidas em termos de autenticação e o aprovisionamento dos principais de serviço.|
|Visual Studio – da Cloud discovery|Da Cloud Discovery com deficiências visuais – ou irá detetar nuvens diferentes ou não irá funcionar.|
|Visual Studio – do AD FS|Com deficiências visuais – somente o Visual Studio Enterprise suporta o AD FS.
Telemetria|Indisponível – dados de telemetria para o Azure Stack como bem como quaisquer pacotes de Galeria de terceiros que dependem de dados de telemetria.|
|Certificados|Indisponível – ligação à Internet é necessária para os serviços de lista de revogação de certificados (CRL) e protocolo de Status de certificado Online (OSCP) no contexto de HTTPS.|
|Cofre de chaves|Um caso de utilização comuns para o Key Vault com deficiências visuais – consiste num aplicativo que leia os segredos em tempo de execução. Para isso, a aplicação tem um principal de serviço no diretório. No Azure Active Directory, os usuários regulares (não-administradores) são por predefinição autorizada a adicionar principais de serviço. No AD (com o AD FS) não são. Isso coloca uma barreira da experiência do ponto-a-ponto, porque um sempre precisa passar por um administrador do diretório para adicionar qualquer aplicação.| 

## <a name="learn-more"></a>Saiba mais
- Para obter informações sobre casos de utilização, de compra, parceiros e fornecedores de hardware de OEM, consulte a [do Azure Stack](https://azure.microsoft.com/overview/azure-stack/) página do produto.
- Para obter informações sobre o plano e a disponibilidade geográfica para o Azure Stack, sistemas integrados, consulte o white paper: [Azure Stack: Uma extensão do Azure](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/). 
- Para saber mais sobre pacotes e preços do Microsoft Azure Stack [transferir o PDF](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf). 

## <a name="next-steps"></a>Passos Seguintes
[Integração de rede do Centro de dados](azure-stack-network.md)
