---
title: "Compreender a utilização de máquina virtual do Azure | Microsoft Docs"
description: "Compreender os detalhes de utilização de máquinas virtuais"
services: virtual-machines
documentationcenter: 
author: mmccrory
manager: jeconnoc
editor: 
tags: azure-virtual-machine
ms.assetid: 
ms.service: 
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: vm
ms.workload: infrastructure-services
ms.date: 12/04/2017
ms.author: memccror
ms.openlocfilehash: c87c4256aa193a4971b75c3230d1996c2efdc352
ms.sourcegitcommit: 7f1ce8be5367d492f4c8bb889ad50a99d85d9a89
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2017
---
# <a name="understanding-azure-virtual-machine-usage"></a>Compreender a utilização de máquina virtual do Azure
Ao analisar os dados de utilização do Azure, podem ser adquiridos insights consumo poderosas – insights que podem ativar melhor custo alocação em toda a organização e de gestão. Este documento fornece uma descrição profunda para os detalhes de consumo de computação do Azure. Para obter mais detalhes sobre a utilização do Azure geral, navegue para [compreender a fatura](/billing/billing-understand-your-bill.md).

## <a name="download-your-usage-details"></a>Transferir os seus detalhes de utilização
Para começar, [transferir os seus detalhes de utilização](/billing/billing-download-azure-invoice-daily-usage-date#download-usage-from-the-account-center-csv.md). A tabela abaixo fornece os valores de definição e exemplo de utilização de máquinas virtuais implementadas através do Azure Resource Manager. Este documento não contém informações detalhadas sobre as VMs implementadas através do nosso modelo clássico.


| Campos             | Significado                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Valores de exemplo                                                                                                                                                                                                                                                                                                                                                   |
|--------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Data de Utilização         | A data em que o recurso foi utilizado.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |  “11/23/2017”                                                                                                                                                                                                                                                                                                                                                     |
| ID do Medidor           | Identifica o serviço de nível superior para que esta utilização pertence.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | "Virtual Machines"                                                                                                                                                                                                                                                                                                                                               |
| Subcategoria do Medidor | O identificador do medidor faturado. <ul><li>Para a utilização da hora de computação, é um medidor para cada tamanho da VM + SO (Windows, não Windows) + região.</li><li>Para utilização de software Premium, há um medidor para cada tipo de software. A maioria das imagens de software premium têm medidores diferentes para cada tamanho core. Para obter mais informações, visite o [página de preços de computação.](https://azure.microsoft.com/pricing/details/virtual-machines/)</li></ul>                                                                                                                                                                                                                                                                                                                                         | "2005544f-659d-49c9-9094-8e0aea1be3a5"                                                                                                                                                                                                                                                                                                                           |
| Nome do Medidor         | Esta ação é específica para cada serviço no Azure. Para computação, é sempre "Horas de computação".                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | "Horas de computação"                                                                                                                                                                                                                                                                                                                                                  |
| Região do Medidor       | Identifica a localização do datacenter para determinados serviços cujo preço é definido com base na localização do datacenter.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |  "JA Leste"                                                                                                                                                                                                                                                                                                                                                       |
| Unidade               | Identifica a unidade que o serviço é cobrado. Recursos de computação são faturados por hora.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | "Horas"                                                                                                                                                                                                                                                                                                                                                          |
| Consumido           | A quantidade de recursos que consumiu para esse dia. Para computação, iremos faturar-para cada um dos minutos que a VM foi executada para uma determinada hora (até 6 casas decimais de precisão).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |    “1”, “0.5”                                                                                                                                                                                                                                                                                                                                                    |
| Localização do Recurso  | Identifica o datacenter onde o recurso está em execução.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | "JA Leste"                                                                                                                                                                                                                                                                                                                                                        |
| Serviço Consumido   | O serviço de plataforma Azure que utilizou.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | "Microsoft. Compute"                                                                                                                                                                                                                                                                                                                                              |
| Grupo de Recursos     | O grupo de recursos no qual o recurso implementado está em execução. Para obter mais informações, consulte [descrição geral do Azure Resource Manager.](/azure-resource-manager/resource-group-overview.md)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |    "MyRG"                                                                                                                                                                                                                                                                                                                                                        |
| ID da Instância        | O identificador do recurso. O identificador contém o nome que especificou para o recurso quando o criou. Para VMs, o ID de instância irá conter o SubscriptionId, ResourceGroupName e VMName (ou conjunto de dimensionamento de nome para a utilização do conjunto de dimensionamento).                                                                                                                                                                                                                                                                                                                                                                                                                    | "/ subscrições/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx / resourceGroups/MyRG/providers/Microsoft.Compute/virtualMachines/MyVM1"<br><br>ou<br><br>"/ subscrições/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx / resourceGroups/MyRG/providers/Microsoft.Compute/virtualMachineScaleSets/MyVMSS1"                                                                                           |
| Etiquetas               | Tag que atribuir ao recurso. Utilize etiquetas para registos de faturação do grupo. Saiba como [marcar as máquinas virtuais.](tag.md) Isto só está disponível para VMs do Gestor de recursos.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | "{"myDepartment":"RD","myUser":"myName"}"                                                                                                                                                                                                                                                                                                                        |
| Informações Adicionais    | Metadados específicos do serviço. Para VMs, vamos preencher o seguinte no campo de informações adicionais: <ul><li>Imagem de específicos do tipo de imagem que tenha sido executada. Localize a lista completa das cadeias suportadas abaixo em tipos de imagem.</li><li>O tipo de serviço: o tamanho que implementou.</li><li>VMName: nome da sua VM. Isto apenas é preenchido para o conjunto de dimensionamento de VMs. Se precisar de VMs de definir o nome de VM para dimensionamento, que pode encontrar na cadeia de ID de instância acima.</li><li>UsageType: Isto especifica o tipo de utilização representa.<ul><li>ComputeHR é a utilização de hora de computação para a VM subjacente, como Standard_D1_v2.</li><li>ComputeHR_SW é a taxa de software premium se a VM estiver a utilizar o software de premium, como o servidor R da Microsoft.</li></ul></li></ul>    | Máquinas virtuais {"ImageType": "Canónica", "ServiceType": "Standard_DS1_v2", "VMName": "", "UsageType": "ComputeHR"}<br><br>Conjuntos de dimensionamento de máquina virtual {"ImageType": "Canónica", "ServiceType": "Standard_DS1_v2", "VMName": "myVM1", "UsageType": "ComputeHR"}<br><br>Premium Software {"ImageType": "","ServiceType": "Standard_DS1_v2", "VMName": "", "UsageType": "ComputeHR_SW"} |

## <a name="image-type"></a>Tipo de Imagem
Para algumas imagens na galeria do Azure, o tipo de imagem é preenchido no campo de informações adicionais. Isto permite aos utilizadores compreender e controlar tem implementado na sua máquina Virtual. Os valores que são povoados neste campo com base na imagem que implementou são os seguintes:
  - BitRock 
  - Canónico 
  - FreeBSD 
  - Abra lógica 
  - Oracle 
  - SLES para SAP 
  - Pré-visualização do SQL Server 14 no Windows Server 2012 R2 Preview 
  - SUSE
  - SUSE Premium
  - StorSimple Cloud Appliance 
  - Red Hat
  - Red Hat para aplicações empresariais SAP     
  - Red Hat para SAP HANA 
  - Cliente Windows BYOL 
  - Windows Server BYOL 
  - Pré-visualização do Windows Server 

## <a name="service-type"></a>Tipo de Serviço
O campo de tipo de serviço no campo de informações adicionais corresponde ao tamanho da VM exato que implementou. Armazenamento Premium VMs (baseadas em SSD) e não premium VMs (baseado em HDD) têm um preço da mesma. Se implementar um tamanho baseadas em SSD, como padrão\_DS2\_v2, consulte o tamanho não SSD (' padrão\_D2\_v2 VM') na coluna subcategoria de medidor e o tamanho de SSD (' padrão\_DS2\_ v2') no campo de informações adicionais.

## <a name="region-names"></a>Nomes de região
O nome de região preenchido no campo localização do recurso os detalhes de utilização varia do nome da região utilizado no Gestor de recursos do Azure. Segue-se um mapeamento entre os valores de região:

|    **Nome de região de Gestor de recursos**       |    **Localização do recurso nos detalhes de utilização**    |
|--------------------------|------------------------------------------|
|    lesteaustrália         |    AU leste                               |
|    sudesteaustrália    |    AU Sudeste                          |
|    sulbrasil           |    Sul do Brasil                              |
|    CanadaCentral         |    Centro de AC                            |
|    CanadaEast            |    CA leste                               |
|    CentralIndia          |    Índia Central                            |
|    euacentral             |    EUA Central                            |
|    chinaeast             |    Leste da China                            |
|    chinanorth            |    China Norte                           |
|    ásiaoriental              |    Ásia Oriental                             |
|    eualeste                |    EUA Leste                               |
|    eualeste2               |    EUA Leste 2                             |
|    GermanyCentral        |    Alemanha Central                            |
|    GermanyNortheast      |    Alemanha Nordeste                          |
|    lestejapão             |    Este JA                               |
|    oestejapão             |    Oeste JA                               |
|    KoreaCentral          |    KR Central                            |
|    KoreaSouth            |    KR Sul                              |
|    euacentronorte        |    EUA Centro-Norte                      |
|    europanorte           |    Europa do Norte                          |
|    euacentrosul        |    EUA Centro-Sul                      |
|    sudesteasiático         |    Sudeste Asiático                        |
|    SouthIndia            |    Sul da Índia                              |
|    UKNorth               |    E.u. a norte                              |
|    uksouth               |    Reino Unido Sul                              |
|    UKSouth2              |    Sul do Reino Unido 2                            |
|    ukwest                |    Reino Unido Oeste                               |
|    USDoDCentral          |    US DoD Centro                        |
|    USDoDEast             |    US DoD Leste                           |
|    USGovArizona          |    Governo dos E.u.a. Arizona                         |
|    usgoviowa             |    USGov Iowa                            |
|    USGovTexas            |    Governo dos E.u.a. Texas                           |
|    usgovvirginia         |    USGov Virginia                        |
|    westcentralus         |    E.U.A. centro oeste                       |
|    europaocidental            |    Europa Ocidental                           |
|    WestIndia             |    Índia Ocidental                               |
|    euaoeste                |    EUA Oeste                               |
|    westus2               |    E.u. a oeste 2                             |


## <a name="virtual-machine-usage-faq"></a>FAQ acerca da utilização de máquina virtual
### <a name="what-resources-are-charged-when-deploying-a-vm"></a>Os recursos que são-lhe cobrados quando implementar uma VM?    
VMs adquirir os custos para a própria VM, qualquer software premium em execução numa VM, o disco de account\managed de armazenamento associada a VM e transfere a largura de banda de rede da VM.
### <a name="how-can-i-tell-if-a-vm-is-using-azure-hybrid-benefit-in-the-usage-csv"></a>Como posso saber se uma VM está a utilizar o benefício de híbrida do Azure no CSV utilização?
Se implementar utilizando o [Azure híbrida benefício](https://azure.microsoft.com/pricing/hybrid-benefit/), são-lhe cobrados a taxa de VM não-Windows, uma vez que colocarem sua própria licença para a nuvem. A fatura poder diferenciar que as VMs do Gestor de recursos estão a ser executado Azure híbrida benefício porque têm um "Windows\_servidor BYOL" ou "Windows\_cliente BYOL" na coluna ImageType.
### <a name="how-are-basic-vs-standard-vm-types-differentiated-in-the-usage-csv"></a>Como são vs básicas. Tipos de VM padrão diferenciadas no CSV utilização?
São disponibilizadas básico e padrão de uma série de VMs. Se implementar uma VM básico, na categoria de Sub medidor, tem a cadeia "Basic". Se implementar uma VM de série A padrão, em seguida, o tamanho da VM é apresentado como "A1 VM" uma vez que o padrão é a predefinição. Para saber mais sobre as diferenças entre básico e padrão, consulte o [página de preços](https://azure.microsoft.com/pricing/details/virtual-machines/).
### <a name="what-are-extrasmall-small-medium-large-and-extralarge-sizes"></a>Quais são os tamanhos de extra pequena, pequena, média, grande e extra grande?
ExtraSmall - ExtraLarge são os nomes de legado para Standard\_A0 – Standard\_A4. Nos registos de utilização VM clássicos, poderá ver esta Convenção utilizada se tiver implementado estes tamanhos.
### <a name="what-is-the-difference-between-meter-region-and-resource-location"></a>O que é a diferença entre a região de medição e localização do recurso?
A região de medidor está associada com a medição. Para alguns serviços do Azure que utilizam um preço para todas as regiões, o campo de região de medidor pode estar em branco. No entanto, uma vez que as VMs têm dedicados os preços por região para máquinas virtuais, este campo é preenchido. Da mesma forma, a localização de recursos para máquinas virtuais é a localização onde a implementação da VM. A região do Azure em ambos os campos são os mesmos, embora possam ter uma convenção de cadeia diferente para o nome de região.
### <a name="why-is-the-imagetype-value-blank-in-the-additional-info-field"></a>Por que motivo é o valor de ImageType em branco no campo de informações adicionais?
O campo de ImageType for preenchido apenas para um subconjunto de imagens. Não foi implementada uma das imagens acima, o ImageType estiver em branco.
### <a name="why-is-the-vmname-blank-in-the-additional-info"></a>Por que motivo o VMName está em branco nas informações adicionais?
O VMName apenas é preenchido no campo de informações adicionais para as VMs num conjunto de dimensionamento. O campo de InstanceID contém o nome da VM para o conjunto de dimensionamento não de VMs.
### <a name="what-does-computehr-mean-in-the-usagetype-field-in-the-additional-info"></a>O que significa ComputeHR no campo UsageType as informações adicionais?
ComputeHR representa a hora de computação de mensagens em fila que representa o evento de utilização para o custo de infrasturcture subjacente. Se o UsageType ComputeHR\_SW, o evento de utilização representa a taxa de software premium para a VM.
### <a name="how-do-i-know-if-i-am-charged-for-premium-software"></a>Como saber se estou a cobrado para premium software?
Quando a explorar a imagem de VM melhor se adequa às suas necessidades, lembre-se de que conheça a [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/compute). A imagem tem a taxa de plano de software. Se vir "Livre" para a velocidade, há sem custos adicionais para o software. 
### <a name="what-is-the-difference-between-microsoftclassiccompute-and-microsoftcompute-in-the-consumed-service"></a>O que é a diferença entre a Microsoft. classiccompute e Microsoft. Compute no serviço consumido?
Microsoft. classiccompute representa os recursos clássicos implementados através do Gestor de serviços do Azure. Se implementar através do Resource Manager, Microsoft. Compute é preenchido no serviço foram consumido. Saiba mais sobre o [modelos de implementação do Azure](/azure-resource-manager/resource-manager-deployment-model.md).
### <a name="why-is-the-instanceid-field-blank-for-my-virtual-machine-usage"></a>Por que motivo o campo de InstanceID está em branco para a utilização a minha máquina Virtual?
Se implementar através do modelo de implementação clássica, a cadeia de InstanceID não está disponível.
### <a name="why-are-the-tags-for-my-vms-not-flowing-to-the-usage-details"></a>Por que razão são as etiquetas para minhas VMs que não são enviados para os detalhes de utilização?
Etiquetas apenas flui que a utilização de CSV para as VMs do Gestor de recursos apenas. Os sinalizadores de recurso clássico não estão disponíveis nos detalhes de utilização.
### <a name="how-can-the-consumed-quantity-be-more-than-24-hours-one-day"></a>Como é que a quantidade foram consumida pode ser mais de 24 horas um dia?
No modelo clássico, a faturação de recursos é agregada ao nível do serviço em nuvem. Se tiver mais do que uma VM num serviço em nuvem que utiliza a medição de faturação mesma, a utilização é agregada em conjunto. VMs implementadas através do Gestor de recursos são faturadas ao nível da VM, pelo que não será aplicada esta agregação.
### <a name="why-is-pricing-not-available-for-dsfsgsls-sizes-on-the-pricing-page"></a>Por isso é de preço não disponível para tamanhos de DS/FS/GS/LS na página de preços?
Armazenamento Premium VMs com capacidade faturadas ao mesmo preço como armazenamento premium não compatíveis com VMs. Apenas os custos de armazenamento diferem. Visite o [página de preços de armazenamento](https://azure.microsoft.com/pricing/details/storage/unmanaged-disks/) para obter mais informações.

## <a name="next-steps"></a>Passos seguintes
Para saber mais sobre os detalhes de utilização, consulte o artigo [compreender a fatura do Microsoft Azure.](/billing/billing-understand-your-bill.md)

