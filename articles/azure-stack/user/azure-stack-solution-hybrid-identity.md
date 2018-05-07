---
title: Configurar a identidade de nuvem híbrida com aplicações do Azure e Azure pilha | Microsoft Docs
description: Saiba como configurar a identidade de nuvem híbrida com aplicações do Azure e de pilha do Azure.
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
ms.date: 05/07/2018
ms.author: mabrigg
ms.reviewer: Anjay.Ajodha
ms.openlocfilehash: 9a025716c2bb6266c1c1c552a2d0791b39429cac
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/03/2018
---
# <a name="tutorial-configure-hybrid-cloud-identity-with-azure-and-azure-stack-applications"></a>Tutorial: Configurar a identidade de nuvem híbrida com aplicações do Azure e de pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Tem duas opções para conceder acesso às suas aplicações no global do Azure e pilha do Azure. Quando a pilha do Azure tem uma ligação de expedição à Internet, pode utilizar o Azure Active Directory (Azure AD). Quando a pilha do Azure está desligada da Internet, pode utilizar o Azure Directory Federated Services (AD FS). Utilize principais de serviço para conceder acesso às suas aplicações na pilha do Azure com vista à, a implementação ou de configuração através do Configuration Manager de recursos do Azure na pilha do Azure. 

Neste tutorial, irá criar um ambiente de exemplo para:

> [!div class="checklist"]
> * Estabelecer uma identidade híbrida no global do Azure e pilha do Azure
> * Obter um token para aceder à API de pilha do Azure.

Estes passos requerem que tenha permissões de operador de pilha do Azure.

## <a name="create-a-service-principal-for-azure-ad-in-the-portal"></a>Criar um principal de serviço para o Azure AD no portal

Se implementou a pilha do Azure com o Azure AD como o arquivo de identidade, pode criar principais de serviço, tal como, fazê-lo para o Azure. [Este documento](https://docs.microsoft.com/en-us/azure/azure-stack/user/azure-stack-create-service-principals#create-service-principal-for-azure-ad) mostra como efetuar os passos através do portal. Verifique se tem o [necessárias permissões do Azure AD](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal#required-permissions) antes do início.

## <a name="create-a-service-principal-for-ad-fs-using-powershell"></a>Criar um principal de serviço para o AD FS com o PowerShell

Se tiver implementado a pilha do Azure com o AD FS, pode utilizar o PowerShell para criar um principal de serviço, atribuir uma função de acesso e iniciar sessão a partir do PowerShell com essa identidade. [Este documento](https://docs.microsoft.com/en-us/azure/azure-stack/user/azure-stack-create-service-principals#create-service-principal-for-ad-fs) mostra como efetuar os passos com o PowerShell.

## <a name="using-the-azure-stack-api"></a>Utilizar a API de pilha do Azure

[Este tutorial](https://docs.microsoft.com/en-us/azure/azure-stack/user/azure-stack-rest-api-use) irá guiá-lo durante o processo de obtenção de um token para aceder à API de pilha do Azure.

## <a name="connect-to-azure-stack-using-powershell"></a>Ligar a pilha do Azure com o Powershell

Eis um script de exemplo utilizando os conceitos taught neste documento para estabelecer a ligação com a pilha do Azure.

### <a name="prerequisites"></a>Pré-requisitos

Uma instalação de pilha do Azure ligado ao Azure Active Directory com uma subscrição, que pode aceder. Se não tiver uma instalação de pilha do Azure, pode utilizar estas instruções para configurar um [Kit de desenvolvimento de pilha do Azure](https://docs.microsoft.com/en-us/azure/azure-stack/asdk/asdk-deploy).

[Este tutorial](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-powershell-configure-quickstart) explica os passos necessários para instalar o Azure PowerShell e ligue à sua instalação de pilha do Azure.

#### <a name="connect-to-azure-stack-using-code"></a>Ligar a pilha do Azure com o código

Para ligar a pilha do Azure com o código, utilize os pontos finais do Azure Resource Manager API para obter a autenticação e os pontos finais de gráfico para a instalação de pilha do Azure e, em seguida, efetuar a autenticação com pedidos REST. Pode encontrar um exemplo de aplicação [aqui](https://github.com/shriramnat/HybridARMApplication).

> [!note]  
A menos que o Azure SDK para o seu idioma à escolha suporta perfis de API do Azure, o SDK pode não funcionar com a pilha do Azure. Para obter mais informações sobre perfis de API do Azure, aceda [aqui](https://docs.microsoft.com/da-dk/azure/azure-stack/user/azure-stack-version-profiles).

## <a name="next-steps"></a>Passos Seguintes

 - Para obter mais informações sobre como identidade é processada na pilha do Azure, consulte o artigo [arquitetura de identidade para o Azure pilha](https://docs.microsoft.com/azure/azure-stack/azure-stack-identity-architecture).  
 - Para saber mais sobre os padrões de nuvem do Azure, consulte [padrões de conceção de nuvem](https://docs.microsoft.com/azure/architecture/patterns).
