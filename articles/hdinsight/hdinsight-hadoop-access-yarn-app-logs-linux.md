---
title: Registos de aplicações do YARN do Hadoop de acesso no HDInsight baseado em Linux - Azure
description: Saiba como aceder a registos de aplicações do YARN num cluster do HDInsight baseado em Linux (Hadoop) através da linha de comandos e um navegador da web.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/22/2018
ms.author: jasonh
ms.openlocfilehash: 0aa8d76ca97789844482d2b8ad7212c2f61a8ab8
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/07/2018
ms.locfileid: "39591808"
---
# <a name="access-yarn-application-logs-on-linux-based-hdinsight"></a>Registos de aplicações do YARN de acesso no HDInsight baseado em Linux

Saiba como aceder aos registos de aplicações do YARN (ainda outro recurso Negociador) num cluster de Hadoop no HDInsight do Azure.

> [!IMPORTANT]
> Os passos neste documento exigem um cluster do HDInsight que utilize o Linux. O Linux é o único sistema operativo utilizado na versão 3.4 ou superior do HDInsight. Para obter mais informações, consulte [controlo de versões de componente de HDInsight](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="YARNTimelineServer"></a>Servidor de linha cronológica do YARN

O [servidor de linha cronológica do YARN](http://hadoop.apache.org/docs/r2.7.3/hadoop-yarn/hadoop-yarn-site/TimelineServer.html) fornece informações genéricas sobre aplicações concluídas e informações de aplicações com arquiteturas específicas através de duas interfaces diferentes. Especificamente:

* Armazenamento e obtenção de informações de aplicação genérica em clusters de HDInsight foi ativado com a versão 3.1.1.374 ou superior.
* O componente de informações de aplicações com arquiteturas específicas do servidor de linha cronológica não está atualmente disponível em clusters do HDInsight.

Informações genéricas sobre aplicações incluem o seguinte tipo de dados:

* O ID da aplicação, um identificador exclusivo de uma aplicação
* O utilizador que iniciou o aplicativo
* Informações sobre tentativas de concluir a aplicação
* Os contentores utilizados por qualquer tentativa de determinado aplicativo

## <a name="YARNAppsAndLogs"></a>Registos de aplicações do YARN e

YARN suporta vários modelos de programação (MapReduce sendo um deles) ao desassociar a gestão de recursos de agendamento/monitorização de aplicações. YARN utiliza um global *ResourceManager* (RM), por função de trabalho-nó *NodeManagers* (NMs) e por aplicativo *ApplicationMasters* (AMs). O AM por aplicação negocia recursos (CPU, memória, disco e rede) para executar a aplicação com o RM. O RM funciona com NMs para conceder estes recursos, o que são concedidos como *contentores*. O AM é responsável por rastrear o progresso dos contentores atribuído pelo RM. Um aplicativo pode exigir o número de contentores dependendo da natureza do aplicativo.

Cada aplicativo pode consistir em várias *tentativas de aplicativo*. Se uma aplicação falhar, pode ser repetida como uma nova tentativa. Cada tentativa é executado num contentor. De certa forma, um contentor fornece o contexto para a unidade básica de trabalho realizado por uma aplicação YARN. Todo o trabalho é feito no contexto de um contentor é executado no nó de trabalho único no qual o contentor foi alocado. Ver [conceitos de YARN] [ YARN-concepts] para referência ainda mais.

Registos de aplicações (e os registos de contentor associado) são essenciais na depuração de aplicativos problemáticos do Hadoop. O YARN fornece uma estrutura legal para recolher, Agregar e armazenar os registos de aplicações com o [Log agregação] [ log-aggregation] funcionalidade. A funcionalidade de agregação de Log torna os registos da aplicação ao aceder ao mais determinístico. Ele agrega registos em todos os contentores num nó de trabalho e armazena-as como um ficheiro de registo de agregados por nó de trabalho. O registo é armazenado no sistema de ficheiros predefinido após a conclusão de uma aplicação. Seu aplicativo pode usar centenas ou milhares de contentores, mas os registos para todos os contentores executados num nó de trabalho única sempre são agregados para um único arquivo. Portanto, há apenas 1 registo por nó de trabalho utilizado pela sua aplicação. Agregação de registo está ativada por predefinição em clusters de HDInsight versão 3.0 e superior. Registos agregados estão localizados no armazenamento de predefinido para o cluster. O caminho seguinte é o caminho do HDFS para os registos:

    /app-logs/<user>/logs/<applicationId>

No caminho, `user` é o nome do utilizador que iniciou o aplicativo. O `applicationId` é o identificador exclusivo atribuído a uma aplicação pelo RM YARN.

Os registos agregados não são diretamente legíveis, como eles são escritos numa [TFile][T-file], [formato binário] [ binary-format] indexados por contêiner. Utilize os registos do YARN Resource Manager ou ferramentas CLI para ver estes registos como texto simples para aplicações ou contentores de interesse.

## <a name="yarn-cli-tools"></a>Ferramentas de CLI do YARN

Para utilizar as ferramentas de CLI do YARN, primeiro tem de ligar para o cluster do HDInsight através de SSH. Para obter informações, veja [Use SSH with HDInsight (Utilizar SSH com o HDInsight)](hdinsight-hadoop-linux-use-ssh-unix.md).

Pode ver estes registos como texto simples ao executar um dos seguintes comandos:

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>

Especifique a &lt;applicationId >, &lt;aplicativos para o usuário que-iniciada-a >, &lt;containerId >, e &lt;endereço do nó do trabalho > informações ao executar estes comandos.

## <a name="yarn-resourcemanager-ui"></a>IU de ResourceManager do YARN

IU de ResourceManager do YARN é executado no nó principal do cluster. Ele é acessado através da IU web do Ambari. Utilize os seguintes passos para ver os registos do YARN:

1. No seu browser, navegue para https://CLUSTERNAME.azurehdinsight.net. Substitua CLUSTERNAME com o nome do cluster do HDInsight.
2. Na lista de serviços no lado esquerdo, selecione **YARN**.

    ![Serviço de yarn selecionado](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarnservice.png)
3. Do **ligações rápidas** menu pendente, selecione um de nós principais do cluster e, em seguida, selecione **ResourceManager Log**.

    ![Links rápidos de yarn](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarnquicklinks.png)

    É apresentada uma lista de ligações para os registos YARN.

[YARN-timeline-server]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:http://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/
