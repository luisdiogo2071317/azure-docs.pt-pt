---
title: Operacionalize ML serviços no HDInsight - Azure | Microsoft Docs
description: Saiba como operacionalizar ML serviços no Azure HDInsight.
services: hdinsight
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: nitinme
ms.openlocfilehash: caefe30ff567a5e24e1f4c3a11309bd35e06190c
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37046144"
---
# <a name="operationalize-ml-services-cluster-on-azure-hdinsight"></a>Operacionalize cluster ML serviços no Azure HDInsight

Depois de ter utilizado cluster ML serviços no HDInsight para concluir os dados de modelação, pode operacionalizar o modelo para tornar as predições. Este artigo fornece instruções sobre como efetuar esta tarefa.

## <a name="prerequisites"></a>Pré-requisitos

* **Um cluster de ML serviços no HDInsight**: para obter instruções, consulte [começar com os serviços de ML no HDInsight](r-server-get-started.md).

* **Um cliente Secure Shell (SSH)**: é utilizado um cliente SSH para ligar remotamente ao cluster do HDInsight e executar comandos diretamente no mesmo. Para obter mais informações, veja [Utilizar SSH com o HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="operationalize-ml-services-cluster-with-one-box-configuration"></a>Cluster de ML serviços com a configuração de uma caixa de operacionalizar

> [!NOTE]
> Os passos abaixo são aplicáveis a R Server 9.0 e 9.1 do servidor de ML. Para ML 9.3 de servidor, consulte [utilizar a ferramenta de administração para gerir a configuração de operationalization](https://docs.microsoft.com/machine-learning-server/operationalize/configure-admin-cli-launch).

1. Aceda através de SSH ao nó de extremidade.

        ssh USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

    Para obter instruções sobre como utilizar o SSH com o Azure HDInsight, consulte [utilizar o SSH com o HDInsight.](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Altere o diretório para a versão relevante e sudo a dll net ponto: 

    - Para o servidor de ML Microsoft 9.1:

            cd /usr/lib64/microsoft-r/rserver/o16n/9.1.0
            sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll

    - Para o Microsoft R Server 9.0:

            cd /usr/lib64/microsoft-deployr/9.0.1
            sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll

3. São apresentados com as opções à sua escolha. Escolha a primeira opção, conforme mostrado na captura de ecrã seguinte, a **configurar o servidor de ML para Operationalization**.

    ![operacionalização de one-box](./media/r-server-operationalize/admin-util-one-box-1.png)

4. Agora são apresentados com a opção de escolha como pretende operacionalizar o servidor de ML. Entre as opções apresentadas, escolha a primeira introduzindo **A**.

    ![operacionalização de one-box](./media/r-server-operationalize/admin-util-one-box-2.png)

5. Quando lhe for pedido, introduza e reintroduza a palavra-passe para um utilizador de administrador local.

6. Deverá ver saídas sugerindo que a operação foi concluída com êxito. Também lhe for pedido para selecionar outra opção de menu. Selecione i para regressar ao menu principal.

    ![operacionalização de one-box](./media/r-server-operationalize/admin-util-one-box-3.png)

7. Opcionalmente, pode efetuar as verificações de diagnóstico ao executar um teste de diagnóstico da seguinte forma:

    a. No menu principal, selecione **6** para executar testes de diagnóstico.

    ![operacionalização de one-box](./media/r-server-operationalize/diagnostic-1.png)

    b. No menu de testes de diagnóstico, selecione **A**. Quando lhe for pedido, introduza a palavra-passe que forneceu para o utilizador de administrador local.

    ![operacionalização de one-box](./media/r-server-operationalize/diagnostic-2.png)

    c. Certifique-se de que o resultado mostra que o estado de funcionamento geral é uma passagem.

    ![operacionalização de one-box](./media/r-server-operationalize/diagnostic-3.png)

    d. Entre as opções de menu apresentadas, introduza **i** para regressar ao menu principal e, em seguida, introduza **8** para sair do utilitário de administração.

### <a name="long-delays-when-consuming-web-service-on-spark"></a>Grandes atrasos quando o consumo do serviço web no Spark

Se ocorrerem atrasos longos quando tentar consumir um serviço Web criado com funções mrsdeploy num contexto de computação Spark, poderá ter de adicionar algumas pastas em falta. A aplicação Spark pertence a um utilizador chamado "*rserve2*" sempre que é invocado a partir de um serviço Web utilizando funções mrsdeploy. Para contornar este problema:

    # Create these required folders for user 'rserve2' in local and hdfs:

    hadoop fs -mkdir /user/RevoShare/rserve2
    hadoop fs -chmod 777 /user/RevoShare/rserve2

    mkdir /var/RevoShare/rserve2
    chmod 777 /var/RevoShare/rserve2


    # Next, create a new Spark compute context:
 
    rxSparkConnect(reset = TRUE)


Nesta fase, a configuração da operacionalização está concluída. Agora, pode utilizar o `mrsdeploy` pacote no seu RClient para ligar ao operationalization no nó de extremidade e começar a utilizar as suas funcionalidades, como [execução remota](https://docs.microsoft.com/machine-learning-server/r/how-to-execute-code-remotely) e [serviços web](https://docs.microsoft.com/machine-learning-server/operationalize/concept-what-are-web-services). Consoante o seu cluster estar ou não configurado numa rede virtual, poderá ter de configurar o túnel de encaminhamento de portas através do início de sessão SSH. As secções a seguir explicam como configurar este túnel.

### <a name="ml-services-cluster-on-virtual-network"></a>Cluster de ML serviços na rede virtual

Confirme que permite o tráfego pela porta 12800 para o nó de extremidade. Desta forma, pode utilizar o nó de extremidade para ligar à funcionalidade Operacionalização.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://[your-cluster-name]-ed-ssh.azurehdinsight.net:12800",
        username = "admin",
        password = "xxxxxxx"
    )


Se `remoteLogin()` não se conseguir ligar ao nó de extremidade, mas conseguir aceder a este através de SSH, tem de verificar se a regra para permitir o tráfego na porta 12800 foi devidamente configurada ou não. Se o problema persistir, pode utilizar uma solução, que consiste em configurar o túnel de encaminhamento de portas através de SSH. Para obter instruções, consulte a secção seguinte:

### <a name="ml-services-cluster-not-set-up-on-virtual-network"></a>Cluster de serviços de ML não configurar a rede virtual

Se o seu cluster não estiver configurado numa VNet ou se estiver com problemas de conectividade através da VNet, pode utilizar o túnel de encaminhamento de portas SSH.

    ssh -L localhost:12800:localhost:12800 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

Quando a sessão SSH estiver ativa, o tráfego da porta 12800 do computador é encaminhado para a porta 12800 do nó de extremidade através dessa sessão. Confirme que utiliza `127.0.0.1:12800` no método `remoteLogin()`. Os registos para operationalization do nó de extremidade através do reencaminhamento de porta.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://127.0.0.1:12800",
        username = "admin",
        password = "xxxxxxx"
    )


## <a name="scale-operationalized-compute-nodes-on-hdinsight-worker-nodes"></a>Dimensionar operacionalizado nós no HDInsight nós de trabalho de computação

Dimensionar os nós de computação, primeiro desativar os nós de trabalho e, em seguida, configurar nós de computação em nós de trabalho desativado.

### <a name="step-1-decommission-the-worker-nodes"></a>Passo 1: Desativar os nós de trabalho

Cluster de ML serviços não é gerido através do YARN. Se os nós de trabalho não são desativados, o Gestor de recursos do YARN não funcionar conforme esperado porque não é compatível com os recursos que está a ser preenchido pelo servidor. Para evitar esta situação, recomendamos a desativação dos nós de trabalho antes de aumentar horizontalmente os nós de computação.

Siga estes passos para desativar nós de trabalho:

1. Inicie sessão na consola do Ambari do cluster e clique em **anfitriões** separador.

2. Selecione nós de trabalho (para ser desativado).

3. Clique em **ações** > **selecionado anfitriões** > **anfitriões** > **ativar o modo de manutenção**. Por exemplo, na imagem seguinte, selecionámos a desativação de wn3 e wn4.  

   ![desativar os nós de trabalho](./media/r-server-operationalize/get-started-operationalization.png)  

* Selecione **ações** > **anfitriões selecionados** > **DataNodes** > clique **Decommission**.
* Selecione **ações** > **anfitriões selecionados** > **NodeManagers** > clique **Decommission**.
* Selecione **ações** > **anfitriões selecionados** > **DataNodes** > clique **parar**.
* Selecione **ações** > **anfitriões selecionados** > **NodeManagers** > clique em **parar**.
* Selecione **ações** > **anfitriões selecionados** > **anfitriões** > clique **parar todos os componentes**.
* Desmarque os nós de trabalho e selecione os nós principais.
* Selecione **ações** > **selecionado anfitriões** > "**anfitriões** > **reiniciar todos os componentes**.

### <a name="step-2-configure-compute-nodes-on-each-decommissioned-worker-nodes"></a>Passo 2: Configurar nós em cada nós de trabalho desativado de computação

1. Aceda através de SSH a cada nó de trabalho desativado.

2. Execute o utilitário de administração com o DLL relevante para o cluster de serviços de ML que tiver. Para ML 9.1 de servidor, execute o seguinte:

        dotnet /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll

3. Introduza **1** para selecionar a opção **configurar o servidor de ML para Operationalization**.

4. Introduza **C** para selecionar a opção `C. Compute node`. Desta forma, o nó de computação é configurado no nó de trabalho.

5. Saia do Utilitário de Administração.

### <a name="step-3-add-compute-nodes-details-on-web-node"></a>Passo 3: Adicionar detalhes de nós no nó de web de computação

Depois de todos os nós de trabalho desativado estão configurados para executar o nó de computação, voltar atrás no nó de extremidade e adicionar endereços IP de nós de trabalho desativada na configuração do nó de servidor de ML web:

1. Aceda através de SSH ao nó de extremidade.

2. Execute `vi /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/appsettings.json`.

3. Procure a secção "Uris" e adicionar IP do nó de trabalho e os detalhes da porta.

       "Uris": {
         "Description": "Update 'Values' section to point to your backend machines. Using HTTPS is highly recommended",
         "Values": [
           "http://localhost:12805", "http://[worker-node1-ip]:12805", "http://[workder-node2-ip]:12805"
         ]
       }

## <a name="next-steps"></a>Passos Seguintes

* [Gerir serviços ML cluster no HDInsight](r-server-hdinsight-manage.md)
* [Opções de contexto do cluster de ML serviços no HDInsight de computação](r-server-compute-contexts.md)
* [As opções de armazenamento do Azure ML serviços de cluster no HDInsight](r-server-storage.md)
