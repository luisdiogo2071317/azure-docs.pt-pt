---
title: "Descrição geral do Gestor de dados do Microsoft Azure StorSimple | Microsoft Docs"
description: "Fornece uma descrição geral do serviço StorSimple Manager de dados"
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 01/16/2018
ms.author: vidarmsft
ms.openlocfilehash: 8b0ff2c100878e568e0a4c67e79864006512bd78
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2018
---
# <a name="storsimple-data-manager-solution-overview"></a>Descrição geral da solução do Gestor de dados do StorSimple

## <a name="overview"></a>Descrição geral

Microsoft Azure StorSimple utiliza armazenamento na nuvem como uma extensão da solução no local e automaticamente os dados de camadas em armazenamento no local e na nuvem. Os dados são armazenados na nuvem num formato com eliminação de duplicados e comprimido para uma eficiência máxima e para reduzir os custos. Como os dados são armazenados no formato do StorSimple, não é imediato consumíveis por outras aplicações na nuvem que pretende utilizar.

O Gestor de dados do StorSimple permite-lhe uma forma totalmente integrada aceder e utilizar os dados de formato do StorSimple na nuvem. Fazê-lo por transformar o formato do StorSimple em blobs nativos e ficheiros, o que pode utilizar com outros serviços, tais como os Media Services do Azure, Azure HDInsights e do Azure Machine Learning.

Este artigo fornece uma descrição geral da solução StorSimple Manager de dados. Também explica como pode utilizar este serviço para escrever as aplicações que utilizam dados StorSimple e outros serviços do Azure na nuvem.

## <a name="how-it-works"></a>Como funciona?

O serviço StorSimple Manager de dados identifica os dados do StorSimple na nuvem a partir de um dispositivo de local de série 8000 do StorSimple. Os dados do StorSimple na nuvem é eliminação, comprimidos formato StorSimple. O serviço do Gestor de dados fornece APIs para extrair os dados de formato StorSimple e transformá-los para outros formatos, tais como blobs do Azure e ficheiros do Azure. Isto transformados dados, em seguida, é prontamente consumidos pelos serviços do Azure HDInsight e suportes de dados do Azure. A transformação de dados permite assim que estes serviços operar os dados transformados do StorSimple do dispositivo de no local de série 8000 do StorSimple. Este fluxo é ilustrado no diagrama seguinte.

![Diagrama de alto nível](./media/storsimple-data-manager-overview/storsimple-data-manager-overview2.png)


## <a name="data-manager-use-cases"></a>Casos de utilização do Gestor de dados

Pode utilizar o Gestor de dados com as funções do Azure, da automatização do Azure e do Azure Data Factory com fluxos de trabalho em execução nos seus dados, conforme for necessário para StorSimple. Pode querer processar suporte de dados de conteúdo que armazenar no StorSimple com Media Services do Azure, ou executar um algoritmo de aprendizagem máquina em que os dados ou que apresente um cluster de Hadoop para analisar os dados que armazena no StorSimple. Com a vasta matriz de serviços disponíveis no Azure combinado com os dados no StorSimple, pode desbloquear o poder dos seus dados.


## <a name="region-availability"></a>Disponibilidade de região

O Gestor de dados do StorSimple está disponível nas seguintes 7 regiões:

 - Sudeste Asiático
 - EUA Leste
 - EUA Oeste
 - EUA Oeste 2
 - EUA Centro-Oeste
 - Europa do Norte
 - Europa Ocidental

No entanto, o Gestor de dados do StorSimple pode ser utilizado para transformar dados nas regiões seguintes. 

![Regiões disponíveis para os dados](./media/storsimple-data-manager-overview/data-manager-job-definition-different-regions.png)

Este conjunto de é maior porque a implementação de recursos em qualquer uma das regiões do acima é capaz de chamar o processo de transformação no abaixo regiões. Por isso, desde que os dados residirem em qualquer uma das regiões do 26, pode transformar os dados a utilizar este serviço.


## <a name="choosing-a-region"></a>Escolher uma região

Recomendamos que:
 - A conta de armazenamento de origem (aquele associadas com o dispositivo StorSimple) e a conta de armazenamento de destino (onde pretende que os dados no formato nativo) de estar na mesma região do Azure.
 - Trazer a sua definição de tarefa e o Gestor de dados na região que contenha a conta de armazenamento StorSimple. Se não for possível, coloque a cópia de segurança o Gestor de dados na região do Azure mais próximo e, em seguida, criar a definição de tarefa na mesma região que a sua conta de armazenamento StorSimple. 

    Se a conta de armazenamento StorSimple não é de 26 regiões que suporta a criação de definição de tarefa, recomendamos que não executar o Gestor de dados do StorSimple como ver latências longas e encargos associados à saída potencialmente elevado.

## <a name="security-considerations"></a>Considerações de segurança

O Gestor de dados do StorSimple tem a chave de encriptação de dados do serviço para transformar o formato do StorSimple para o formato nativo. A chave de encriptação de dados do serviço é gerada quando o primeiro dispositivo se regista no serviço StorSimple. Para obter mais informações sobre esta chave, aceda a [segurança do StorSimple](storsimple-8000-security.md).

A chave de encriptação de dados do serviço fornecido como entrada é armazenado no Cofre de chaves é criado quando cria um Gestor de dados. O Cofre reside na mesma região do Azure como o Gestor de dados do StorSimple. Esta chave é eliminada ao eliminar o serviço do Gestor de dados.

Esta chave é utilizada por recursos de computação para efetuar a transformação. Computação estes recursos estão localizados na mesma região do Azure como a definição da tarefa. Esta região pode ou não ser o mesmo que a região onde colocar a cópia de segurança o Gestor de dados.

Se a sua região do Gestor de dados é diferente da sua região de definição de tarefa, é importante compreender que dados/metadados reside em cada um destas regiões. O diagrama seguinte ilustra o efeito de ter regiões diferentes para a definição de tarefa e o Gestor de dados.

![Definição de serviço e a tarefa em regiões diferentes](./media/storsimple-data-manager-overview/data-manager-job-different-regions.png)

## <a name="next-steps"></a>Passos Seguintes

[Utilize o Gestor de dados StorSimple IU para transformar os seus dados](storsimple-data-manager-ui.md).