---
title: Por meio de programação – registos de aplicações do YARN do Hadoop de acesso do Azure
description: Registos de aplicações de acesso por meio de programação num cluster do Hadoop no HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: b7b93ca9c8638451d23a27edeed823e593a95b23
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/06/2018
ms.locfileid: "51035650"
---
# <a name="access-apache-yarn-application-logs-on-windows-based-hdinsight"></a>Registos de aplicações do Apache YARN de acesso no HDInsight baseado em Windows
Este documento explica como acessar os registos para aplicativos de Apache YARN que tem concluído a num cluster de Hadoop baseado em Windows no Azure HDInsight

> [!IMPORTANT]
> As informações neste documento aplicam-se apenas a clusters do HDInsight baseado em Windows. O Linux é o único sistema operativo utilizado na versão 3.4 ou superior do HDInsight. Para obter mais informações, veja [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (Desativação do HDInsight no Windows). Para obter informações sobre como acessar o YARN registos em clusters do HDInsight baseado em Linux, consulte [registos de aplicações de acesso Apache YARN no Hadoop baseado em Linux no HDInsight](hdinsight-hadoop-access-yarn-app-logs-linux.md)
>


### <a name="prerequisites"></a>Pré-requisitos
* Um cluster do HDInsight baseado em Windows.  Ver [Hadoop baseado em Windows criar clusters no HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="yarn-timeline-server"></a>Servidor de linha cronológica do YARN
O <a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">servidor de linha cronológica do YARN</a> fornece informações genéricas sobre aplicações concluídas, bem como framework informações específicas da aplicação através de duas interfaces diferentes. Especificamente:

* Armazenamento e obtenção de informações de aplicação genérica em clusters de HDInsight foi ativado com a versão 3.1.1.374 ou superior.
* O componente de informações de aplicações com arquiteturas específicas do servidor de linha cronológica não está atualmente disponível em clusters do HDInsight.

Informações genéricas sobre aplicações incluem os seguintes tipos de dados:

* O ID da aplicação, um identificador exclusivo de uma aplicação
* O utilizador que iniciou o aplicativo
* Informações sobre tentativas de concluir a aplicação
* Os contentores utilizados por qualquer tentativa de determinado aplicativo

Nos seus clusters do HDInsight, essas informações são armazenadas pelo Azure Resource Manager. As informações são guardadas para o arquivo de histórico no armazenamento padrão para o cluster. Estes dados genéricos sobre aplicações concluídas podem ser obtidos através da REST API:

    GET on https://<cluster-dns-name>.azurehdinsight.net/ws/v1/applicationhistory/apps


## <a name="yarn-applications-and-logs"></a>Registos de aplicações do YARN e
YARN suporta vários modelos de programação ao desassociar a gestão de recursos de agendamento/monitorização de aplicações. YARN utiliza um global *ResourceManager* (RM), por função de trabalho-nó *NodeManagers* (NMs) e por aplicativo *ApplicationMasters* (AMs). O AM por aplicação negocia recursos (CPU, memória, disco e rede) para executar a aplicação com o RM. O RM funciona com NMs para conceder estes recursos, o que são concedidos como *contentores*. O AM é responsável por rastrear o progresso dos contentores atribuído pelo RM. Um aplicativo pode exigir o número de contentores dependendo da natureza do aplicativo.

* Cada aplicativo pode consistir em várias *tentativas de aplicativo*. 
* Contentores são concedidos a uma tentativa de específica de um aplicativo. 
* Um contentor fornece o contexto para uma unidade básica de trabalho. 
* Trabalho que é feito no contexto de um contentor é executado no nó de trabalho única que o contentor foi atribuído a. 

Para obter mais informações, consulte [conceitos de YARN][YARN-concepts].

Registos de aplicações (e os registos de contentor associado) são essenciais na depuração de aplicativos problemáticos do Hadoop. O YARN fornece uma estrutura legal para recolher, Agregar e armazenar os registos de aplicações com o [Log agregação] [ log-aggregation] funcionalidade. A funcionalidade de agregação de Log torna os registos da aplicação ao aceder ao mais determinístico, como ele agrega registos em todos os contentores num nó de trabalho e armazena-as como um agregado ficheiro de registo por nó de trabalho no sistema de ficheiros predefinido após a conclusão de uma aplicação. Seu aplicativo pode usar centenas ou milhares de contentores, mas os registos para todos os contentores executados num nó de trabalho único são agregados para um único arquivo, resultando num arquivo por nó de trabalho utilizado pela sua aplicação. Agregação de registo está ativada por predefinição em clusters do HDInsight (versão 3.0 e superior), e os registos agregados podem ser encontrados no contentor predefinido do cluster na seguinte localização:

    wasb:///app-logs/<user>/logs/<applicationId>

Nessa localização *usuário* é o nome do utilizador que iniciou o aplicativo, e *applicationId* é o identificador exclusivo de uma aplicação como atribuído pelo RM YARN.

Os registos agregados não são diretamente legíveis, como eles são escritos numa [TFile][T-file], [formato binário] [ binary-format] indexados por contêiner. O YARN fornece ferramentas CLI para Despejar estes registos como texto simples para aplicações ou contentores de interesse. Pode ver estes registos como texto simples através da execução de um do YARN seguinte comandos diretamente em nós de cluster (depois de ligar ao mesmo através de RDP):

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>


## <a name="yarn-resourcemanager-ui"></a>IU de ResourceManager do YARN
IU de ResourceManager do YARN é executado no nó principal do cluster e pode ser acedido através do dashboard do portal do Azure:

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).
2. No menu da esquerda, clique em **navegue**, clique em **Clusters do HDInsight**, clique num cluster baseado em Windows que pretende aceder os registos de aplicações do YARN.
3. No menu superior, clique em **Dashboard**. É apresentada uma página aberta no browser nova guia chamada **consola de consulta do HDInsight**.
4. Partir **consola de consulta do HDInsight**, clique em **IU do Yarn**.

[YARN-timeline-server]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:http://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/
