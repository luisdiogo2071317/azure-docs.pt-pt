---
title: Configurar a identidade de cloud híbrida com as aplicações do Azure e o Azure Stack | Documentos da Microsoft
description: Saiba como configurar a identidade de cloud híbrida com aplicações do Azure e o Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/14/2019
ms.author: mabrigg
ms.reviewer: anajod
ms.lastreviewed: 01/14/2019
ms.openlocfilehash: d36b94ac9f130649e880e6bff1fa3a8b223828b4
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55244413"
---
# <a name="tutorial-configure-hybrid-cloud-identity-for-azure-and-azure-stack-applications"></a>Tutorial: Configurar a identidade de cloud híbrida para aplicações do Azure e o Azure Stack

*Aplica-se a: Integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Saiba como configurar uma identidade de cloud híbrida para as suas aplicações do Azure e o Azure Stack.

Tem duas opções para conceder acesso às suas aplicações no global Azure e o Azure Stack.

 * Quando o Azure Stack tem uma ligação contínua à Internet, pode utilizar o Azure Active Directory (Azure AD).
 * Quando o Azure Stack é desligado da Internet, pode usar a Azure Directory Federated Services (AD FS).

Utilizar principais de serviço para conceder acesso às suas aplicações do Azure Stack com o objetivo de implementação ou de configuração com o Azure Resource Manager no Azure Stack.

Neste tutorial, irá criar um ambiente de exemplo para:

> [!div class="checklist"]
> - Estabelecer uma identidade híbrida no global do Azure e o Azure Stack
> - Obter um token para aceder à API do Azure Stack.

Tem de ter permissões de operador do Azure Stack para obter os passos neste tutorial.

> [!Tip]  
> ![pillars.png híbrida](./media/azure-stack-solution-cloud-burst/hybrid-pillars.png)  
> Microsoft Azure Stack é uma extensão do Azure. O Azure Stack coloca a agilidade e inovação da cloud de informática para o seu ambiente no local e ativar a única cloud híbrida que permite-lhe criar e implementar aplicações de híbridas em qualquer lugar.  
> 
> O White Paper [considerações de Design para aplicações híbridas](https://aka.ms/hybrid-cloud-applications-pillars) analisa os pilares de qualidade de software (posicionamento, escalabilidade, disponibilidade, resiliência, capacidade de gerenciamento e segurança) para criação, implantação e operação do híbrida aplicações. As considerações de design ajudam a otimizar o design de aplicações híbridas, minimizando desafios em ambientes de produção.


## <a name="create-a-service-principal-for-azure-ad-in-the-portal"></a>Criar um principal de serviço para o Azure AD no portal

Se implementar o Azure Stack com o Azure AD como o repositório de identidades, pode criar principais de serviço, tal como fazer para o Azure. O [criar principais de serviço](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-create-service-principals#create-service-principal-for-azure-ad) artigo mostra-lhe como realizar os passos no portal. Verifique que tem o [do Azure AD permissões obrigatórias](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#required-permissions) antes de começar.

## <a name="create-a-service-principal-for-ad-fs-using-powershell"></a>Criar um principal de serviço para o AD FS com o PowerShell

Se tiver implementado o Azure Stack com o AD FS, pode utilizar o PowerShell para criar um principal de serviço, atribuir uma função de acesso e iniciar sessão a partir do PowerShell usando essa identidade. [Criar principal de serviço para o AD FS](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-create-service-principals#create-service-principal-for-ad-fs) mostra-lhe como realizar os passos necessários com o PowerShell.

## <a name="using-the-azure-stack-api"></a>Usando a API do Azure Stack

O [API do Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-rest-api-use) tutorial explica-lhe o processo de obtenção de um token para aceder à API do Azure Stack.

## <a name="connect-to-azure-stack-using-powershell"></a>Ligar ao Azure Stack com o Powershell

O início rápido [para começar a trabalhar com o PowerShell no Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-configure-quickstart) explica-lhe os passos necessários para instalar o Azure PowerShell e ligue-se à sua instalação do Azure Stack.

### <a name="prerequisites"></a>Pré-requisitos

Uma instalação do Azure Stack ligado ao Azure Active Directory com uma subscrição, que pode aceder. Se não tiver uma instalação do Azure Stack, pode utilizar estas instruções para configurar uma [Development Kit do Azure Stack](https://docs.microsoft.com/azure/azure-stack/asdk/asdk-deploy).

#### <a name="connect-to-azure-stack-using-code"></a>Ligar ao Azure Stack usando código

Para ligar ao Azure Stack através de código, utilize os pontos finais do Azure Resource Manager API para obter a autenticação e os pontos finais de gráfico para a sua instalação do Azure Stack e, em seguida, efetuar a autenticação com pedidos REST. Pode encontrar um exemplo de aplicação de cliente no [GitHub](https://github.com/shriramnat/HybridARMApplication).

>[!Note]
>A menos que o Azure SDK para a linguagem de sua escolha suporta perfis de API do Azure, o SDK pode não funcionar com o Azure Stack. Para saber mais sobre os perfis de API do Azure, veja a [gerir perfis de versão de API](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-version-profiles) artigo.

## <a name="next-steps"></a>Passos Seguintes

 - Para saber mais sobre como a identidade é processada no Azure Stack, veja [arquitetura de identidade para o Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-identity-architecture).
 - Para saber mais sobre os padrões de Cloud do Azure, veja [padrões de conceção de Cloud](https://docs.microsoft.com/azure/architecture/patterns).
