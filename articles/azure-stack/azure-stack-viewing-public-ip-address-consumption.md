---
title: Ver o consumo de endereço IP público na pilha do Azure | Microsoft Docs
description: Os administradores podem ver o consumo de endereços IP públicos numa região
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 0f77be49-eafe-4886-8c58-a17061e8120f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/28/2018
ms.author: mabrigg
ms.openlocfilehash: 50bf01d6de6105d3041c6bb88e803f3d110f751d
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/02/2018
ms.locfileid: "29742463"
---
# <a name="view-public-ip-address-consumption-in-azure-stack"></a>Ver o consumo de endereço IP público na pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Como um administrador da nuvem, pode ver:
 - O número de endereços IP públicos que teriam sido alocados aos inquilinos.
 - O número de endereços IP públicos que ainda estão disponíveis para alocação.
 - A percentagem de endereços IP públicos que teriam sido alocados nessa localização.

O **utilização de agrupamentos de IP público** mosaico mostra o número de endereços IP públicos consumido em conjuntos de endereços IP públicos. Para cada endereço IP, o mosaico mostra a utilização do inquilino VM do IaaS instâncias, os serviços de infraestrutura de recursos de infraestrutura e recursos do endereço IP público que foram criados explicitamente pelos inquilinos.

O objetivo do mosaico é atribuir operadores do Azure pilha uma noção do número de endereços IP públicos utilizados nesta localização. O número ajuda os administradores de determinar se estiverem a executar baixas neste recurso.

O **endereços IP públicos** item de menu em **recursos de inquilino** lista apenas os endereços IP públicos que tenham sido *explicitamente criado por inquilinos*. Pode encontrar o item de menu no **fornecedores de recursos**, **rede** painel. O número de **utilizado** endereços IP público no **utilização de agrupamentos de IP público** mosaico sempre é diferente do (superior) o número no **endereços IP públicos** mosaico em  **Recursos de inquilino**.

## <a name="view-the-public-ip-address-usage-information"></a>Ver as informações de utilização de endereço IP públicas
Para ver o número total de endereços IP públicos que ter sido consumida na região:

1. No portal de administrador de pilha do Azure, selecione **mais serviços**, em **recursos administrativo**, selecione **fornecedores de recursos**.
2. Na lista de **fornecedores de recursos**, selecione **rede**.
3. O **rede** painel apresenta o **utilização de agrupamentos de IP público** na peça de mosaico do **descrição geral** secção.

![Painel do fornecedor de recursos de rede](media/azure-stack-viewing-public-ip-address-consumption/image01.png)

O **utilizado** número representa o número de atribuir endereços IP públicos do conjuntos de endereços IP públicos. O **livres** número representa o número de IP público endereços de IP público endereços agrupamentos que não foram atribuídos e ainda estão disponíveis. O **utilizada** número representa o número de utilizado ou atribuídos endereços como uma percentagem do número total de endereços IP públicos no conjuntos de endereços IP públicos nessa localização.

## <a name="view-the-public-ip-addresses-that-were-created-by-tenant-subscriptions"></a>Ver os endereços IP públicos que foram criados por subscrições de inquilino
Selecione **endereços IP públicos** em **recursos de inquilino**. Reveja a lista de endereços IP públicos explicitamente criado por subscrições de inquilino numa região específica.

![Endereços IP públicos do inquilino](media/azure-stack-viewing-public-ip-address-consumption/image02.png)

Poderá reparar que alguns endereços IP públicos que teriam sido alocados dinamicamente aparecem na lista. No entanto, um endereço não foi associado com os mesmos ainda. Foi criado o recurso de endereço do fornecedor de recursos de rede, mas ainda não no controlador de rede.

O controlador de rede não atribuir um endereço para o recurso até está vinculada a uma interface, uma placa de interface de rede (NIC), um balanceador de carga ou um gateway de rede virtual. Quando o endereço IP público está vinculada a uma interface, controlador de rede atribui um endereço IP. O endereço é apresentado no **endereço** campo.

## <a name="view-the-public-ip-address-information-summary-table"></a>Ver a pública IP endereço informações tabela de resumo
Em casos diferentes, são atribuídos endereços IP públicos que determinam se o endereço é apresentado na lista de um ou outro.

| **Caso de atribuição de endereço IP público** | **É apresentada no resumo de utilização** | **É apresentada na lista de endereços IP pública do inquilino** |
| --- | --- | --- |
| Endereço IP público dinâmico ainda não foi atribuído a um NIC ou Balanceador de carga (temporário) |Não |Sim |
| Dinâmico endereço IP público atribuído a um NIC ou Balanceador de carga. |Sim |Sim |
| Endereço IP público estático atribuído a um inquilino NIC ou Balanceador de carga. |Sim |Sim |
| Endereço IP público estático atribuído a um ponto de final de serviço da infraestrutura de recursos de infraestrutura. |Sim |Não |
| Endereço IP público implicitamente criado para instâncias de VM do IaaS e utilizada para NAT de saída na rede virtual. Estes são criados nos bastidores sempre que um inquilino cria uma instância VM para que as VMs podem enviar informações à Internet. |Sim |Não |

## <a name="next-steps"></a>Passos Seguintes
[Gerir contas de armazenamento na pilha do Azure](azure-stack-manage-storage-accounts.md)