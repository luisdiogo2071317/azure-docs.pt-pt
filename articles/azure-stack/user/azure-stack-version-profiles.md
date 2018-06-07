---
title: Gerir perfis de versão de API no Azure pilha | Microsoft Docs
description: Saiba mais sobre perfis de versão de API na pilha do Azure.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2018
ms.author: mabrigg
ms.reviewer: sijuman
ms.openlocfilehash: adbe88a44ac38868a68a6845c328ef4cf7fba60c
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34604442"
---
# <a name="manage-api-version-profiles-in-azure-stack"></a>Gerir perfis de versão de API na pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Perfis de API especificar o fornecedor de recursos do Azure e a versão da API para pontos finais REST do Azure. Pode criar clientes personalizados em idiomas diferentes através de perfis de API. Cada cliente utiliza um perfil de API para contactar o fornecedor de recursos correto e a versão de API de pilha do Azure.

Pode criar uma aplicação para trabalhar com fornecedores de recursos do Azure, sem ter de ordenar exatamente a versão de cada fornecedor de recursos API é compatível com a pilha do Azure. Apenas alinhar a sua aplicação para um perfil; o SDK reverte para a versão de API correta.

Este tópico ajuda-o:

 - Compreenda os perfis de API de pilha do Azure.
 - Saiba como pode utilizar perfis de API para desenvolver as suas soluções.
 - Ver onde deve encontrar orientações específicas do código.

## <a name="summary-of-api-profiles"></a>Resumo dos perfis de API

- Perfis de API são utilizados para representar um conjunto de fornecedores de recursos do Azure e as respetivas versões de API.
- Perfis de API foram criadas para a criação de modelos em várias nuvens do Azure. Perfis foram concebidos para satisfazer a necessidade de uma interface compatível e estável.
- Perfis são lançados quatro vezes um ano.
- São utilizadas três convenções de nomenclatura de perfil:
    - **latest**  
        Contém as versões de API mais recentes, lançadas em global do Azure.
    - **yyyy-mm-dd-hybrid**  
    Lançadas a uma cadência biannual, esta versão está centrado nas consistência e a estabilidade em várias nuvens. Este perfil destina-se ideal compatibilidade de pilha do Azure.
    - **aaaa-mm-dd-perfil** encontra-se entre ideal estabilidade e às funcionalidades mais recentes.

### <a name="azure-api-profiles-and-azure-stack-compatibility"></a>Perfis de API do Azure e a compatibilidade de pilha do Azure

Os perfis de API do Azure mais recentes não são compatíveis com a pilha do Azure. Pode utilizar as seguintes convenções de nomenclatura para identificar os perfis da utilizar para as suas soluções de pilha do Azure.

**Latest**  
Este perfil tem as versões de API mais atualizadas à sua encontradas no Azure global, que não funcionará na pilha do Azure. **Mais recente** tem o maior número de alterações. O perfil coloca reservados estabilidade e a compatibilidade com outras nuvens. Se estiver a tentar utilizar as versões de API mais atualizadas, **mais recente** o perfil deve utilizar.

**Aaaa-mm-dd-híbrida**  
Este perfil é lançado em Março e Setembro, todos os anos. Este perfil tem ideal estabilidade e a compatibilidade com várias nuvens. **Aaaa-mm-dd-híbrida** foi concebido para global do Azure e pilha do Azure de destino. As versões de API do Azure listadas neste perfil serão os mesmos que as que estão listadas na pilha do Azure. Pode utilizar este perfil para desenvolver o código para soluções de nuvem híbrida.

**yyyy-mm-dd-profile**  
Este perfil é libertado para o global Azure em Junho e Dezembro. Este perfil não funcionará em relação a pilha do Azure; Normalmente, existirão muitas alterações. Embora encontra-se entre ideal estabilidade e funcionalidades mais recentes, a diferença entre **mais recente** e este perfil é que **mais recente** será sempre consistir em versões de API mais recentes, independentemente de quando a API foi lançada. Por exemplo, se uma nova versão de API é criada para a API de computação amanhã, essa versão de API será listada no **mais recente**, mas não no **aaaa-mm-dd-perfil** porque já existe este perfil.  **aaaa-mm-dd-perfil** aborda as versões mais atualizadas à sua lançadas antes de Junho ou antes de Dezembro.

## <a name="azure-resource-manager-api-profiles"></a>Perfis de API do Gestor de recursos do Azure

Pilha do Azure não utiliza a versão mais recente das versões de API encontradas no global Azure. Quando estiver a criar uma solução, tem de encontrar a versão da API para cada fornecedor de recursos do Azure que é compatível com a pilha do Azure.

Em vez de pesquisar cada fornecedor de recursos e a versão específica suportado pela pilha do Azure, pode utilizar um perfil de API. O perfil Especifica um conjunto de fornecedores de recursos e versões de API. O SDK, ou uma ferramenta criadas com o SDK, irá reverter para a api-version de destino especificada no perfil. Com perfis de API, pode especificar uma versão de perfil que se aplica a um modelo completo e, em runtime, o Azure Resource Manager seleciona a versão correta do recurso.

Os perfis de API funcionar com as ferramentas que utilizar o Azure Resource Manager, tais como o PowerShell, o CLI do Azure, o código fornecido no SDK e o Microsoft Visual Studio. SDKs e ferramentas podem utilizar perfis para ler a versão de módulos e bibliotecas para incluir quando criar uma aplicação.

Por exemplo, se utilizar o PowerShell para criar um armazenamento conta através do **Microsoft** fornecedor de recursos, que suporta a api-version 2016-03-30 e uma VM com o fornecedor de recursos Microsoft. Compute api-version 2015-12-01 , terá de procurar que suporte de módulo do PowerShell 2016-03-30 para armazenamento e o módulo suporta 2015-02-01 para computação e instalá-las. Em vez disso, pode utilizar um perfil. Utilize o cmdlet * * instalar perfil * profilename *** e PowerShell carrega as versões corretas dos módulos.

Da mesma forma, ao utilizar o SDK Python para criar uma aplicação baseada no Python, pode especificar o perfil. O SDK carrega os módulos de direita para os fornecedores de recursos que especificou no script.

Como um programador pode concentrar-se sobre como escrever a sua solução. Em vez de a pesquisar as versões de api, o fornecedor de recursos e que em nuvem funciona em conjunto, utilize um perfil e saber que o seu código irá funcionar em todas as nuvens que suportam esse perfil.

## <a name="api-profile-code-samples"></a>Exemplos de código do perfil de API

Pode encontrar exemplos de código para o ajudar a integrar a solução com o seu idioma de preferência com pilha do Azure através da utilização de perfis. Atualmente, pode encontrar exemplos e documentação de orientação para os seguintes idiomas:

- **PowerShell**  
Pode utilizar o **AzureRM.Bootstrapper** módulo disponível através de galeria do PowerShell para obter os cmdlets do PowerShell necessários para trabalhar com perfis de versão de API. Para informações, consulte [perfis de versão de API de utilização para o PowerShell](azure-stack-version-profiles-powershell.md).
- **CLI 2.0 do Azure**  
Pode atualizar a configuração do ambiente para utilizar o perfil de versão de API específico da pilha do Azure. Para informações, consulte [perfis de versão de API de utilização para o Azure CLI 2.0](azure-stack-version-profiles-azurecli2.md).
- **ACEDA**  
SDK ACEDA, um perfil é uma combinação de tipos de recursos diferente com diferentes versões de diferentes serviços. perfis estão disponíveis em perfis da / caminho, com a respetiva versão no **aaaa-MM-DD** formato. Para informações, consulte [perfis de versão de API de utilização para ir](azure-stack-version-profiles-go.md).
- **Ruby**  
O SDK de Ruby para o Gestor de recursos de pilha do Azure fornece ferramentas para ajudar a criar e gerir a sua infraestrutura. Fornecedores de recursos no SDK incluem computação, redes virtuais e armazenamento com o idioma Ruby. Para informações, consulte [perfis de versão de API de utilização com Ruby](azure-stack-version-profiles-ruby.md)
- **Python**  
- O SDK Python suporta perfis de versão de API para plataformas de nuvem diferente, tais como a pilha do Azure e global do Azure de destino. Pode utilizar perfis de API na criação de soluções para uma nuvem híbrida. Para informações, consulte [perfis de versão de API de utilização com o Python](azure-stack-version-profiles-python.md)

## <a name="next-steps"></a>Passos Seguintes

* [Instalar o PowerShell para o Azure Stack](azure-stack-powershell-install.md)
* [Configurar o Azure pilha ambiente do utilizador do PowerShell](azure-stack-powershell-configure-user.md)
* [Reveja os detalhes sobre as versões de API do fornecedor de recursos suportados pelos perfis de](azure-stack-profiles-azure-resource-manager-versions.md).