---
title: Gerir perfis de versão de API no Azure Stack | Documentos da Microsoft
description: Saiba mais sobre os perfis de versão de API no Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: sijuman
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: 28ff7c9f6ca5fc6365b3fe1b9a91d2159c8b3f48
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55247617"
---
# <a name="manage-api-version-profiles-in-azure-stack"></a>Gerir perfis de versão de API no Azure Stack

*Aplica-se a: Integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Perfis de API especificar o fornecedor de recursos do Azure e a versão de API para pontos finais REST do Azure. Pode criar clientes personalizados em idiomas diferentes através de perfis de API. Cada cliente utiliza um perfil de API para contactar o fornecedor de recursos corretos e a versão de API para o Azure Stack.

Pode criar uma aplicação para trabalhar com fornecedores de recursos do Azure sem ter de classificar exatamente qual versão de cada API do fornecedor de recursos é compatível com o Azure Stack. Alinhar apenas seu aplicativo para um perfil; o SDK é revertido para a versão de API correta.

Este tópico ajuda-o:

 - Compreenda os perfis de API para o Azure Stack.
 - Saiba como pode utilizar perfis de API para desenvolver as suas soluções.
 - Veja onde encontrar orientações específicas de código.

## <a name="summary-of-api-profiles"></a>Resumo dos perfis de API

- Os perfis de API são utilizados para representar um conjunto de fornecedores de recursos do Azure e suas versões de API.
- Perfis de API foram criados para a criação de modelos em várias clouds do Azure. Perfis de fornecem uma interface compatível e estável.
- Os perfis são lançados quatro vezes por ano.
- São utilizadas três convenções de nomenclatura de perfil:
    - **latest**  
        Contém as versões de API mais recentes, lançadas no global Azure.
    - **yyyy-mm-dd-hybrid**  
    Lançado bianual, esta versão se concentra na consistência e a estabilidade em várias clouds. Este perfil se destina a compatibilidade do Azure Stack ideal.
    - **aaaa-mm-dd-perfil** equilibra estabilidade ideal e as funcionalidades mais recentes.

## <a name="azure-api-profiles-and-azure-stack-compatibility"></a>Perfis de API do Azure e a compatibilidade do Azure Stack

Os perfis de API do Azure mais recentes não são compatíveis com o Azure Stack. Pode utilizar as seguintes convenções de nomenclatura para identificar quais perfis a utilizar para as suas soluções do Azure Stack.

**Latest**  
Este perfil tem as versões mais atualizadas API encontradas no global Azure, que não funcionam no Azure Stack. **Mais recente** tem o maior número de alterações de última hora. O perfil coloca à parte, estabilidade e compatibilidade com outras clouds. Se estiver a tentar usar as versões de API mais atualizadas, **mais recente** é o perfil que deve utilizar.

**Yyyy-mm-dd-hybrid**  
Este perfil é lançado em Março e Setembro, todos os anos. Ele tem ideal estabilidade e compatibilidade com várias nuvens. **Aaaa-mm-dd-híbrida** foi concebido para o Azure global e o Azure Stack de destino. As versões de API do Azure listadas neste perfil serão os mesmos que aqueles que estão listados no Azure Stack. Pode utilizar este perfil para desenvolver o código de soluções na cloud híbrida.

**yyyy-mm-dd-profile**  
Este perfil é lançado para o global Azure em Junho e Dezembro. Não funciona com o Azure Stack; Normalmente, haverá muitas alterações de última hora. Embora ele faz o balanceamento de estabilidade ideal e as funcionalidades mais recentes, a diferença entre **mais recente** e este perfil é que **mais recente** sempre consiste nas versões de API mais recentes, independentemente de quando foi a API lançado. Por exemplo, se uma nova versão de API é criada para a API de computação amanhã, essa versão de API está listado na **mais recente**, mas não na **aaaa-mm-dd-perfil** perfil, porque este perfil já existe. **aaaa-mm-dd-perfil** abrange as versões mais atualizadas lançadas antes de Junho ou antes de Dezembro.

## <a name="azure-resource-manager-api-profiles"></a>Perfis de API do Resource Manager do Azure

O Azure Stack não usa a versão mais recente das versões de API encontradas no global Azure. Quando cria uma solução, tem de encontrar a versão de API para cada fornecedor de recursos do Azure que é compatível com o Azure Stack.

Em vez de cada fornecedor de recursos e a versão específica suportados pelo Azure Stack de pesquisa, pode utilizar um perfil de API. O perfil Especifica um conjunto de fornecedores de recursos e as versões de API. O SDK, ou uma ferramenta criada com o SDK irá reverter para o destino `api-version` especificada no perfil. Com perfis de API, pode especificar uma versão de perfil que se aplica a um modelo de todo e em tempo de execução, o Azure Resource Manager seleciona a versão correta do recurso.

Perfis de API trabalham com ferramentas que utilizam o Azure Resource Manager, como o PowerShell, CLI do Azure, o código fornecido no SDK e Microsoft Visual Studio. Ferramentas e SDKs podem utilizar perfis para ler a versão dos módulos e bibliotecas a incluir ao criar um aplicativo.

Por exemplo, se utilizar o PowerShell para criar uma conta de armazenamento com o **Microsoft. Storage** fornecedor de recursos, que suporta **versão de api** 2016-03-30 e uma VM com o  **Microsoft. Compute** fornecedor de recursos com **api-version** 2015-12-01, deve pesquisar que suporte de módulo do PowerShell 2016-03-30 para o armazenamento e qual módulo suporta 2015-02-01 para computação e, em seguida, instalá-los. Em vez disso, pode utilizar um perfil. Utilize o cmdlet `Install-Profile <profilename>`, e o PowerShell carrega a versão correta dos módulos.

Da mesma forma, ao utilizar o SDK de Python para criar uma aplicação baseada em Python, pode especificar o perfil. O SDK carrega os módulos certos para os fornecedores de recursos que especificou no script.

Como desenvolvedor, possa se concentrar em escrever a sua solução. Em vez de pesquisas sobre que versões de API, o fornecedor de recursos e a nuvem funcionam em conjunto, pode utilizar um perfil e saber que seu código funciona em todas as nuvens que oferecem suporte a esse perfil.

## <a name="api-profile-code-samples"></a>Exemplos de código do perfil de API

Pode encontrar exemplos de código para o ajudar a integrar a sua solução com a sua linguagem preferida com o Azure Stack através de perfis. Atualmente, pode encontrar exemplos e orientação para os seguintes idiomas:

- **.NET** pode utilizar o perfil de .NET API para obter a versão mais recente, mais estável de cada tipo de recurso num pacote de fornecedor de recursos. Para obter mais informações, consulte [perfis de versão de utilizar a API com .NET no Azure Stack](azure-stack-version-profiles-net.md).
- **PowerShell**  
Pode utilizar o **AzureRM.Bootstrapper** disponível através da galeria do PowerShell para obter os cmdlets do PowerShell necessários para trabalhar com perfis de versão de API do módulo. Para obter informações, consulte [perfis de versão de API de utilização para o PowerShell](azure-stack-version-profiles-powershell.md).
- **CLI do Azure**  
Pode atualizar a configuração do seu ambiente para utilizar o perfil de versão de API específico do Azure Stack. Para obter informações, consulte [perfis de versão de API de utilização para a CLI do Azure](azure-stack-version-profiles-azurecli2.md).
- **Go**  
No Go SDK, um perfil é uma combinação de diferentes tipos de recursos com versões diferentes de diferentes serviços. Os perfis estão disponíveis em perfis/caminho, com a respetiva versão na **DD-MM-AAAA** formato. Para obter informações, consulte [perfis de versão de API de utilização para Go](azure-stack-version-profiles-go.md).
- **Ruby**  
O SDK de Ruby para o Azure Stack Resource Manager fornece ferramentas para ajudar a criar e gerir a sua infraestrutura. Fornecedores de recursos no SDK incluem computação, redes virtuais e armazenamento com a linguagem Ruby. Para obter informações, consulte [perfis de versão de API de utilização com Ruby](azure-stack-version-profiles-ruby.md)
- **Python**  
O Python SDK suporta perfis de versão de API para plataformas de cloud diferente, como o Azure Stack e o global Azure. Pode utilizar perfis de API para criar soluções para uma cloud híbrida. Para obter informações, consulte [perfis de versão de utilizar a API com Python](azure-stack-version-profiles-python.md)

## <a name="next-steps"></a>Passos Seguintes

* [Instalar o PowerShell para o Azure Stack](azure-stack-powershell-install.md)
* [Configurar o ambiente do PowerShell do utilizador do Azure Stack](azure-stack-powershell-configure-user.md)
* [Veja os detalhes sobre as versões de API do fornecedor de recursos suportadas pelos perfis](azure-stack-profiles-azure-resource-manager-versions.md).
