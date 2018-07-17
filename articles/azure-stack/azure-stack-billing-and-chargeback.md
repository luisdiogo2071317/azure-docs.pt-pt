---
title: A faturação do cliente e de estorno no Azure Stack | Documentos da Microsoft
description: Saiba como obter informações de utilização de recursos do Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: brenduns
ms.reviewer: alfredop
ms.openlocfilehash: f055837711b52fc32cb387fb86c623d3502f47ab
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090144"
---
# <a name="usage-and-billing-in-azure-stack"></a>Utilização e faturação no Azure Stack

Este artigo descreve como os utilizadores do Azure Stack são cobrados a utilização de recursos. Pode aprender como as informações de faturas são acessadas para análise e de cobrança novamente.

O Azure Stack recolhe e grupos de dados de utilização de recursos utilizados. Em seguida, o Azure Stack reencaminha estes dados para Azure Commerce. Azure Commerce cobra-lhe para utilização do Azure Stack da mesma forma como ele seria faturar-lhe para utilização do Azure.

Também pode obter dados de utilização e a exportação para o seu próprio faturação ou estorno fazer uma cópia system com um adaptador de faturação ou exportá-lo a uma ferramenta de inteligência de negócio como o Microsoft Power BI.


## <a name="usage-pipeline"></a>Pipeline de utilização

Cada fornecedor de recursos no Azure Stack publica os dados de utilização por utilização de recursos. O serviço de utilização periodicamente (hora a hora e diariamente) agrega dados de utilização e armazena-os na base de dados de utilização. Operadores de pilha e os utilizadores do Azure podem acessar os dados de utilização armazenado através das APIs de utilização de recursos do Azure Stack. 

Se tiver [registado a sua instância do Azure Stack com o Azure](azure-stack-register.md), Azure Stack está configurado para enviar os dados de utilização para o Azure Commerce. Depois dos dados são carregados para o Azure, pode acessá-lo através do portal de faturação ou ao utilizar as APIs de utilização de recursos do Azure. Consulte a [relatórios de dados de utilização](azure-stack-usage-reporting.md) artigo para saber mais sobre a utilização de que os dados são reportados para o Azure.  

A imagem seguinte mostra os principais componentes no pipeline de utilização: 

![Pipeline de utilização](media\azure-stack-billing-and-chargeback\usagepipeline.png)

## <a name="what-usage-information-can-i-find-and-how"></a>As informações de utilização posso encontrar e como?

Fornecedores de recursos de pilha do Azure (por exemplo, computação, armazenamento e rede) geram dados de utilização em intervalos por hora para cada subscrição. Os dados de utilização contém informações sobre o recurso utilizado como nome de recurso, a subscrição utilizada e a quantidade usada. Para saber mais sobre os recursos de ID de medidores, consulte a [FAQ da API de utilização](azure-stack-usage-related-faq.md) artigo.

Depois que foram recolhidos os dados de utilização, é [reportado para o Azure](azure-stack-usage-reporting.md) para gerar uma fatura, o qual pode ser visualizada através do portal de faturação do Azure. 

> [!NOTE]  
> Relatórios de dados de utilização não é necessário para o Azure Stack Development Kit e para utilizadores de sistema integrado do Azure Stack portadores de licenças de acordo com o modelo de capacidade. Para saber mais sobre o licenciamento no Azure Stack, veja a [empacotamento e preços](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf) folha de dados.

O portal de faturação do Azure mostra dados de utilização para os recursos faturáveis. Além de recursos faturáveis, o Azure Stack captura dados de utilização para um conjunto mais amplo de recursos, o qual pode aceder no seu ambiente do Azure Stack através de REST APIs ou o PowerShell. Operadores do Azure Stack podem obter os dados de utilização para todas as subscrições de utilizador. Utilizadores individuais só podem obter os detalhes de utilização. 

## <a name="usage-reporting-for-multitenant-cloud-service-providers"></a>Utilização de relatórios para fornecedores de serviços Cloud de multi-inquilino

Um multi-inquilino fornecedor de serviços Cloud (CSP) que tenha muitos clientes com o Azure Stack pode desejar cada utilização do cliente de relatórios em separado, para que o fornecedor pode cobrar a utilização de diferentes subscrições do Azure. 

Cada cliente vai ter a sua identidade representada por um inquilino diferente do Azure Active Directory (Azure AD). O Azure Stack oferece suporte a atribuir uma subscrição do CSP para cada inquilino do Azure AD. Pode adicionar os inquilinos e as suas subscrições para o registo do Azure Stack base. O registo de base é feito para todas as pilhas do Azure. Se uma subscrição não está registrada para um inquilino, o utilizador ainda pode utilizar o Azure Stack e a utilização das mesmas será enviada para a subscrição utilizada para o registo de base. 


## <a name="next-steps"></a>Passos Seguintes

[Registe-se com o Azure Stack](azure-stack-registration.md)

[Reportar dados de utilização do Azure Stack para o Azure](azure-stack-usage-reporting.md)

[API de utilização de recursos do fornecedor](azure-stack-provider-resource-api.md)

[API de utilização de recursos de inquilino](azure-stack-tenant-resource-usage-api.md)

[Perguntas Freqüentes relacionadas com a utilização](azure-stack-usage-related-faq.md)