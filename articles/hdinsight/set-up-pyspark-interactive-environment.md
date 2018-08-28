---
title: Ferramentas do HDInsight do Azure - configurar o ambiente interativo do PySpark para Visual Studio Code
description: Saiba como utilizar o Azure HDInsight Tools para Visual Studio Code para criar e submeter consultas e scripts.
keywords: VScode, ferramentas do Azure HDInsight, Hive, Python, PySpark, Spark, HDInsight, Hadoop, LLAP, interativas do Hive, Interactive Query
services: hdinsight
ms.service: hdinsight
author: jejiang
ms.author: jejiang
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 10/27/2017
ms.openlocfilehash: e57fd3747ef9cbd55d073e02bc14816ca949bcd6
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43046240"
---
# <a name="set-up-the-pyspark-interactive-environment-for-visual-studio-code"></a>Configurar o ambiente interativo do PySpark para Visual Studio Code

Os passos seguintes mostram-lhe como instalar pacotes de Python, executando **HDInsight: interativo do PySpark**.


## <a name="set-up-the-pyspark-interactive-environment-on-macos-and-linux"></a>Configurar o ambiente interativo do PySpark no macOS e Linux
Se estiver a utilizar **python 3.x**, tem de utilizar o comando **pip3** para os seguintes passos:

1. Certifique-se **Python** e **pip** estão instalados.
 
    ![Versão de pip do Python](./media/set-up-pyspark-interactive-environment/check-python-pip-version.png)

2.  Instale o Jupyter.
    ```
    sudo pip install jupyter
    ```
   Poderá ver a seguinte mensagem de erro no Linux e macOS:

   ![Erro 1](./media/set-up-pyspark-interactive-environment/error1.png)

   ```Resolve:
    sudo pip uninstall asyncio
    sudo pip install trollies
    ```

3. Instale **libkrb5 dev** (para Linux). Poderá ver a seguinte mensagem de erro:

   ![Erro 2](./media/set-up-pyspark-interactive-environment/error2.png)
       
   ```Resolve:
   sudo apt-get install libkrb5-dev 
   ```

3. Instale **sparkmagic**.
   ```
   sudo pip install sparkmagic
   ```

4. Certifique-se de que **ipywidgets** se encontra corretamente instalado ao executar o seguinte:
   ```
   sudo jupyter nbextension enable --py --sys-prefix widgetsnbextension
   ```
   ![Instalar os kernels do wrapper](./media/set-up-pyspark-interactive-environment/ipywidget-enable.png)
 

5. Instale os kernels de wrapper. Execute **pip Mostrar sparkmagic**. A saída mostra o caminho para o **sparkmagic** instalação. 

    ![localização de sparkmagic](./media/set-up-pyspark-interactive-environment/sparkmagic-location.png)
   
6. Vá para o local e, em seguida, execute:

   ```Python2
   sudo jupyter-kernelspec install sparkmagic/kernels/pysparkkernel   
   ```
   ```Python3
   sudo jupyter-kernelspec install sparkmagic/kernels/pyspark3kernel
   ```

   ![instalação de kernelspec do jupyter](./media/set-up-pyspark-interactive-environment/jupyter-kernelspec-install.png)
7. Verificar o estado de instalação.

    ```
    jupyter-kernelspec list
    ```
    ![lista de kernelspec do jupyter](./media/set-up-pyspark-interactive-environment/jupyter-kernelspec-list.png)

    Para kernels disponíveis: 
    - **python2** e **pysparkkernel** correspondem aos **python 2.x**. 
    - **python3** e **pyspark3kernel** correspondem aos **python 3.x**. 

8. Reinicie o VS Code e, em seguida, voltar para o editor de script que está a executar **HDInsight: interativo do PySpark**.

## <a name="next-steps"></a>Passos Seguintes

### <a name="demo"></a>Demonstração
* HDInsight para o VS Code: [vídeo](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Ferramentas e extensões
* [Utilize a ferramenta do Azure HDInsight para Visual Studio Code](hdinsight-for-vscode.md)
* [Utilizar o Azure Toolkit para IntelliJ para criar e submeter aplicações do Spark Scala](spark/apache-spark-intellij-tool-plugin.md)
* [Utilizar o Azure Toolkit para IntelliJ para depurar aplicações do Spark remotamente através de SSH](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Utilizar o Azure Toolkit para IntelliJ para depurar aplicações do Spark remotamente através de VPN](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Utilizar ferramentas do HDInsight no Azure Toolkit para Eclipse para criar Spark aplicações](spark/apache-spark-eclipse-tool-plugin.md)
* [Utilizar ferramentas do HDInsight para IntelliJ com a Sandbox da Hortonworks](hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Utilizar blocos de notas do Zeppelin com um cluster do Spark no HDInsight](spark/apache-spark-zeppelin-notebook.md)
* [Kernels disponíveis para o bloco de notas do Jupyter no cluster do Spark para o HDInsight](spark/apache-spark-jupyter-notebook-kernels.md)
* [Utilizar pacotes externos com blocos de notas do Jupyter](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalar o Jupyter no computador e ligar a um cluster do Spark do HDInsight](spark/apache-spark-jupyter-notebook-install-locally.md)
* [Visualizar dados do Hive com o Microsoft Power BI no Azure HDInsight](hadoop/apache-hadoop-connect-hive-power-bi.md)
* [Utilizar o Zeppelin para executar consultas do Hive no HDInsight do Azure ](hdinsight-connect-hive-zeppelin.md)
