---
title: Instalar Presto em clusters do Linux de HDInsight do Azure
description: Saiba como instalar Presto e Airpal em clusters de Hadoop do HDInsight baseado em Linux utilizar ações de Script.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: jasonh
ms.openlocfilehash: ea777b13348b84aaeb7cb7628a4d0aac9f5705bd
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/07/2018
ms.locfileid: "39591426"
---
# <a name="install-and-use-presto-on-hdinsight-hadoop-clusters"></a>Instalar e utilizar o Presto em clusters do HDInsight Hadoop

Neste documento, irá aprender a instalar prontamente em clusters Hadoop do HDInsight utilizando a ação de Script. Também irá aprender a instalar Airpal num cluster HDInsight Presto existente.

> [!IMPORTANT]
> Os passos neste documento exigem uma **cluster de Hadoop do HDInsight 3.5** que utilizam o Linux. O Linux é o único sistema operativo utilizado na versão 3.4 ou superior do HDInsight. Para obter mais informações, consulte [versões do HDInsight](hdinsight-component-versioning.md).

## <a name="what-is-presto"></a>O que é Presto?
[Presto](https://prestodb.io/overview.html) é um rápido motor de consulta SQL distribuído para grandes volumes de dados. Presto é adequado para consultas interativas de petabytes de dados. Para obter mais informações sobre os componentes de Presto e como elas funcionam em conjunto, consulte [conceitos Presto](https://github.com/prestodb/presto/blob/master/presto-docs/src/main/sphinx/overview/concepts.rst).

> [!WARNING]
> Componentes fornecidos com o cluster do HDInsight são totalmente suportadas e Support da Microsoft irá ajudar a isolar e resolver problemas relacionados com esses componentes.
> 
> Componentes personalizados, por exemplo, Presto, recebem suporte comercialmente razoável para ajudá-lo a resolver o problema. Isso pode resultar em resolver o problema ou pedir-lhe para interagir com os canais disponíveis para as tecnologias de código-fonte aberto, onde os conhecimentos aprofundados para essa tecnologia é encontrado. Por exemplo, há muitos sites de Comunidade que podem ser utilizados, como: [fórum do MSDN para o HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [ http://stackoverflow.com ](http://stackoverflow.com). Também projetos Apache tem sites de projeto no [ http://apache.org ](http://apache.org), por exemplo: [Hadoop](http://hadoop.apache.org/).
> 
> 


## <a name="install-presto-using-script-action"></a>Instalar prontamente com ação de script

Esta secção fornece instruções sobre como utilizar o script de exemplo, ao criar um novo cluster com o portal do Azure. 

1. Iniciar o aprovisionamento de um cluster, utilizando os passos em [clusters do HDInsight baseado em Linux aprovisionar](hdinsight-hadoop-create-linux-clusters-portal.md). Certifique-se de criar o cluster com o **personalizado** fluxo de criação do cluster. O cluster tem de cumprir os seguintes requisitos.

    * Tem de ser um cluster do Hadoop com o HDInsight versão 3.6.

    * Terá de utilizar armazenamento do Azure que o arquivo de dados. Ainda, o com, Presto, num cluster que utiliza o Azure Data Lake Store como a opção de armazenamento não é suportado. 

    ![Criação do cluster HDInsight utilizando opções personalizadas](./media/hdinsight-hadoop-install-presto/hdinsight-install-custom.png)

2. Sobre o **definições avançadas** área, selecione **ações de Script**e forneça as informações abaixo:
   
   * **NOME**: introduza um nome amigável para a ação de script.
   * **URI do script de Bash**: `https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh`
   * **HEAD**: Marque esta opção
   * **Função de trabalho**: Marque esta opção
   * **ZOOKEEPER**: desmarque esta caixa de verificação
   * **PARÂMETROS**: deixe este campo em branco


3. Na parte inferior a **ações de Script** área, clique nas **selecione** botão para guardar a configuração. Por último, clique a **selecionar** na parte inferior do **definições avançadas** área para guardar as informações de configuração.

4. Continuar o aprovisionamento do cluster, conforme descrito em [clusters do HDInsight baseado em Linux aprovisionar](hdinsight-hadoop-create-linux-clusters-portal.md).

    > [!NOTE]
    > O Azure PowerShell, a CLI do Azure, o SDK de .NET do HDInsight ou modelos Azure Resource Manager também podem ser utilizados para aplicar ações de script. Também pode aplicar ações de script para clusters já em execução. Para obter mais informações, consulte [HDInsight personalizar clusters com ações de Script](hdinsight-hadoop-customize-cluster-linux.md).
    > 
    > 

## <a name="use-presto-with-hdinsight"></a>Utilizar o Presto com o HDInsight

Para trabalhar com Presto num cluster do HDInsight, utilize os seguintes passos:

1. Ligar ao cluster do HDInsight com o SSH:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Para obter mais informações, veja [Utilizar SSH com o HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).
     

2. Inicie o shell Presto com o seguinte comando.
   
        presto --schema default

3. Executar uma consulta numa tabela de exemplo **hivesampletable**, que está disponível em todos os clusters do HDInsight por predefinição.
   
        select count (*) from hivesampletable;
   
    Por predefinição, [Hive](https://prestodb.io/docs/current/connector/hive.html) e [TPCH](https://prestodb.io/docs/current/connector/tpch.html) conectores para Presto já estão configuradas. Conector de ramo de registo está configurado para utilizar a instalação do ramo de registo predefinido instalado, por isso, todas as tabelas de ramo de registo será automaticamente visíveis no Presto.

    Para obter mais informações, consulte [documentação Presto](https://prestodb.io/docs/current/index.html).

## <a name="use-airpal-with-presto"></a>Utilizar Airpal com Presto

[Airpal](https://github.com/airbnb/airpal#airpal) é uma interface de consulta baseada na web de código-fonte aberto para Presto. Para obter mais informações sobre Airpal, consulte [Airpal documentação](https://github.com/airbnb/airpal#airpal).

Utilize os seguintes passos para instalar Airpal no nó de extremidade:

1. Utilizar SSH, ligue ao nó principal do cluster HDInsight que instalou Presto:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Para obter mais informações, veja [Utilizar SSH com o HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Assim que estiver ligado, execute o seguinte comando.

        sudo slider registry  --name presto1 --getexp presto 
   
    Pode ver o resultado semelhante ao seguinte JSON:

        {
            "coordinator_address" : [ {
                "value" : "10.0.0.12:9090",
                "level" : "application",
                "updatedTime" : "Mon Apr 03 20:13:41 UTC 2017"
        } ]

3. A partir da saída, anote o valor para o **valor** propriedade. Este valor será necessário ao instalar o Airpal edgenode o cluster. A partir da saída acima, é o valor que irá precisar **10.0.0.12:9090**.

4. Utilizar o modelo **[aqui](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2Fpresto-hdinsight%2Fmaster%2Fairpal-deploy.json)** para criar um edgenode de cluster do HDInsight e forneça os valores, conforme mostrado na captura de ecrã seguinte.

    ![Instalação de HDInsight Airpal num Presto cluster](./media/hdinsight-hadoop-install-presto/hdinsight-install-airpal.png)

5. Clique em **Comprar**.

6. Depois das alterações são aplicadas à configuração do cluster, pode acessar a interface da web Airpal utilizando os seguintes passos.

    1. A caixa de diálogo do cluster, clique em **aplicativos**.

        ![Lançamento de HDInsight Airpal no Presto cluster](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal.png)

    2. Partir do **aplicações instaladas** área, clique em **Portal** contra airpal.

        ![Lançamento de HDInsight Airpal no Presto cluster](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal-1.png)

    3. Quando lhe for pedido, introduza as credenciais de administrador que especificou ao criar o cluster de Hadoop do HDInsight.

## <a name="customize-a-presto-installation-on-hdinsight-cluster"></a>Personalizar uma instalação Presto num cluster do HDInsight

Para personalizar a instalação, utilize os seguintes passos:

1. Utilizar SSH, ligue ao nó principal do cluster HDInsight que instalou Presto:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Para obter mais informações, veja [Utilizar SSH com o HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Faça as alterações de configuração no ficheiro `/var/lib/presto/presto-hdinsight-master/appConfig-default.json`. Para obter mais informações sobre configuração Presto, consulte [Presto configuração para clusters baseados no YARN](https://prestodb.io/presto-yarn/installation-yarn-configuration-options.html).

3. Parar e eliminar a instância em execução atual de Presto.

        sudo slider stop presto1 --force
        sudo slider destroy presto1 --force

4. Inicie uma nova instância do Presto com a personalização.

       sudo slider create presto1 --template /var/lib/presto/presto-hdinsight-master/appConfig-default.json --resources /var/lib/presto/presto-hdinsight-master/resources-default.json

5. Aguarde que a nova instância estar pronto e anote o endereço de coordenador presto.


       sudo slider registry --name presto1 --getexp presto

## <a name="generate-benchmark-data-for-hdinsight-clusters-that-run-presto"></a>Gerar dados de benchmark para clusters do HDInsight que executam o Presto

TPC-DS é a norma da indústria para medir o desempenho de muitos sistemas de suporte de decisão, incluindo sistemas de grandes volumes de dados. Pode usar Presto para gerar dados e avaliar o compara com os seus dados de benchmark do HDInsight. Para obter mais informações, consulte [aqui](https://github.com/hdinsight/tpcds-datagen-as-hive-query/blob/master/README.md).



## <a name="see-also"></a>Consulte também
* [Instalar e utilizar Hue em clusters do HDInsight](hdinsight-hadoop-hue-linux.md). Hue é uma IU que torna mais fácil criar, executar e guarde Pig e Hive tarefas web.

* [Instalar o Giraph nos clusters do HDInsight](hdinsight-hadoop-giraph-install-linux.md). Utilize a personalização de cluster para instalar o Giraph nos clusters do HDInsight Hadoop. Giraph permite-lhe efetuar o processamento através do Hadoop de gráficos e pode ser utilizado com o Azure HDInsight.

* [Instalar o Solr nos clusters do HDInsight](hdinsight-hadoop-solr-install-linux.md). Utilize a personalização de cluster para instalar o Solr nos clusters do HDInsight Hadoop. Solr permite-lhe efetuar operações de pesquisa poderosa em dados armazenados.

[hdinsight-install-r]: hdinsight-hadoop-r-scripts-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
