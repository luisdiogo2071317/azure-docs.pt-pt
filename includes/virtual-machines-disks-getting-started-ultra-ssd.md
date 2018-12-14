---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 09/24/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: ec8fa6c06dff0091627a800c895d45fd3b0e778e
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2018
ms.locfileid: "53381630"
---
# <a name="enabling-azure-ultra-ssds"></a>Ativar o SSDs Ultra do Azure

SSD Ultra do Azure oferece alto débito e armazenamento de disco de baixa latência consistente IOPS elevado para VMs IaaS do Azure. Esta nova oferta fornece a parte superior do desempenho de linha nos níveis de disponibilidade mesmo como nossas ofertas de discos existentes. Vantagens adicionais de Ultra SSD incluem a capacidade de alterar dinamicamente o desempenho do disco, juntamente com as suas cargas de trabalho sem a necessidade de reiniciar as máquinas virtuais. Ultra SSD é adequada para cargas de trabalho com uso intensivo de dados, como SAP HANA, bases de dados de escalão superior e cargas de trabalho pesado de transação.

Atualmente, o Ultra SSds estão em pré-visualização e deverá [inscrever](https://aka.ms/UltraSSDPreviewSignUp) na pré-visualização para poder aceder aos mesmos.

Quando for aprovado, execute um dos seguintes comandos para determinar exatamente em qual zona na região E.U.A. Leste 2 para implementar o Ultra SSD para:

PowerShell: `Get-AzureRmComputeResourceSku | where {$_.ResourceType -eq "disks" -and $_.Name -eq "UltraSSD_LRS" }`

CLI: `az vm list-skus --resource-type disks --query “[?name==’UltraSSD_LRS’]”`

A resposta será semelhante ao formulário abaixo, onde X é a zona a utilizar para implementar na região E.U.A. Leste 2. X pode ser 1, 2 ou 3.

|ResourceType  |Nome  |Localização  |Zonas  |Restrição  |Capacidade  |Valor  |
|---------|---------|---------|---------|---------|---------|---------|
|Discos     |UltraSSD_LRS         |eualeste2         |X         |         |         |         |

Se não houve resposta do comando, isso significa que o registo para a funcionalidade é ainda pendente ou não aprovado ainda.

Agora que sabe exatamente em qual zona para implementar, siga os passos de implementação neste artigo para obter o seu primeiro VMs implementadas com discos de Ultra SSD.

## <a name="deploying-an-ultra-ssd"></a>Implementar um SSD Ultra

Primeiro, determine o tamanho da VM para implementar. Como parte desta pré-visualização, apenas as famílias DsV3 e EsV3 VM são suportadas. Consulte a segunda tabela sobre isso [blogue](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/) para obter mais detalhes sobre estes tamanhos VM.
Consulte também o exemplo [criar uma VM com vários discos de Ultra SSD](https://aka.ms/UltraSSDTemplate), que mostra como criar uma VM com vários discos de Ultra SSD.

As seguintes descrevem as alterações de modelo do Resource Manager de novo/modificada: **apiVersion** para `Microsoft.Compute/virtualMachines` e `Microsoft.Compute/Disks` tem de ser definido como `2018-06-01` (ou posterior).

Especifique o disco Sku UltraSSD_LRS, capacidade de disco, IOPS e débito em MBps para criar um disco Ultra SSD. Segue-se um exemplo que cria um disco com 1.024 GiB (GiB = 2 ^ 30 Bytes), 80 000 IOPS e 1200 MBps (MBps = 10 ^ 6 Bytes por segundo):

```json
"properties": {  
    "creationData": {  
    "createOption": "Empty"  
},  
"diskSizeGB": 1024,  
"diskIOPSReadWrite": 80000,  
"diskMBpsReadWrite": 1200,  
}
```

Adicionar uma capacidade adicional nas propriedades da VM para indicar a sua capacidade de SSD Ultra (consulte a [exemplo](https://aka.ms/UltraSSDTemplate) para o modelo do Resource Manager completo):

```json
{
    "apiVersion": "2018-06-01",
    "type": "Microsoft.Compute/virtualMachines",
    "properties": {
                    "hardwareProfile": {},
                    "additionalCapabilities" : {
                                    "ultraSSDEnabled" : "true"
                    },
                    "osProfile": {},
                    "storageProfile": {},
                    "networkProfile": {}
    }
}
```

Assim que a VM está aprovisionada, pode particionar e formatar os discos de dados e configurá-los para cargas de trabalho.

## <a name="additional-ultra-ssd-scenarios"></a>Cenários de Ultra SSD adicionais

- Durante a criação de VM, Ultra SSDs podem ser criadas implicitamente também. No entanto, estes discos irão receber um valor predefinido para IOPS (500) e o débito (8 MiB/s).
- SSDs Ultra adicionais podem ser anexados a VMs compatíveis de Ultra SSD.
- Ultra SSD oferece suporte a ajustar os atributos de desempenho de disco (IOPS e débito) em tempo de execução sem desligar o disco da máquina virtual. Assim que tiver sido emitida uma operação de redimensionamento de desempenho de disco num disco, pode demorar até uma hora para que a alteração, na verdade, entre em vigor.
- Aumentar a capacidade de disco necessitam de uma máquina virtual para ser desalocados.

## <a name="next-steps"></a>Passos Seguintes

Se quiser experimentar o novo tipo de disco e ainda não se inscreveu para a pré-visualização, ainda, [pedir acesso através desta pesquisa](https://aka.ms/UltraSSDPreviewSignUp).
