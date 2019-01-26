---
title: Configurar o agendamento para clusters do HDInsight baseado em Linux - Azure da aplicação de patches de SO
description: Saiba como configurar o agendamento para clusters do HDInsight baseado em Linux a aplicação de patches de SO.
services: hdinsight
author: omidm1
ms.author: omidm
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/24/2019
ms.openlocfilehash: 402a4d59b57803b8a9c0094799ceee6a92df43f9
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2019
ms.locfileid: "54911358"
---
# <a name="os-patching-for-hdinsight"></a>Sistema operacional a aplicação de patches para HDInsight 

> [!IMPORTANT]
> Imagens do Ubuntu tornam-se disponível para criação de cluster do HDInsight novo dentro de 3 meses de ser publicada. A partir de Janeiro de 2019, são clusters em execução **não** aplicação de patches automática. Os clientes tem de utilizar as ações de script ou de outros mecanismos para corrigir um cluster em execução.

## <a name="how-to-configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>Como configurar o sistema operacional a aplicação de patches de agendamento para clusters do HDInsight baseado em Linux
As máquinas virtuais num cluster do HDInsight tem de ser reiniciado, ocasionalmente, para que podem ser instaladas patches de segurança importantes. A partir de 1 de Agosto de 2016, os novos clusters do HDInsight baseado em Linux (versão 3.4 ou superior,) são reiniciados através da agenda seguinte:

1. Uma máquina virtual do cluster pode apenas de reinício para patches no máximo, uma vez durante um período de 30 dias.
2. O reinício ocorre partida 12AM UTC.
3. O processo de reinicialização é escalonado em máquinas virtuais no cluster, para que o cluster ainda está disponível durante o processo de reinício.
4. O primeiro reinício para um cluster recém-criado não acontece mais cedo do que 30 dias após a data de criação do cluster.

Com a ação de script descrita neste artigo, pode modificar o sistema operacional a agenda de correção da seguinte forma:
1. Ativar ou desativar reinícios automáticos
2. Conjunto a frequência dos reinícios (dias entre reinícios)
3. Definir o dia da semana quando ocorre uma reinicialização

> [!NOTE]  
> Esta ação de script só funciona com clusters do HDInsight baseado em Linux criados após 1 de Agosto de 2016. Patches entrarão em vigor apenas quando as VMs são reiniciadas. 

## <a name="how-to-use-the-script"></a>Como utilizar o script 

Quando utilizar este script requer as seguintes informações:
1. A localização do script: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv01/os-patching-reboot-config.sh.  HDInsight utiliza este URI para localizar e executar o script em todas as máquinas virtuais no cluster.
  
2. Os tipos de nós de cluster que o script é aplicado a: nó principal, workernode, zookeeper. Este script deve ser aplicado a todos os tipos de nó do cluster. Se ele não se aplica a um tipo de nó, em seguida, as máquinas virtuais para esse tipo de nó vão continuar a utilizar a agenda de correções anterior.


3.  Parâmetro: Este script aceita três parâmetros numéricos:

    | Parâmetro | Definição |
    | --- | --- |
    | Ativar/desativar reinícios automáticos |0 ou 1. Um valor de 0 desativa a reinícios automáticos enquanto 1 ativa reinícios automáticos. |
    | Frequência |7 a 90 (inclusive). O número de dias a aguardar antes de reiniciar as máquinas virtuais para patches que requerem um reinício. |
    | Dia da semana |1 a 7 (inclusive). Um valor de 1 indica que o reinício deve ocorrer numa segunda-feira e 7 indica um exemplo de Sunday.For, utilizando parâmetros de 2 de 60 1 resulta em automático é reiniciado a cada 60 dias (no máximo) na Terça-feira. |
    | Persistência |Ao aplicar uma ação de script a um cluster existente, pode marcar o script como persistente. Os scripts persistentes são aplicados quando workernodes novos são adicionados ao cluster através de operações de dimensionamento. |

> [!NOTE]  
> Tem de marcar esse script como persistente ao aplicar a um cluster existente. Caso contrário, qualquer novos nós criado por meio de operações de dimensionamento irão utilizar a predefinição de agenda de correções.  Se aplicar o script como parte do processo de criação do cluster, ele é mantido automaticamente.

## <a name="next-steps"></a>Passos Seguintes

Para passos específicos sobre como utilizar a ação de script, consulte as secções seguintes a [HDInsight baseado em Linux personalizar clusters com ação de script](hdinsight-hadoop-customize-cluster-linux.md):

* [Utilize uma ação de script durante a criação do cluster](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation)
* [Aplicar uma ação de script para um cluster em execução](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)
