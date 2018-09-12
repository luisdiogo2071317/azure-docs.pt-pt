---
title: Substituir um nó de unidade de escala num sistema integrado do Azure Stack | Documentos da Microsoft
description: Saiba como substituir um nó de unidade de escala físicos num sistema integrado do Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: f9434689-ee66-493c-a237-5c81e528e5de
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2018
ms.author: mabrigg
ms.openlocfilehash: 1b37b150dad4951a4ade81f226b515ce9cae9053
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44377059"
---
# <a name="replace-a-scale-unit-node-on-an-azure-stack-integrated-system"></a>Substituir um nó de unidade de escala num sistema integrado do Azure Stack

*Aplica-se a: sistemas integrados do Azure Stack*

Este artigo descreve o processo geral para substituir um computador físico (também conhecido como um *nó de unidade de escala*) numa pilha de Azure sistema integrado. Substituição de nó de unidade de escala real passos irão variar com base no seu fornecedor de hardware do fabricante de equipamento original (OEM). Consulte a documentação do fornecedor campo unidade substituível em (FRU) para obter passos detalhados que são específicas para o seu sistema.

O diagrama de fluxo que se segue mostra o processo FRU geral para substituir um nó de unidade de dimensionamento inteiro.

![Gráfico de fluxo de processo do nó de substituição](media/azure-stack-replace-node/replacenodeflow.png)

* Esta ação pode não ser necessária com base na condição física do hardware.

## <a name="review-alert-information"></a>Reveja as informações de alerta

Se um nó de unidade de escala estiver em baixo, receberá os alertas críticos seguintes:

- Nó não ligado ao controlador de rede
- Nó inacessível para colocação de máquina virtual
- Nó de unidade de escala está offline

![Lista de alertas de unidade de escala para baixo](media/azure-stack-replace-node/nodedownalerts.png)

Se abrir o **nó de unidade de escala é offline** alerta, a descrição do alerta contém o nó de unidade de escala que está inacessível. Também poderá receber alertas adicionais na solução de monitorização específicos de OEM que está a ser executado no anfitrião de ciclo de vida do hardware.

![Detalhes de alerta offline do nó](media/azure-stack-replace-node/nodeoffline.png)

## <a name="scale-unit-node-replacement-process"></a>Processo de substituição de nó de unidade de escala

Os seguintes passos são fornecidos como uma visão geral sobre o processo de substituição de nó de unidade de escala. Consulte a documentação de FRU do seu fornecedor de hardware de OEM para obter passos detalhados que são específicas para o seu sistema. Não siga estes passos sem referir-se a sua documentação fornecida pelo OEM.

1. Utilize o [drenar](azure-stack-node-actions.md#scale-unit-node-actions) ação para colocar o nó de unidade de escala em modo de manutenção. Esta ação pode não ser necessária com base na condição física do hardware.

   > [!NOTE]
   > De qualquer forma, apenas um nó pode ser drenado e está desligada, ao mesmo tempo, sem quebrar o S2D (espaços de armazenamento direto).

2. Se o nó ainda está ligado, utilize o [desligar](azure-stack-node-actions.md#scale-unit-node-actions) ação. Esta ação pode não ser necessária com base na condição física do hardware.
 
   > [!NOTE]
   > No caso pouco provável que a ação de desligar não funciona, utilize a interface de web do controlador (BMC) de gestão da placa base em vez disso.

1. Substitua o computador físico. Normalmente, isso é feito pelo seu fornecedor de hardware de OEM.
2. Utilize o [reparação](azure-stack-node-actions.md#scale-unit-node-actions) ação para adicionar o novo computador físico para a unidade de escala.
3. Utilizar o ponto final com privilégios para [verificar o estado de reparação de disco virtual](azure-stack-replace-disk.md#check-the-status-of-virtual-disk-repair). Com novas unidades de dados, uma tarefa de reparação de armazenamento completa pode demorar várias horas, dependendo da carga de sistema e consumidos espaço.
4. Após concluir a ação de reparação, confirme que todos os alertas ativos foram fechados automaticamente.

## <a name="next-steps"></a>Passos Seguintes

- Para obter informações sobre como substituir um disco físico fontes, consulte [substituir um disco](azure-stack-replace-disk.md). 
- Para obter informações sobre a substituição de um componente de hardware não fontes, consulte [substituir componentes de hardware](azure-stack-replace-component.md).
