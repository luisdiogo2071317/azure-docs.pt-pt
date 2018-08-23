---
title: Ações de nó de unidade de dimensionamento no Azure Stack | Documentos da Microsoft
description: Saiba como ver o estado do nó e tire partido do poder desligar, drenagem e retomar a ações de nó num sistema integrado do Azure Stack.
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
ms.date: 08/14/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: 1f59f2ce6e3bf8d34ce225aa93da76ad523775e0
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/15/2018
ms.locfileid: "42055789"
---
# <a name="scale-unit-node-actions-in-azure-stack"></a>Ações de nó de unidade de dimensionamento no Azure Stack

*Aplica-se a: sistemas integrados do Azure Stack*

Este artigo descreve como visualizar o estado de uma unidade de escala e de seus nós associados e como utilizar as ações de nó disponível. Ações de nó incluem power no poder de fora, drenagem, retomam e reparem. Normalmente, utiliza estas ações de nó durante a substituição de campo de partes ou para cenários de recuperação de nó.

> [!Important]  
> Todas as ações de nó descritas neste artigo devem apenas destino um nó por vez.


## <a name="view-the-status-of-a-scale-unit-and-its-nodes"></a>Ver o estado de uma unidade de escala e respetivos nós

No portal do administrador, pode ver facilmente o estado de uma unidade de escala e de seus nós associados.

Para ver o estado de uma unidade de escala:

1. Sobre o **gestão da região** mosaico, selecione a região.
2. No lado esquerdo, sob **recursos de infraestrutura**, selecione **unidades de escala**.
3. Nos resultados, selecione a unidade de escala.
 
Aqui, pode ver as seguintes informações:

- nome da região. O nome da região é referenciado com **-localização** no módulo do PowerShell.
- tipo de sistema
- total de núcleos de lógica
- total de memória
- a lista de nós individuais e o respetivo estado; qualquer um dos **em execução** ou **parado**.

![Mosaico de unidade de escala que mostra o estado de execução para cada nó](media/azure-stack-node-actions/ScaleUnitStatus.PNG)

## <a name="view-information-about-a-scale-unit-node"></a>Ver informações sobre um nó de unidade de escala

Se selecionar um nó individual, pode ver as seguintes informações:

- nome da região
- modelo de servidor
- Endereço IP do controlador de gestão da placa base (BMC)
- Estado operacional
- Número total de núcleos
- quantidade total de memória
 
![Mosaico de unidade de escala que mostra o estado de execução para cada nó](media/azure-stack-node-actions/NodeActions.PNG)

Também pode executar ações de nó de unidade de escala a partir daqui.

## <a name="scale-unit-node-actions"></a>Ações de nó de unidade de escala

Quando Exibe informações sobre um nó de unidade de escala, também pode realizar ações de nó, tais como:

- Drenagem e retomar
- Reparar

Estado operacional do nó determina quais as opções estão disponíveis.

### <a name="power-off"></a>Desligar

O **desligar** ação se desligar o nó. É o mesmo como se pressionar o botão de energia. Ele faz **não** envia um sinal de encerramento para o sistema operativo. Para planeada desligar operações, certifique-se de que drenar um nó de unidade de escala em primeiro lugar.

Esta ação é normalmente utilizada quando um nó está em estado suspenso e já não responde a pedidos.

> [!Important] 
> Esta funcionalidade só está disponível através do PowerShell. Estará disponível no portal de administrador do Azure Stack novamente mais tarde.


Para executar a ação através do PowerShell de Desativação:

````PowerShell
  Stop-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
```` 

No caso pouco provável que a ação de desligar não funciona, utilize a interface de web BMC em vez disso.

### <a name="power-on"></a>Ligar

O **ligar** ação ativa o nó. É o mesmo como se pressionar o botão de energia. 

> [!Important] 
> Esta funcionalidade só está disponível através do PowerShell. Estará disponível no portal de administrador do Azure Stack novamente mais tarde.

Para executar a potência em ação através do PowerShell:

````PowerShell
  Start-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
````

No caso pouco provável que o poder na ação não funciona, utilize a interface de web BMC em vez disso.

### <a name="drain"></a>Drenar

O **drenar** ação evacua todas as cargas de trabalho ativas por distribuí-los entre os nós restantes nessa unidade de escala específico.

Esta ação é normalmente utilizada durante a substituição de campo de partes, como a substituição de um nó completo.

> [!IMPORTANT]  
> Certifique-se de que drenar um nó apenas durante uma janela de manutenção planeada, onde os utilizadores foram notificadas sobre. Em algumas condições, cargas de trabalho ativas podem assistir a interrupções.

Para executar a ação de drenagem através do PowerShell:

  ````PowerShell
  Disable-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
  ````

### <a name="resume"></a>Retomar

O **retomar** ação de retoma de um nó drained e marca-o Active Directory para colocação de carga de trabalho. Cargas de trabalho anteriores que estavam em execução no nó não a reativação pós-falha. (Se drenar um nó e, em seguida, desligar, ao energia o nó de volta no, não está marcado como ativo para colocação de carga de trabalho. Quando estiver pronto, tem de utilizar a ação de retoma para marcar o nó como o Active Directory.)

Para executar a ação de retoma através do PowerShell:

  ````PowerShell
  Enable-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
  ````

### <a name="repair"></a>Reparar

O **reparação** ação repara um nó. Utilize apenas para qualquer um dos seguintes cenários:

- Substituição de nó completo (com ou sem novos discos de dados)
- Após a falha de componente de hardware e de substituição (se aconselhado na documentação (FRU) de unidade substituível em campo).

> [!IMPORTANT]  
> Consulte a documentação de FRU do seu fornecedor de hardware de OEM para obter os passos exatos quando é necessário substituir um nó ou de componentes de hardware individuais. A documentação de FRU irá especificar se tiver de executar a ação de reparação depois de substituir um componente de hardware.  

Quando executa a ação de reparação, tem de especificar o endereço IP do BMC. 

Para executar a ação de reparação através do PowerShell:

  ````PowerShell
  Repair-AzsScaleUnitNode -Location <RegionName> -Name <NodeName> -BMCIPAddress <BMCIPAddress>
  ````

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre o módulo de administrador de recursos de infraestrutura do Azure Stack, veja [Azs.Fabric.Admin](https://docs.microsoft.com/powershell/module/azs.fabric.admin/?view=azurestackps-1.4.0).