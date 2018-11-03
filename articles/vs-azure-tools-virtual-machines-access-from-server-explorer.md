---
title: Aceder a máquinas virtuais do Azure a partir do Explorador de servidores | Documentos da Microsoft
description: Obtenha uma visão geral de como visualizar, criar e gerir máquinas virtuais do Azure (VMs) no Server Explorer no Visual Studio.
services: visual-studio-online
author: ghogen
manager: douge
assetId: eb3afde6-ba90-4308-9ac1-3cc29da4ede0
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 8/31/2017
ms.author: ghogen
ms.openlocfilehash: d25e79efa496d27d16c4c10c587ba25f25507455
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969413"
---
# <a name="accessing-azure-virtual-machines-from-server-explorer"></a>Aceder a máquinas virtuais do Azure a partir do Explorador de servidores

Se tiver máquinas virtuais alojadas pelo Azure, pode acessá-los no Explorador de servidores. Tem primeiro de iniciar sessão sua subscrição do Azure para ver os seus serviços móveis. Para iniciar sessão, abra o menu de atalho para o nó do Azure no Server Explorer e escolha **ligue-se ao Microsoft Azure**.

1. No Cloud Explorer, escolha uma máquina virtual e, em seguida, selecione a chave de F4 para mostrar a janela de propriedades.

    A tabela seguinte mostra as propriedades que estão disponíveis, mas eles são todos somente leitura. Para alterá-los, utilize o [portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).

   | Propriedade | Descrição |
   | --- | --- |
   | Nome DNS |O URL com o endereço da Internet da máquina virtual. |
   | Ambiente |Para uma máquina virtual, o valor desta propriedade é sempre de produção. |
   | Nome |O nome da máquina virtual. |
   | Tamanho |O tamanho da máquina virtual, que reflete a quantidade de memória e espaço em disco disponível. Para obter mais informações, consulte [tamanhos de máquinas virtuais](https://docs.microsoft.com/azure/cloud-services/cloud-services-sizes-specs). |
   | Estado |Os valores incluem inicial, introdução, parar, parado e a obter o estado. Se for apresentada a obter o estado, o estado atual é desconhecido. Os valores para esta propriedade diferentes dos valores que são utilizados no [portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040). |
   | subscriptionID |O ID de subscrição para a sua conta do Azure. Pode mostrar essas informações sobre o [portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040) visualizando as propriedades de uma subscrição. |
2. Escolha um nó de ponto final e, em seguida, veja a **propriedades** janela.
3. A tabela seguinte descreve as propriedades disponíveis de pontos de extremidade, mas são só de leitura. Para adicionar ou editar os pontos finais para uma máquina virtual, utilize o [portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040). 

   | Propriedade | Descrição |
   | --- | --- |
   | Nome |Um identificador para o ponto final. |
   | Porta privada |A porta para acesso à rede interna para a sua aplicação. |
   | Protocolo |O protocolo que usa a camada de transporte para este ponto final, TCP ou UDP. |
   | Porta Pública |A porta que é utilizada para acesso público à sua aplicação. |
