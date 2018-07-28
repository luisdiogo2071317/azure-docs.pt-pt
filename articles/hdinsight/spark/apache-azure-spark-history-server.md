---
title: Utilização alargada de servidor de histórico do Spark para depurar e diagnosticar aplicações do Spark - Azure HDInsight | Documentos da Microsoft
description: Utilização alargada de servidor de histórico do Spark para depurar e diagnosticar aplicações do Spark - Azure HDInsight.
services: hdinsight
author: jejiang
manager: DJ
editor: Jenny Jiang
tags: azure-portal
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: ''
ms.topic: article
ms.date: 07/12/2018
ms.author: jejiang
ms.openlocfilehash: 61e857b01f75cb30e3d213703bd8ee8014d902d6
ms.sourcegitcommit: cfff72e240193b5a802532de12651162c31778b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39309865"
---
# <a name="use-extended-spark-history-server-to-debug-and-diagnose-spark-applications"></a>Utilizar o servidor de histórico expandido do Spark para depurar e diagnosticar aplicações do Spark

Este artigo fornece orientações sobre como utilizar estendido de servidor de histórico do Spark para depurar e diagnosticar aplicações do Spark em execução e concluídas. A extensão inclui atualmente separador de dados e o separador de gráfico. No separador de dados, os usuários possam marcar os dados de entrada e saídos da tarefa do Spark. No separador de gráfico, os utilizadores podem verificar o fluxo de dados e reproduzir gráfico da tarefa.

## <a name="open-the-spark-history-server"></a>Abra o servidor de histórico do Spark

Servidor de histórico do Spark é a IU da web para aplicações do Spark em execução e concluídas. 

### <a name="to-open-the-spark-history-server-web-ui-from-azure-portal"></a>Para abrir a interface do Usuário do Spark histórico de servidor Web a partir do portal do Azure

1. Partir do [portal do Azure](https://portal.azure.com/), abra o cluster do Spark. Para obter mais informações, consulte [listar e Mostrar clusters](../hdinsight-administer-use-portal-linux.md#list-and-show-clusters).
2. Partir **ligações rápidas**, clique em **Dashboard do Cluster**e, em seguida, clique em **servidor de histórico do Spark**. Quando lhe for pedido, introduza as credenciais de administrador para o cluster do Spark. 

    ![Servidor de histórico do spark](./media/apache-azure-spark-history-server/launch-history-server.png "Server do histórico do Spark")

### <a name="to-open-the-spark-history-server-web-ui-by-url"></a>Para abrir a IU da Web de servidor de histórico de Spark por URL
Abra do servidor de histórico do Spark ao navegar para o URL seguinte, substitua <ClusterName> com o nome de cluster do Spark de cliente.

   ```
   https://<ClusterName>.azurehdinsight.net/sparkhistory
   ```

A web do servidor de histórico do Spark a interface do Usuário é semelhante a:

![Servidor de histórico do Spark do HDInsight](./media/apache-azure-spark-history-server/hdinsight-spark-history-server.png)


## <a name="open-the-data-tab-from-spark-history-server"></a>Abra o separador de dados do servidor de histórico do Spark
Selecione o ID da tarefa, em seguida, clique em **dados** no menu da ferramenta para obter a vista de dados.

+ Verifique os **entradas**, **saídas**, e **operações de tabela** ao selecionar os separadores em separado.

    ![Separadores de dados](./media/apache-azure-spark-history-server/sparkui-data-tabs.png)

+ Copiar todas as linhas ao clicar no botão **cópia**.

    ![Cópia de dados](./media/apache-azure-spark-history-server/sparkui-data-copy.png)

+ Guarde todos os dados como ficheiro CSV, ao clicar no botão **csv**.

    ![Dados guardar](./media/apache-azure-spark-history-server/sparkui-data-save.png)

+ Procurar por introduzir palavras-chave no campo **pesquisa**, o resultado da pesquisa serão apresentados imediatamente.

    ![Pesquisa de dados](./media/apache-azure-spark-history-server/sparkui-data-search.png)

+ Clique no cabeçalho da coluna para ordenar a tabela, clique no sinal de expandir uma linha para mostrar mais detalhes ou clique o sinal de subtração para recolher uma linha.

    ![Tabela de dados](./media/apache-azure-spark-history-server/sparkui-data-table.png)

+ Baixar arquivo único ao clicar no botão **transferir parcial** que colocar no lado direito, em seguida, o ficheiro selecionado será transferido para o local, se o ficheiro não existe mais, ele será aberto um separador novo para mostrar as mensagens de erro.

    ![Linha de transferência de dados](./media/apache-azure-spark-history-server/sparkui-data-download-row.png)

+ Copie o caminho completo ou o caminho relativo ao selecionar o **caminho completo de cópia**, **caminho relativo de cópia** que expande a partir do menu de download. Para ficheiros de armazenamento do azure data lake, **abrir no Explorador de armazenamento do Azure** iniciará o Explorador de armazenamento do Azure e localize a pasta quando iniciar sessão.

    ![Caminho de cópia de dados](./media/apache-azure-spark-history-server/sparkui-data-copy-path.png)

+ Clique em que o número abaixo da tabela para navegar páginas quando demasiado muitas linhas para exibir numa página. 

    ![Página de dados](./media/apache-azure-spark-history-server/sparkui-data-page.png)

+ Paire o rato sobre o ponto de interrogação junto ao dados a mostrar a descrição, ou clique no ponto de interrogação para obter mais informações.

    ![Dados de mais informações](./media/apache-azure-spark-history-server/sparkui-data-more-info.png)

+ Enviar comentários com problemas ao clicar **nos fornecer comentários**.

    ![comentários de gráfico](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)


## <a name="open-the-graph-tab-from-spark-history-server"></a>Abra o separador de gráfico do servidor de histórico do Spark
Selecione o ID da tarefa, em seguida, clique em **Graph** no menu da ferramenta para obter a vista de gráfico da tarefa.

+ Verificar a descrição geral do seu trabalho gráfico da tarefa gerado. 

+ Por predefinição, esta operação irá mostrar todas as tarefas e podem ser filtrado por **ID da tarefa**.

    ![ID da tarefa de gráfico](./media/apache-azure-spark-history-server/sparkui-graph-jobid.png)

+ Por predefinição, **progresso** é selecionado, utilizador foi possível verificar o fluxo de dados ao selecionar **leitura/Written** na lista pendente de **apresentar**.

    ![exibição de gráfico](./media/apache-azure-spark-history-server/sparkui-graph-display.png)

    A exibição de nó de gráfico cor que mostra o mapa térmico.

    ![mapa térmico de gráfico](./media/apache-azure-spark-history-server/sparkui-graph-heatmap.png)

+ Reproduzir a tarefa ao clicar o **reprodução** botão e parar em qualquer altura ao clicar no botão stop. O ecrã de tarefa na cor para mostrar o estado diferente quando reprodução:

    + Verde para foi concluída com êxito: A tarefa foi concluída com êxito.
    + Cor de laranja para repetida: instâncias das tarefas que falhou, mas não afetam o resultado final da tarefa. Estas tarefas tinham duplicar ou repita instâncias que podem ter êxito mais tarde.
    + Vermelho para falhou: A tarefa falhou.
    + Azul para execução: A tarefa é executada.
    + Branco para ignorado ou aguardando: A tarefa está a aguardar para ser executado, ou o estágio foi ignorada.

    ![mostruário de cores do gráfico, em execução](./media/apache-azure-spark-history-server/sparkui-graph-color-running.png)
 
    ![mostruário de cores do gráfico, falhado](./media/apache-azure-spark-history-server/sparkui-graph-color-failed.png)
 
    > [!NOTE]
    > Reprodução de cada tarefa é permitida. Quando uma tarefa não tem qualquer fase ou ainda não tiver concluída, a reprodução não é suportada.


+ Rato se desloca para aplicar zoom de entrada/saída gráfico da tarefa, ou clique em **Zoom para se ajustarem** para torná-lo ajustar ao ecrã.
 
    ![zoom de gráfico para caber](./media/apache-azure-spark-history-server/sparkui-graph-zoom2fit.png)

+ Coloque o cursor no nó de gráfico para ver a dica de ferramenta quando há falha de tarefas e clique no estágio para abrir a página de fase.

    ![Descrição do gráfico](./media/apache-azure-spark-history-server/sparkui-graph-tooltip.png)

+ O nó de gráfico da tarefa mostra as seguintes informações de cada estágio:
    + ID.
    + Nome ou descrição.
    + Número total de tarefas.
    + Dados lidos: a soma de tamanho de entrada e shuffle tamanho de leitura.
    + Escrita de dados: a soma de tamanho de saída e shuffle escrever tamanho.
    + Tempo de execução: o tempo entre a hora de início da primeira tentativa e tempo de conclusão da última tentativa.
    + Contagem de linhas: a soma dos registos de entrada, saída de registos, misturar os registos de leitura e misturar os registos de escrita.
    + Curso.

    > [!NOTE]
    > Por predefinição, o nó de gráfico de tarefa irá apresentar informações da última tentativa de cada estágio (exceto para o estágio de tempo de execução), mas durante o gráfico de reprodução nó mostrará as informações de cada tentativa.

    > [!NOTE]
    > Para o tamanho de dados de leitura e escrita, utilize 1 MB = 1000 KB = de Bytes 1000 * 1000.

+ Enviar comentários com problemas ao clicar **nos fornecer comentários**.

    ![comentários de gráfico](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)


## <a name="faq"></a>FAQ

### <a name="1-revert-to-community-version"></a>1. Reverter para a versão de Comunidade

Para reverter para a versão de Comunidade, siga os passos abaixo:

1. Cluster aberto no Ambari. Clique em **Spark2** no painel esquerdo.
2. Clique em **configurações** separador.
3. Expanda o grupo de **personalizado spark2-predefinições**.
4. Clique em **Adicionar propriedade**, adicione **spark.ui.enhancement.enabled=false**, guardar.
5. A propriedade define como **false** agora.
6. Clique em **guardar** para guardar a configuração.

    ![Desativa a funcionalidade](./media/apache-azure-spark-history-server/sparkui-turn-off.png)

7. Clique em **Spark2** no restante painel, em **resumo** separador, clique em **Spark2 histórico servidor**.

    ![Reinicie o servidor1](./media/apache-azure-spark-history-server/sparkui-restart-1.png) 

8. Reinicie o servidor de histórico clicando **reinicie** de **Spark2 histórico servidor**.

    ![Reinicie o Servidor2](./media/apache-azure-spark-history-server/sparkui-restart-2.png)  

9. Atualizar a web de servidor de histórico do Spark da interface do Usuário, ele vai ser revertido para a versão de Comunidade.

### <a name="2-upload-history-server-event"></a>2. Carregar o evento de servidor de histórico

Caso se depare com erro de servidor de histórico, siga os passos para fornecer o evento:
1. Transferir eventos ao clicar em **transferir** no servidor de histórico da interface do Usuário da web.

    ![Transferir eventos](./media/apache-azure-spark-history-server/sparkui-download-event.png)

2. Clique em **nos fornecer comentários** partir do separador de dados/gráfico.

    ![comentários de gráfico](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)

3. Forneça o título e descrição do erro, arraste o ficheiro zip para o campo de edição, em seguida, clique em **submeter novo problema**.

    ![problema de ficheiro](./media/apache-azure-spark-history-server/sparkui-file-issue.png)


### <a name="3-upgrade-jar-file-for-hotfix-scenario"></a>3. Atualizar o ficheiro jar para o cenário de correção

Se quiser atualizar com correções, utilize o script abaixo que irá atualizar enhancement.jar* do spark.

**upgrade_spark_enhancement.SH**:

   ```bash
    #!/usr/bin/env bash

    # Copyright (C) Microsoft Corporation. All rights reserved.

    # Arguments:
    # $1 Enhancement jar path

    if [ "$#" -ne 1 ]; then
        >&2 echo "Please provide the upgrade jar path."
        exit 1
    fi

    install_jar() {
        tmp_jar_path="/tmp/spark-enhancement-hotfix-$( date +%s )"

        if wget -O "$tmp_jar_path" "$2"; then
            for FILE in "$1"/spark-enhancement*.jar
            do
                back_up_path="$FILE.original.$( date +%s )"
                echo "Back up $FILE to $back_up_path"
                mv "$FILE" "$back_up_path"
                echo "Copy the hotfix jar file from $tmp_jar_path   to $FILE"
                cp "$tmp_jar_path" "$FILE"

                "Hotfix done."
                break
            done
        else    
            >&2 echo "Download jar file failed."
            exit 1
        fi
    }

    jars_folder="/usr/hdp/current/spark2-client/jars"
    jar_path=$1

    if ls ${jars_folder}/spark-enhancement*.jar 1>/dev/null 2>&1;   then
        install_jar "$jars_folder" "$jar_path"
    else
        >&2 echo "There is no target jar on this node. Exit with no action."
        exit 0
    fi
   ```

**Utilização**: 

`upgrade_spark_enhancement.sh https://${jar_path}`

**Exemplo**:

`upgrade_spark_enhancement.sh https://${account_name}.blob.core.windows.net/packages/jars/spark-enhancement-${version}.tgz` 

**Para usar o arquivo de bash no portal do Azure**

1. Inicie [Portal do Azure](https://ms.portal.azure.com)e selecione o cluster.
2. Clique em **ações de Script**, em seguida, **submeter novo**. Concluir o **submeter ação de script** formam, em seguida, clique em **criar** botão.
    
    + **Tipo de script**: selecione **personalizado**.
    + **Nome**: Especifique um nome de script.
    + **URI do script de bash**: carregar o ficheiro de bash para cluster privado, em seguida, copie o URL aqui. Em alternativa, utilize o URI fornecido.
    
   ```upgrade_spark_enhancement
    https://hdinsighttoolingstorage.blob.core.windows.net/shsscriptactions/upgrade_spark_enhancement.sh
   ```

    + Para verificar **Head** e **trabalho**.
    + **Parâmetros**: definir a siga de parâmetros a utilização de bash.

    ![carregar o registo ou a correção de atualização](./media/apache-azure-spark-history-server/sparkui-upload2.png)


## <a name="known-issue"></a>Problema conhecido

1.  Atualmente, só funciona para o cluster do Spark 2.3.

2.  Dados de entrada/saída com o RDD não serão apresentados no separador de dados.

## <a name="next-steps"></a>Passos Seguintes

* [Gerir os recursos de um cluster do Spark no HDInsight](apache-spark-resource-manager.md)
* [Configurar as definições do Spark](apache-spark-settings.md)


## <a name="contact-us"></a>Contacte-nos

Se tiver algum comentário ou se tiver quaisquer outros problemas ao utilizar esta ferramenta, enviar um e-mail para ([hdivstool@microsoft.com](mailto:hdivstool@microsoft.com)).