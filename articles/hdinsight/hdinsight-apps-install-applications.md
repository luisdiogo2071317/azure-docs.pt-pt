---
title: Instalar aplicações do Hadoop de terceiros no Azure HDInsight
description: Saiba como instalar aplicações do Hadoop de terceiros no Azure HDInsight.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: jasonh
ms.openlocfilehash: c355d1c4021d8d0877dea216e0c11002b70595e4
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43105223"
---
# <a name="install-third-party-hadoop-applications-on-azure-hdinsight"></a>Instalar aplicações do Hadoop de terceiros no Azure HDInsight

Saiba como instalar uma aplicação do Hadoop de terceiros no Azure HDInsight. Para obter instruções sobre como instalar a sua própria aplicação, veja [Instalar aplicações do HDInsight personalizadas](hdinsight-apps-install-custom-applications.md).

Uma aplicação do HDInsight é um aplicativo que os utilizadores podem instalar num cluster do HDInsight. Estas aplicações podem ser desenvolvidas pela Microsoft, por fornecedores independentes de software (ISV) ou por si.  

A lista seguinte mostra as aplicações publicadas:

* **Plataforma de informações de AtScale** transforma o seu cluster do HDInsight num servidor OLAP de escalamento horizontal. O aplicativo permite que consulta milhares de milhões de linhas de dados de forma interativa usando as ferramentas de BI do Microsoft Excel, Power BI, Tableau Software para QlikView.
* **O cask CDAP para HDInsight** fornece a primeira plataforma de integração unificada para macrodados que reduz o tempo de produção de aplicações de dados de data lakes em 80%. Esta aplicação apenas suporta clusters Standard HBase 3.4.
* **DATAIKU DDS no HDInsight** permite aos profissionais de dados para criar protótipos, criar e implementar serviços altamente específicos que transformam dados não processados em predições de negócio com impacto.
* **H2O Artificial Intelligence para o HDInsight (Beta)** Sparkling Water da H2O oferece suporte aos seguintes algoritmos distribuídos: GLM, Bayes Ingênua, distribuídas Random floresta, máquina aumentam a gradação, redes Neurais profundas, K-means, o PCA, de aprendizagem profunda Modelos de classificação baixos generalizados, deteção de anomalias e Autoencoders.
* **O Kyligence Analytics Platform** Kyligence Analytics Platform (KAP) é um armazém de dados empresariais com tecnologia Apache Kylin e o Apache Hadoop; fornece frações de segundos consultar latência num conjunto de dados de grande escala e simplifica a análise de dados do os utilizadores empresariais e os analistas. 
* **Preparação de dados de autoatendimento a Adaptive**
* **Servidor de tarefas do Spark para o Executor do Spark de KNIME** servidor de tarefas do Spark para o Executor do Spark de KNIME é utilizada para ligar a plataforma de análise de KNIME a clusters do HDInsight.
* O **Recoletor de dados de Streamsets para o HDnsight** fornece um ambiente de desenvolvimento integrado com todas as funcionalidades (IDE) que lhe permite estruturar, testar, implementa e gerir qualquer para qualquer pipelines de ingestão que misturam fluxo e lote de dados e incluem uma variedade de transformações no fluxo — tudo isto sem ter de escrever código personalizado. 
* **[Trifacta](http://www.trifacta.com/)**  permite que os engenheiros de dados e analistas explorar e preparar os dados de diversos dos dias de hoje com a utilização de machine learning para fornecer uma experiência de usuário de progresso, o fluxo de trabalho e a arquitetura de forma mais eficiente.
* **O WANdisco Fusion HDI aplicação** permite conectividade contínua de consistente a dados à medida que muda independentemente de onde está localizado. Ele fornece acesso aos seus dados em qualquer altura e em qualquer lugar com nenhum tempo de inatividade e sem interrupção.

As instruções fornecidas neste artigo utilizam o portal do Azure. Também pode exportar o modelo do Azure Resource Manager a partir do portal ou obter uma cópia do modelo do Resouce Manager junto de fornecedores e utilizar o Azure PowerShell e a CLI do Azure para implementar o modelo.  Ver [criar clusters Hadoop no HDInsight com modelos do Resource Manager](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

## <a name="prerequisites"></a>Pré-requisitos
Se pretender instalar aplicações do HDInsight num cluster do HDInsight existente, tem de ter um cluster do HDInsight. Para criar um, consulte [Criar clusters](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster). Também pode instalar aplicações do HDInsight ao criar um cluster do HDInsight.

## <a name="install-applications-to-existing-clusters"></a>Instalar aplicações em clusters existentes
O procedimento seguinte mostra-lhe como instalar aplicações do HDInsight num cluster HDInsight existente.

**Instalar uma aplicação do HDInsight**

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Clique em **Clusters do HDInsight** no menu à esquerda.
3. Clique num cluster HDInsight.  Se não tiver um, tem de criá-lo.  Veja [Create clusters (Criar clusters)](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster).
4. Clique em **Aplicações** na categoria **Configurações**. Pode ver uma lista de aplicações instaladas. Se não consegue encontrar Aplicações, significa que não existe nenhuma aplicação para esta versão do cluster do HDInsight.
   
    ![menu do portal das aplicações do HDInsight](./media/hdinsight-apps-install-applications/hdinsight-apps-portal-menu.png)
5. Clique em **adicionar** no menu. Deverá ver uma lista de aplicações do HDInsight existentes.
   
    ![aplicações do HDInsight disponíveis](./media/hdinsight-apps-install-applications/hdinsight-apps-list.png)
6. Clique das aplicações disponíveis e, em seguida, siga as instruções para aceitar os termos legais.

Pode verificar o estado da instalação nas notificações do portal (clique no ícone de sino na parte superior do portal). Depois da aplicação está instalada, a aplicação é apresentada na lista de aplicações instaladas.

## <a name="install-applications-during-cluster-creation"></a>Instalar aplicações durante a criação do cluster
Tem a opção de instalar aplicações do HDInsight quando cria um cluster. Durante o processo, as aplicações do HDInsight são instaladas depois de o cluster ser criado e estar no estado Em Execução. Para instalar aplicações durante a criação do cluster no portal do Azure, utilize a opção – personalizado – em vez da predefinição – rápido criar – opção.

## <a name="list-installed-hdinsight-apps-and-properties"></a>Apresentar as aplicações do HDInsight instaladas e propriedades
O portal mostra uma lista das aplicações do HDInsight instaladas relativas a um cluster e as propriedades de cada uma dessas aplicações.

**Aplicações do HDInsight e apresentar as propriedades**

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Clique em **Clusters do HDInsight** no menu à esquerda. 
3. Clique num cluster HDInsight.
4. Partir **definições**, clique em **aplicativos** sob o **configuração** categoria. As aplicações instaladas são listadas à direita. 
   
    ![aplicações instaladas do HDInsight](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps-with-apps.png)
5. Clique numa das aplicações instaladas para mostrar a propriedade. As listas de propriedades:
   
   * Nome da aplicação: o nome da aplicação.
   * Estado: o estado da aplicação. 
   * Página da Web: O URL da aplicação web que implementou no nó de extremidade. A credencial é a mesma que as credenciais de utilizador HTTP que configurou para o cluster.
   * Ponto final de HTTP: a credencial é a mesma que as credenciais de utilizador HTTP que configurou para o cluster. 
   * Ponto final de SSH: pode utilizar o SSH para ligar ao nó de extremidade. As credenciais de SSH são as mesmas que as credenciais de utilizador de SSH que configurou para o cluster. Para obter informações, veja [Use SSH with HDInsight (Utilizar SSH com o HDInsight)](hdinsight-hadoop-linux-use-ssh-unix.md).
6. Para eliminar uma aplicação, clique com o botão direito do aplicativo e, em seguida, clique em **eliminar** no menu de contexto.

## <a name="connect-to-the-edge-node"></a>Ligar ao nó de extremidade
Pode ligar ao nó de extremidade com HTTP e SSH. Pode encontrar as informações relativas ao ponto final no [portal](#list-installed-hdinsight-apps-and-properties). Para obter informações, veja [Use SSH with HDInsight (Utilizar SSH com o HDInsight)](hdinsight-hadoop-linux-use-ssh-unix.md).

As credenciais do ponto final de HTTP são as credenciais de utilizador HTTP que configurou para o cluster HDInsight; as credenciais do ponto final de SSH são as credenciais de SSH que configurou para o cluster HDInsight.

## <a name="troubleshoot"></a>Resolução de problemas
Veja [Troubleshoot the installation (Resolver problemas de instalação)](hdinsight-apps-install-custom-applications.md#troubleshoot-the-installation).

## <a name="next-steps"></a>Passos Seguintes
* [Instalar aplicações do HDInsight personalizadas](hdinsight-apps-install-custom-applications.md): Saiba como implementar uma aplicação HDInsight não publicada para o HDInsight.
* [Publicar aplicações do HDInsight](hdinsight-apps-publish-applications.md): Saiba como publicar as suas aplicações do HDInsight personalizadas no Azure Marketplace.
* [MSDN: Instalar uma aplicação do HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): Saiba como definir aplicações do HDInsight.
* [Personalizar clusters do HDInsight baseados em Linux através da Ação de Script](hdinsight-hadoop-customize-cluster-linux.md): saiba como utilizar a Ação de Script para instalar outras aplicações.
* [Create Linux-based Hadoop clusters in HDInsight using Resource Manager templates (Criar clusters Hadoop baseados em Linux no HDInsight com modelos do Resource Manager)](hdinsight-hadoop-create-linux-clusters-arm-templates.md): saiba como chamar modelos do Resource Manager para criar clusters HDInsight.
* [Utilizar nós de extremidade vazios no HDInsight](hdinsight-apps-use-edge-node.md): saiba como utilizar um nó de extremidade vazio para aceder ao cluster do HDInsight, testar aplicações do HDInsight e alojar aplicações do HDInsight.

