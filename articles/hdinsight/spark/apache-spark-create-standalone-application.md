---
title: 'Tutorial: Criar uma aplicação Scala Maven para Spark no HDInsight com IntelliJ | Microsoft Docs'
description: Crie uma aplicação do Spark escrita em Scala tendo como sistema de compilação o Apache Maven e um arquétipo Maven existente para Scala fornecido pelo IntelliJ IDEA.
services: hdinsight
documentationcenter: ''
author: mumian
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.assetid: b2467a40-a340-4b80-bb00-f2c3339db57b
ms.service: hdinsight
ms.custom: hdinsightactive,mvc
ms.devlang: na
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: jgao
ms.openlocfilehash: ed90e50167f7e86c464b1571b91dc27435437e9b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34627421"
---
# <a name="tutorial-create-a-scala-maven-application-for-spark-in-hdinsight-using-intellij"></a>Tutorial: Criar uma aplicação Scala Maven para Spark no HDInsight com IntelliJ

Neste tutorial, vai aprender a criar uma aplicação do Spark escrita em Scala com o Maven e IntelliJ IDEA. O artigo utiliza o Apache Maven como o sistema de compilação e começa com um arquétipo Maven existente para Scala fornecido pelo IntelliJ IDEA.  Criar uma aplicação Scala no IntelliJ IDEA envolve os seguintes passos:

* Utilizar o Maven como o sistema de compilação.
* Atualizar o ficheiro Project Object Model (POM) (POM) para resolver dependências do módulo do Spark.
* Escrever a aplicação no Scala.
* Gerar um ficheiro jar que pode ser submetido para clusters do Spark no HDInsight.
* Utilizar o Livy para executar a aplicação no cluster do Spark.

> [!NOTE]
> O HDInsight também disponibiliza uma ferramenta de plug-in para o IntelliJ IDEA para facilitar o processo de criação e submissão de aplicações para um cluster do Spark no HDInsight no Linux. Para obter mais informações, veja [Use HDInsight Tools Plugin for IntelliJ IDEA to create and submit Spark applications](apache-spark-intellij-tool-plugin.md) (Utilizar o Plug-in HDInsight Tools para o IntelliJ IDEA para criar e submeter aplicações do Spark).
> 

Neste tutorial, ficará a saber como:
> [!div class="checklist"]
> * Utilizar o IntelliJ para desenvolver uma aplicação Scala Maven

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.


## <a name="prerequisites"></a>Pré-requisitos

* Um cluster do Apache Spark no HDInsight. Para obter instruções, veja [Criar clusters do Apache Spark no Azure HDInsight](apache-spark-jupyter-spark-sql.md).
* Oracle Java Development kit. Pode instalá-lo a partir [daqui](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* Um IDE Java. Este artigo utiliza o IntelliJ IDEA 18.1.1. Pode instalá-lo a partir [daqui](https://www.jetbrains.com/idea/download/).

## <a name="use-intellij-to-create-application"></a>Utilize o IntelliJ para criar uma aplicação

1. Inicie o IntelliJ IDEA e, em seguida, crie um projeto. Na caixa de diálogo **Novo Projeto**, faça o seguinte: 

   a. Selecione **HDInsight** > **Spark no HDInsight (Scala)**.

   b. Na lista **Ferramenta de compilação**, selecione uma das seguintes opções, de acordo com as suas necessidades:

      * **Maven**, para o suporte de assistente de criação do projeto Scala
      * **SBT**, para gerir as dependências e a complicação do projeto Scala

   ![Caixa de diálogo O Novo Projeto](./media/apache-spark-create-standalone-application/create-hdi-scala-app.png)

2. Selecione **Seguinte**.

3. O assistente de criação do projeto Scala deteta automaticamente se instalou o plug-in do Scala. Selecione **Instalar**.

   ![Verificação do Plug-in do Scala](./media/apache-spark-create-standalone-application/Scala-Plugin-check-Reminder.PNG) 

4. Para transferir o Plug-in do Scala, selecione **OK**. Siga as instruções para reiniciar o IntelliJ. 

   ![A caixa de diálogo de instalação do plug-in do Scala](./media/apache-spark-create-standalone-application/Choose-Scala-Plugin.PNG)

5. Na janela **Novo Projeto**, faça o seguinte:  

    ![Selecionar o SDK do Spark](./media/apache-spark-create-standalone-application/hdi-new-project.png)

   a. Introduza um nome do projeto e localização.

   b. Na lista pendente **SDK do Projeto**, selecione **Java 1.8** para o cluster do Spark 2.x ou selecione **Java 1.7** para o cluster do Spark 1.x.

   c. Na lista pendente **versão do Spark**, o assistente de criação do projeto Scala integra a versão correta para o SDK do Spark e o SDK do Scala. Se a versão do cluster do Spark for anterior à 2.0, selecione **Spark 1.x**. Caso contrário, selecione **Spark2.x**. Este exemplo utiliza o **Spark 2.0.2 (Scala 2.11.8)**.

6. Selecione **Concluir**.

## <a name="install-scala-plugin-for-intellij-idea"></a>Instalar o plug-in do Scala para o IntelliJ IDEA
Para instalar o plug-in do Scala, utilize os seguintes passos:

1. Abra o IntelliJ IDEA.
2. No ecrã de boas-vindas, selecione **Configure** (Configurar) e, em seguida, selecione **Plugins** (Plug-ins).
   
    ![Ativar o plug-in do Scala](./media/apache-spark-create-standalone-application/enable-scala-plugin.png)
3. Selecione **Install JetBrains plugin** (Instalar o plug-in JetBrains), no canto inferior esquerdo. 
4. Na caixa de diálogo **Browse JetBrains Plugins** (Procurar Plug-ins de JetBrains), procure **Scala** e selecione **Install** (Instalar).
   
    ![Instalar o plug-in do Scala](./media/apache-spark-create-standalone-application/install-scala-plugin.png)
5. Após a instalação bem-sucedida do plug-in, tem de reiniciar o IDE.

## <a name="create-a-standalone-scala-project"></a>Criar um projeto de Scala autónomo
1. Abra o IntelliJ IDEA.
2. No menu **File** (Ficheiro), selecione **New > Project** (Novo > Projeto) para criar um projeto novo.
3. Na caixa de diálogo do projeto novo, selecione as escolhas abaixo:
   
    ![Criar projeto do Maven](./media/apache-spark-create-standalone-application/create-maven-project.png)
   
   * Selecione **MVC** como o tipo de projeto.
   * Especifique um **Project SDK** (SDK de Projeto). Selecione **New** (Novo) e navegue para o diretório de instalação do Java, normalmente `C:\Program Files\Java\jdk1.8.0_66`.
   * Selecione a opção **Create from archetype** (Criar a partir de arquétipo).
   * Na lista de arquétipos, selecione **org.scala-tools.archetypes:scala-archetype-simple**. Este arquétipo cria a estrutura de diretório certa e transfere as dependências predefinidas necessárias para escrever programas em Scala.
4. Selecione **Seguinte**.
5. Indique valores relevantes para **GroupId** (ID de Grupo), **ArtifactId** (ID de Artefacto) e **Version** (Versão). São utilizados os seguintes valores neste tutorial:

    - GroupId: com.microsoft.spark.example
    - ArtifactId: SparkSimpleApp
6. Selecione **Seguinte**.
7. Verifique as definições e selecione **Next** (Seguinte).
8. Confirme o nome e a localização do projeto e selecione **Finish** (Concluir).
9. No painel do lado esquerdo, selecione **src > test > scala > com > microsoft > spark > example**, clique com o botão direito do rato em **MySpec** e selecione **Delete** (Eliminar). Não precisa deste ficheiro para a aplicação.
  
10. Nos passos subsequentes, vai atualizar o pom.xml para definir as dependências da aplicação em Scala do Spark. Para que essas dependências sejam transferidas e resolvidas automaticamente, tem de configurar o Maven em conformidade.
   
    ![Configurar o Maven para transferências automáticas](./media/apache-spark-create-standalone-application/configure-maven.png)
   
   1. No menu **File** (Ficheiro), selecione **Settings** (Definições).
   2. Na caixa de diálogo **Settings** (Definições), navegue para **Build, Execution, Deployment** (Compilação, Execução, Implementação)  > **Build Tools** (Ferramentas de Compilação)  > **Maven** > **Importing** (Importação).
   3. Selecione a opção **Import Maven projects automatically** (Importar projetos do Maven automaticamente).
   4. Selecione **Apply** (Aplicar) e **OK**.
11. No painel do lado esquerdo, selecione **src > main > scala > com.microsoft.spark.example** e faça duplo clique em **App** (Aplicação) para abrir App.scala.

12. Substitua o código de exemplo existente pelo seguinte código e guarde as alterações. Este código lê os dados do HVAC.csv (disponível em todos os clusters do Spark no HDInsight), obtém as linhas que só têm um dígito na sexta coluna e escreve a saída para **/HVACOut**, no contentor de armazenamento predefinido do cluster.

        package com.microsoft.spark.example
   
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
   
        /**
          * Test IO to wasb
          */
        object WasbIOTest {
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("WASBIOTest")
            val sc = new SparkContext(conf)
   
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
            //find the rows which have only one digit in the 7th column in the CSV
            val rdd1 = rdd.filter(s => s.split(",")(6).length() == 1)
   
            rdd1.saveAsTextFile("wasb:///HVACout")
          }
        }
13. No painel do lado esquerdo, faça duplo clique em **pom.xml**.
   
   1. Em `<project>\<properties>`, adicione os seguintes segmentos:
      
          <scala.version>2.10.4</scala.version>
          <scala.compat.version>2.10.4</scala.compat.version>
          <scala.binary.version>2.10</scala.binary.version>
   2. Em `<project>\<dependencies>`, adicione os seguintes segmentos:
      
           <dependency>
             <groupId>org.apache.spark</groupId>
             <artifactId>spark-core_${scala.binary.version}</artifactId>
             <version>1.4.1</version>
           </dependency>
      
      Guarde as alterações ao pom.xml.
10. Crie o ficheiro .jar. O IntelliJ IDEA permite a criação do JAR como um artefacto de um projeto. Realize os seguintes passos.
    
    1. No menu **File** (Ficheiro), selecione **Project Structure** (Estrutura do Projeto).
    2. Na caixa de diálogo **Project Structure** (Estrutura do Projeto), selecione **Artifacts** (Artefacto) e selecione o sinal de mais. Na caixa de diálogo de pop-up, selecione **JAR** e **From modules with dependencies** (De módulos com dependências).
       
        ![Criar o JAR](./media/apache-spark-create-standalone-application/create-jar-1.png)
    3. Na caixa de diálogo **Create JAR from Modules** (Criar JAR a partir de módulos), selecione as reticências (![ellipsis](./media/apache-spark-create-standalone-application/ellipsis.png)) em **Main Class** (Classe Principal).
    4. Na caixa de diálogo **Select Main Class** (Selecionar Classe Principal), selecione a classe que aparece por predefinição e, em seguida, selecione **OK**.
       
        ![Criar o JAR](./media/apache-spark-create-standalone-application/create-jar-2.png)
    5. Na caixa de diálogo **Create JAR from Modules** (Criar JAR a partir de Módulos), confirme que a opção **extract to the target JAR** (extrair para o JAR de destino) e selecione **OK**.  Esta definição cria um único JAR com todas as dependências.
       
        ![Criar o JAR](./media/apache-spark-create-standalone-application/create-jar-3.png)
    6. O separador do esquema da saída apresenta uma lista de todos os jars que estão incluídos como parte do projeto do Maven. Pode selecionar e eliminar aqueles em que a aplicação de Scala não tem dependências diretas. Na aplicação que está a criar aqui, pode remover todos menos o último (**SparkSimpleApp compile output**). Selecione os jars a eliminar e, em seguida, selecione o ícone **Delete** (Eliminar).
       
        ![Criar o JAR](./media/apache-spark-create-standalone-application/delete-output-jars.png)
       
        Confirme que a caixa de diálogo **Include in project build** (Incluir na compilação do projeto) está selecionada, o que garante que o jar é criado sempre que o projeto for compilado ou atualizado. selecione **Apply** (Aplicar) e **OK**.
    7. No menu **Build** (Compilar), selecione **Build Artifacts** (Compilar Artefactos) para criar o jar. O jar de saída é criado em **\out\artifacts**.
       
        ![Criar o JAR](./media/apache-spark-create-standalone-application/output.png)

## <a name="run-the-application-on-the-spark-cluster"></a>Executar a aplicação no cluster do Spark
Para executar a aplicação no cluster, pode utilizar as seguintes abordagens:

* **Copiar o jar da aplicação para o blob de armazenamento do Azure** associado ao cluster. Para o fazer, pode utilizar [**AzCopy**](../../storage/common/storage-use-azcopy.md), um utilitário de linha de comandos. Tem à sua disposição muitos outros clientes para carregar dados. Pode saber mais sobre esses clientes em [Upload data for Hadoop jobs in HDInsight](../hdinsight-upload-data.md) (Carregar dados para trabalhos do Hadoop no HDInsight).
* **Utilizar o Livy para submeter um trabalho de aplicação remotamente** para o cluster do Spark. Os clusters do Spark no HDInsight incluem o Livy que expõe os pontos finais de REST para submeter remotamente trabalhos do Spark. Para obter mais informações, veja [Submit Spark jobs remotely using Livy with Spark clusters on HDInsight](apache-spark-livy-rest-interface.md) (Utilizar o Livy para submeter trabalhos do Spark remotamente com clusters do Spark no HDInsight).

## <a name="next-step"></a>Passo seguinte

Neste artigo, aprendeu a criar uma aplicação Scala do Spark. Avance para o próximo artigo para saber como executar esta aplicação num cluster do HDInsight Spark com Livy.

> [!div class="nextstepaction"]
>[Executar tarefas remotamente num cluster do Spark com o Livy](./apache-spark-livy-rest-interface.md)

