---
title: Gestão da região no Azure Stack | Documentos da Microsoft
description: Descrição geral da gestão da região no Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: e94775d5-d473-4c03-9f4e-ae2eada67c6c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2018
ms.author: sethm
ms.reviewer: efemmano
ms.openlocfilehash: 401b81ceb7ab71528a4ad11bc7d8944b4d732933
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "49078870"
---
# <a name="region-management-in-azure-stack"></a>Gestão da região no Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

O Azure Stack usa o conceito de regiões, o que são entidades lógicas compostas os recursos de hardware que compõem a infraestrutura do Azure Stack. Dentro de gestão da região, pode encontrar todos os recursos necessários para operar com êxito a infraestrutura do Azure Stack.

Integrada uma implementação do sistema (conhecido como um *cloud do Azure Stack*) constitui uma única região. Cada Development Kit do Azure Stack tem uma região, com o nome **local**. Se implementar um segundo sistema integrado do Azure Stack ou configurar outra instância do kit de desenvolvimento num hardware separado, esta nuvem do Azure Stack é uma região diferente.

## <a name="information-available-through-the-region-management-tile"></a>Informações disponíveis por meio do mosaico de gestão da região

O Azure Stack tem um conjunto de capacidades de gestão de região disponíveis na **gestão da região** mosaico. Este mosaico não está disponível para um operador do Azure Stack no dashboard predefinido no portal do administrador. Através deste mosaico, pode monitorizar e atualizar a sua região do Azure Stack e os respetivos componentes, que são específicos de cada região.

 ![O mosaico de gestão da região](media/azure-stack-manage-region/image1.png)

 Se clicar numa região no mosaico de gestão da região, pode acessar as seguintes informações:

  ![Descrição de painéis no painel de gestão da região](media/azure-stack-manage-region/image2.png)

1. **Menu de recursos**. Aqui, pode acessar áreas de gerenciamento de infra-estrutura específicas e ver e gerir recursos de utilizador, como contas de armazenamento e redes virtuais.

2. **Alertas**. Isto apresenta uma lista de alertas de todo o sistema e fornece detalhes sobre cada um dos alertas.

3. **Atualizações**. Aqui pode ver a versão atual da sua infraestrutura do Azure Stack, as atualizações disponíveis e o histórico de atualização. Também pode atualizar o seu sistema integrado.

4. **Fornecedores de recursos**. Fornecedores de recursos é o local para gerir a funcionalidade de utilizador fornecida pelos componentes necessários para executar o Azure Stack. Cada fornecedor de recursos é fornecido com uma experiência administrativa. Esta experiência pode incluir alertas para o fornecedor específico, métricas e outras capacidades de gestão específicas para o fornecedor de recursos.

5. **Funções de infraestrutura**. Funções de infraestrutura são os componentes necessários para executar o Azure Stack. Apenas as funções de infraestrutura que reportam os alertas são listadas. Ao selecionar uma função, pode ver os alertas associados a função e as instâncias de função onde esta função está em execução.

## <a name="next-steps"></a>Passos Seguintes

- [Monitorizar o estado de funcionamento e alertas no Azure Stack](azure-stack-monitor-health.md)
- [Gerir atualizações no Azure Stack](azure-stack-updates.md)
