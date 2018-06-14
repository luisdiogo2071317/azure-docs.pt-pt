---
title: Faturação de cliente e o estorno na pilha do Azure | Microsoft Docs
description: Saiba como obter as informações de utilização de recursos do Azure pilha.
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
ms.date: 02/25/2018
ms.author: mabrigg
ms.reviewer: alfredop
ms.openlocfilehash: eca335797f48b7c44351655f17c8b6499f3d5999
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/12/2018
ms.locfileid: "29877488"
---
# <a name="usage-and-billing-in-azure-stack"></a>Utilização e faturação na pilha do Azure

Este artigo descreve como os utilizadores de pilha do Azure são cobrados para utilização de recursos. Pode saber como as informações de faturação são acedidas para análise e os encargos-back.

Pilha do Azure recolhe e grupos de dados de utilização para todos os recursos utilizados e reencaminha estes dados para comércio de Azure. Azure comércio bills, para utilização de pilha do Azure da mesma forma como este seria faturar-lhe para utilização do Azure.

Também pode obter dados de utilização e de exportação para as suas próprias encargos ou de faturação fazer uma cópia de sistema com um adaptador de faturação ou exportá-los para uma ferramenta de business intelligence, tais como o Microsoft Power BI e utilizá-la para análise.


## <a name="usage-pipeline"></a>Pipeline de utilização

Cada fornecedor de recursos na pilha do Azure emite dados de utilização por utilização de recursos. O serviço de utilização periodicamente (hora a hora e diariamente) agrega dados de utilização e armazena-os na base de dados de utilização. Do Azure operadores de pilha e os utilizadores podem aceder os dados de utilização armazenado através das APIs de utilização de recursos de pilha do Azure. 

Se tiver [registado a sua instância de pilha do Azure com o Azure](azure-stack-register.md), pilha do Azure está configurada para enviar os dados de utilização para comércio de Azure. Depois dos dados são carregados para o Azure, pode aceder através do portal de faturação ou através de APIs de utilização de recursos do Azure. Consulte o [relatórios de dados de utilização](azure-stack-usage-reporting.md) tópico para saber mais sobre a utilização de que dados são reportados para o Azure.  

A imagem seguinte mostra os componentes chave no pipeline de utilização: 

![Pipeline de utilização](media\azure-stack-billing-and-chargeback\usagepipeline.png)

## <a name="what-usage-information-can-i-find-and-how"></a>As informações de utilização posso encontrar e como?

Fornecedores de recursos de pilha do Azure, tais como a computação, armazenamento e rede, geram dados de utilização em intervalos por hora para cada subscrição. Os dados de utilização contém informações sobre o recurso utilizado como nome de recurso, a subscrição utilizada e a quantidade utilizado. Para saber mais sobre os recursos de ID de medidores, consulte o [FAQ de API de utilização](azure-stack-usage-related-faq.md) artigo.

Depois dos dados de utilização tenham sido recolhidos, é [reportado para o Azure](azure-stack-usage-reporting.md) para gerar uma fatura, que pode ser visualizada através do portal de faturação do Azure. 


> [!NOTE]
> Relatórios de dados de utilização não é necessário para o Kit de desenvolvimento de pilha do Azure e para os utilizadores de sistema de pilha do Azure integrado que licença no modelo de capacidade. Para saber mais sobre o licenciamento na pilha do Azure, consulte o [empacotamento e preços](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf) folha de dados.

O portal de faturação do Azure mostra os dados de utilização dos recursos cobráveis. Para além dos recursos de cobráveis, a pilha do Azure captura dados de utilização para um conjunto alargado de recursos, o qual pode aceder no seu ambiente de pilha do Azure através de REST APIs ou PowerShell. Os operadores do Azure da pilha podem obter os dados de utilização para todas as subscrições de utilizador. Utilizadores individuais só podem obter os detalhes de utilização. 

## <a name="usage-reporting-for-multitenant-cloud-service-providers"></a>Utilização de relatórios para o multi-inquilino fornecedores de serviços Cloud

Multitenant fornecedor de serviços em nuvem (CSP) que tenha muitos clientes utilizando a pilha do Azure pode pretender reportar separadamente, cada utilização de cliente para que o fornecedor pode cobram utilização para diferentes subscrições do Azure. 

Cada cliente vai ter a sua identidade representada por um inquilino do Azure Active Directory (Azure AD) diferente. Pilha do Azure suporta a atribuição uma subscrição de CSP para cada inquilino do Azure AD. Pode adicionar inquilinos e as suas subscrições para o registo de pilha do Azure base. O registo de base é feito para todas as pilhas do Azure. Se uma subscrição não está registada para um inquilino, o utilizador pode continuar a utilizar a pilha do Azure e respetiva utilização será enviada para a subscrição utilizada para o registo de base. 


## <a name="next-steps"></a>Passos Seguintes

[Registar a pilha do Azure](azure-stack-registration.md)

[Reportar dados de utilização de pilha do Azure para o Azure](azure-stack-usage-reporting.md)

[API de utilização de recursos do fornecedor](azure-stack-provider-resource-api.md)

[API de utilização de recursos de inquilino](azure-stack-tenant-resource-usage-api.md)

[FAQ relacionados com a utilização](azure-stack-usage-related-faq.md)