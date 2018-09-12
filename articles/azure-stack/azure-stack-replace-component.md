---
title: Substituir um componente de hardware num nó de unidade de escala do Azure Stack | Documentos da Microsoft
description: Saiba como substituir um componente de hardware num sistema integrado do Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2018
ms.author: mabrigg
ms.openlocfilehash: df9470813f3f9c3bff58882879c06e7b7b0fc15b
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44379609"
---
# <a name="replace-a-hardware-component-on-an-azure-stack-scale-unit-node"></a>Substituir um componente de hardware num nó de unidade de escala do Azure Stack

*Aplica-se a: sistemas integrados do Azure Stack*

Este artigo descreve o processo geral para substituir componentes de hardware que são não fontes. Substituição real passos variam com base no seu fornecedor de hardware do fabricante de equipamento original (OEM). Consulte a documentação do fornecedor campo unidade substituível em (FRU) para obter passos detalhados que são específicas para o seu sistema integrado do Azure Stack.

Componentes não fontes incluem o seguinte:

- CPU *
- Memória *
- O controlador de gestão de placa-mãe/da placa base (BMC) / vídeo cartão
- Adaptador de barramento do disco controlador/anfitrião (HBA) / backplane
- Adaptador de rede (NIC)
- Disco do sistema operativo *
- Unidades de dados (unidades que não suportam a troca de acesso frequente, por exemplo PCI-e suplementos cartões) *

* Estes componentes podem suportar a troca de acesso frequente, mas podem variar de acordo com a implementação de fornecedor. Consulte a documentação de FRU do seu fornecedor de OEM para obter passos detalhados.

O diagrama de fluxo que se segue mostra o processo FRU geral para substituir um componente de hardware não fontes.

![Diagrama de fluxo que mostra o fluxo de substituição de componente](media/azure-stack-replace-component/replacecomponentflow.PNG)

* Esta ação pode não ser necessária com base na condição física do hardware.

* * Se o seu fornecedor de hardware de OEM efetua a substituição de componente e atualizações de que firmware pode variar com base no seu contrato de suporte.

## <a name="review-alert-information"></a>Reveja as informações de alerta

O estado de funcionamento do Azure Stack e o sistema de monitorização controlam o estado de funcionamento de adaptadores de rede e unidades de dados controladas pelo espaços de armazenamento direto. Não controla a outros componentes de hardware. Para todos os outros componentes de hardware, os alertas são gerados no hardware específicos do fornecedor de solução que é executado no anfitrião de ciclo de vida do hardware de monitorização.  

## <a name="component-replacement-process"></a>Processo de substituição de componente

Os passos seguintes fornecem uma visão geral sobre o processo de substituição de componente. Não siga estes passos sem referir-se a sua documentação FRU fornecidos pelo OEM.

1. Utilize o [drenar](azure-stack-node-actions.md#scale-unit-node-actions) ação para colocar o nó de unidade de escala em modo de manutenção. Esta ação pode não ser necessária com base na condição física do hardware.

   > [!NOTE]
   > De qualquer forma, apenas um nó pode ser drenado e está desligada, ao mesmo tempo, sem quebrar o S2D (espaços de armazenamento direto).

2. Depois do nó de unidade de escala está no modo de manutenção, utilize o [desligar](azure-stack-node-actions.md#scale-unit-node-actions) ação. Esta ação pode não ser necessária com base na condição física do hardware.

   > [!NOTE]
   > No caso pouco provável que a ação de desligar não funciona, utilize a interface de web do controlador (BMC) de gestão da placa base em vez disso.

3. Substitua o componente de hardware danificado. Se o seu fornecedor de hardware de OEM efetua a substituição de componente pode variar com base no seu contrato de suporte.  
4. Atualize o firmware. Siga o processo de atualização de firmware específicas do fornecedor com o anfitrião de ciclo de vida do hardware para se certificar de que o componente de hardware substituído tem o firmware aprovado nível aplicado. Se o seu fornecedor de hardware de OEM executa este passo pode variar com base no seu contrato de suporte.  
5. Utilize o [reparação](azure-stack-node-actions.md#scale-unit-node-actions) ação para repor o nó de unidade de escala a unidade de escala.
6. Utilizar o ponto final com privilégios para [verificar o estado de reparação de disco virtual](azure-stack-replace-disk.md#check-the-status-of-virtual-disk-repair). Com novas unidades de dados, uma tarefa de reparação de armazenamento completa pode demorar várias horas, dependendo da carga de sistema e consumidos espaço.
7. Após concluir a ação de reparação, confirme que todos os alertas ativos foram fechados automaticamente.

## <a name="next-steps"></a>Passos Seguintes

- Para obter informações sobre como substituir um disco físico fontes, consulte [substituir um disco](azure-stack-replace-disk.md).
- Para obter informações sobre a substituição de um nó físico, consulte [substituir um nó de unidade de escala](azure-stack-replace-node.md).
