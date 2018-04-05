---
title: Evento de criar conjunto do Azure Batch | Microsoft Docs
description: Referência para o conjunto do Batch criar eventos.
services: batch
author: dlepow
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: danlep
ms.openlocfilehash: bf7dfc2600c3d94faeb8d03561f6f2b30a0ee2d2
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="pool-create-event"></a>Evento de criação de conjunto

 Este evento é emitido depois de criar um conjunto. O conteúdo do registo irá expor informações gerais sobre o conjunto. Tenha em atenção que se o tamanho de destino do conjunto é maior que 0 nós de computação, um evento de início do conjunto redimensionamento seguirá imediatamente depois deste evento.

 O exemplo seguinte mostra o corpo de um conjunto de criar evento para um conjunto criado utilizando a propriedade CloudServiceConfiguration.

```
{
    "id": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "3",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicated": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoScale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

|Elemento|Tipo|Notas|
|-------------|----------|-----------|
|ID|Cadeia|O id do conjunto.|
|displayName|Cadeia|O nome a apresentar do conjunto.|
|vmSize|Cadeia|O tamanho das máquinas virtuais no conjunto. Todas as máquinas virtuais num conjunto são o mesmo tamanho. <br/><br/> Para obter informações acerca dos tamanhos disponíveis de máquinas virtuais para serviços em nuvem agrupamentos (agrupamentos criados com cloudServiceConfiguration), consulte [tamanhos para serviços em nuvem](http://azure.microsoft.com/documentation/articles/cloud-services-sizes-specs/). Batch suporta todos os tamanhos de VM de serviços em nuvem, exceto `ExtraSmall`.<br/><br/> Para obter informações sobre a VM disponível tamanhos de agrupamentos de utilização de imagens no Marketplace de máquinas virtuais (agrupamentos criados com virtualMachineConfiguration) consulte [tamanhos das Virtual Machines](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-sizes/) (Linux) ou [tamanhos das Virtual Máquinas](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) (Windows). O Batch suporta todos os tamanhos de VM do Azure, exceto `STANDARD_A0` e os do armazenamento premium (séries `STANDARD_GS`, `STANDARD_DS` e `STANDARD_DSV2`).|
|[cloudServiceConfiguration](#bk_csconf)|Tipo complexo|A configuração do serviço em nuvem para o conjunto.|
|[virtualMachineConfiguration](#bk_vmconf)|Tipo complexo|A configuração de máquina virtual para o conjunto.|
|[networkConfiguration](#bk_netconf)|Tipo complexo|A configuração de rede para o conjunto.|
|resizeTimeout|Hora|O tempo limite para alocação de nós de computação para o conjunto especificado para a última operação de redimensionamento no conjunto.  (O dimensionamento inicial quando o conjunto for criado conta como um redimensionamento.)|
|targetDedicated|Int32|O número de nós de computação que são pedidos para o conjunto.|
|enableAutoScale|Booleano|Especifica se o tamanho do conjunto ajusta automaticamente ao longo do tempo.|
|enableInterNodeCommunication|Booleano|Especifica se o agrupamento está configurado para comunicação direta entre nós.|
|isAutoPool|Booleano|Speficies se o conjunto foi criado através do mecanismo de AutoPool de uma tarefa.|
|maxTasksPerNode|Int32|O número máximo de tarefas que podem ser executados em simultâneo num único nó de computação no conjunto.|
|vmFillType|Cadeia|Define a forma como o serviço Batch distribui as tarefes entre nós de computação no conjunto. Os valores válidos são distribuídos ou pacote.|

###  <a name="bk_csconf"></a> cloudServiceConfiguration

|Nome do elemento|Tipo|Notas|
|------------------|----------|-----------|
|osFamily|Cadeia|A família de SO convidado do Azure para ser instalada nas máquinas virtuais no conjunto.<br /><br /> Os valores possíveis são:<br /><br /> **2** – 2 família do SO, equivalente ao Windows Server 2008 R2 SP1.<br /><br /> **3** – 3 família do SO, equivalente para o Windows Server 2012.<br /><br /> **4** – 4 família do SO, equivalente ao Windows Server 2012 R2.<br /><br /> Para obter mais informações, consulte [Azure de SO convidado](https://azure.microsoft.com/documentation/articles/cloud-services-guestos-update-matrix/#releases).|
|targetOSVersion|Cadeia|A versão de SO convidado do Azure para ser instalada nas máquinas virtuais no conjunto.<br /><br /> O valor predefinido é **\*** que especifica a versão mais recente do sistema operativo para a família especificada.<br /><br /> Para outros valores permitidos, consulte [Azure de SO convidado](https://azure.microsoft.com/documentation/articles/cloud-services-guestos-update-matrix/#releases).|

###  <a name="bk_vmconf"></a> virtualMachineConfiguration

|Nome do elemento|Tipo|Notas|
|------------------|----------|-----------|
|[imageReference](#bk_imgref)|Tipo complexo|Especifica informações sobre a plataforma ou a imagem do Marketplace para utilizar.|
|nodeAgentSKUId|Cadeia|O SKU do agente de nó do Batch aprovisionado no nó de computação.|
|[windowsConfiguration](#bk_winconf)|Tipo complexo|Especifica as definições do sistema operativo Windows na máquina virtual. Esta propriedade não pode ser especificado se o imageReference está a referenciar uma imagem de SO Linux.|

###  <a name="bk_imgref"></a> imageReference

|Nome do elemento|Tipo|Notas|
|------------------|----------|-----------|
|publisher|Cadeia|O publicador da imagem.|
|oferta|Cadeia|A oferta da imagem.|
|sku|Cadeia|O SKU da imagem.|
|versão|Cadeia|A versão da imagem.|

###  <a name="bk_winconf"></a> windowsConfiguration

|Nome do elemento|Tipo|Notas|
|------------------|----------|-----------|
|enableAutomaticUpdates|Booleano|Indica se a máquina virtual está ativada para as atualizações automáticas. Se esta propriedade não for especificada, o valor predefinido é verdadeiro.|

###  <a name="bk_netconf"></a> networkConfiguration

|Nome do elemento|Tipo|Notas|
|------------------|--------------|----------|
|subnetId|Cadeia|Especifica o identificador de recurso da sub-rede na qual nós de computação do conjunto são criados.|
