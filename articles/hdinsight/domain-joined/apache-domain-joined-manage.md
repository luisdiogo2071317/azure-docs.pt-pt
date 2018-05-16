---
title: Gerir clusters do HDInsight associados a um domínio - Azure
description: Saiba como gerir clusters do HDInsight associados a um domínio
services: hdinsight
author: omidm1
manager: jhubbard
editor: cgronlun
ms.assetid: 6ebc4d2f-2f6a-4e1e-ab6d-af4db6b4c87c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: omidm
ms.openlocfilehash: 31b58176a75918cf68cda695f27216031670493e
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="manage-domain-joined-hdinsight-clusters"></a>Gerir clusters do HDInsight associados a um domínio
Saiba mais utilizadores e funções no HDInsight associados a um domínio e como gerir clusters do HDInsight associados a um domínio.

## <a name="use-vscode-to-link-to-domain-joined-cluster"></a>Utilizar o VSCode para ligar a um cluster associado a um domínio

Pode ligar um cluster normal utilizando o nome de utilizador do Ambari gerida, também ligar um cluster do hadoop de segurança utilizando o nome de utilizador de domínio (tais como: user1@contoso.com).
1. Abra a paleta de comando selecionando **CTRL + SHIFT + P**e, em seguida, introduza **HDInsight: ligar um cluster**.

   ![comando de cluster de ligação](./media/apache-domain-joined-manage/link-cluster-command.png)

2. Introduza HDInsight URL de cluster-entrada de nome de utilizador -> > palavra-passe de entrada -> selecione o tipo de cluster ->-mostra informações de êxito se passado a verificação.
   
   ![caixa de diálogo de cluster de ligação](./media/apache-domain-joined-manage/link-cluster-process.png)

   > [!NOTE]
   > O nome de utilizador ligado e a palavra-passe são utilizados se o cluster tem sessão iniciada na subscrição do Azure tanto ligado um cluster. 
   
3. Pode ver um cluster de ligado utilizando o comando **cluster lista**. Agora pode submeter um script para este cluster ligado.

   ![cluster ligado](./media/apache-domain-joined-manage/linked-cluster.png)

4. Pode também pode desassociar um cluster por inputting **HDInsight: desassociar um cluster** de paleta de comando.

## <a name="use-intellij-to-link-to-domain-joined-cluster"></a>Utilizar o IntelliJ para ligar a um cluster associado a um domínio

Pode ligar um cluster normal utilizando o nome de utilizador do Ambari gerida, também ligar um cluster do hadoop de segurança utilizando o nome de utilizador de domínio (tais como: user1@contoso.com). 
1. Clique em **ligar um cluster** de **Explorador do Azure**.

   ![menu de contexto do cluster de ligação](./media/apache-domain-joined-manage/link-a-cluster-context-menu.png)

2. Introduza **nome do Cluster**, **nome de utilizador** e **palavra-passe**. Tem de verificar o nome de utilizador e palavra-passe se foi a falha de autenticação. Opcionalmente, adicione a conta do Storage, chave de armazenamento, em seguida, selecione um contentor do contentor de armazenamento. Informações de armazenamento estão no Explorador de armazenamento na árvore da esquerda do
   
   ![caixa de diálogo de cluster de ligação](./media/apache-domain-joined-manage/link-a-cluster-dialog.png)

   > [!NOTE]
   > Utilizamos a chave de armazenamento ligado, o nome de utilizador e a palavra-passe se o cluster tem sessão iniciada na subscrição do Azure tanto ligado um cluster.
   > ![Explorador de armazenamento no IntelliJ](./media/apache-domain-joined-manage/storage-explorer-in-IntelliJ.png)

   
3. Pode ver um cluster de ligado no **HDInsight** nós se as informações de entrada estão corretas. Agora pode submeter uma aplicação para este cluster ligado.

   ![cluster ligado](./media/apache-domain-joined-manage/linked-cluster-intellij.png)

4. Também pode desassociar um cluster de **Explorador do Azure**.
   
   ![Desassociar de cluster](./media/apache-domain-joined-manage/unlink.png)

## <a name="use-eclipse-to-link-to-domain-joined-cluster"></a>Utilizar o Eclipse para ligar a um cluster associado a um domínio

Pode ligar um cluster normal utilizando o nome de utilizador do Ambari gerida, também ligar um cluster do hadoop de segurança utilizando o nome de utilizador de domínio (tais como: user1@contoso.com).
1. Clique em **ligar um cluster** de **Explorador do Azure**.

   ![menu de contexto do cluster de ligação](./media/apache-domain-joined-manage/link-a-cluster-context-menu.png)

2. Introduza **nome do Cluster**, **nome de utilizador** e **palavra-passe**, em seguida, clique no botão OK para ligar o cluster. Opcionalmente, introduza a conta do Storage, chave de armazenamento e, em seguida, selecione o contentor de armazenamento para o Explorador de armazenamento a trabalhar na vista de árvore à esquerda
   
   ![caixa de diálogo de cluster de ligação](./media/apache-domain-joined-manage/link-cluster-dialog.png)
   
   > [!NOTE]
   > Utilizamos a chave de armazenamento ligado, o nome de utilizador e a palavra-passe se o cluster tem sessão iniciada na subscrição do Azure tanto ligado um cluster.
   > ![Explorador de armazenamento no Eclipse](./media/apache-domain-joined-manage/storage-explorer-in-Eclipse.png)

3. Pode ver um cluster de ligado no **HDInsight** nó depois de clicar no botão OK, se as informações de entrada estão corretas. Agora pode submeter uma aplicação para este cluster ligado.

   ![cluster ligado](./media/apache-domain-joined-manage/linked-cluster-intellij.png)

4. Também pode desassociar um cluster de **Explorador do Azure**.
   
   ![Desassociar de cluster](./media/apache-domain-joined-manage/unlink.png)

## <a name="access-the-clusters-with-enterprise-security-package"></a>Aceder os clusters com o pacote de segurança da empresa.

Pacote de segurança da empresa (anteriormente conhecido como o HDInsight Premium) fornece acesso de vários utilizador para o cluster, onde a autenticação é efetuada pelo Active Directory e a autorização por Apache Ranger e ACLs de armazenamento (ADLS ACLs). Autorização proporciona limites segurados entre vários utilizadores e permite que apenas com privilégios que os utilizadores tenham acesso aos dados com as políticas de autorização.

Isolamento de utilizador e de segurança são importantes para um cluster do HDInsight com o pacote de segurança da empresa. Para cumprir estes requisitos, o acesso SSH para o cluster com o pacote de segurança da empresa é bloqueado. A tabela seguinte mostra os métodos de acesso recomendada para cada tipo de cluster:

|Carga de trabalho|Cenário|Método de acesso|
|--------|--------|-------------|
|Hadoop|Ramo de registo – tarefas/consultas interativas |<ul><li>[Beeline](#beeline)</li><li>[Vista do Hive](../hadoop/apache-hadoop-use-hive-ambari-view.md)</li><li>[ODBC/JDBC – Power BI](../hadoop/apache-hadoop-connect-hive-power-bi.md)</li><li>[Ferramentas do Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)</li></ul>|
|Spark|Interativas tarefas/consultas, PySpark interativa|<ul><li>[Beeline](#beeline)</li><li>[Zeppelin com o Livy](../spark/apache-spark-zeppelin-notebook.md)</li><li>[Vista do Hive](../hadoop/apache-hadoop-use-hive-ambari-view.md)</li><li>[ODBC/JDBC – Power BI](../hadoop/apache-hadoop-connect-hive-power-bi.md)</li><li>[Ferramentas do Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)</li></ul>|
|Spark|Cenários do batch – envio de Spark, PySpark|<ul><li>[Livy](../spark/apache-spark-livy-rest-interface.md)</li></ul>|
|Consulta interativa (LLAP)|Interativo|<ul><li>[Beeline](#beeline)</li><li>[Vista do Hive](../hadoop/apache-hadoop-use-hive-ambari-view.md)</li><li>[ODBC/JDBC – Power BI](../hadoop/apache-hadoop-connect-hive-power-bi.md)</li><li>[Ferramentas do Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)</li></ul>|
|Qualquer|Instalar aplicação personalizada|<ul><li>[Ações de script](../hdinsight-hadoop-customize-cluster-linux.md)</li></ul>|

   > [!NOTE]
   > Jupyter não está instalado/suportado no pacote de segurança da empresa.

Com as APIs padrão ajuda-o partir da perspetiva de segurança. Além disso, obtenha as seguintes vantagens:

1.  **Gestão** – pode gerir o seu código e automatizar tarefas utilizando APIs padrão – Livy, HS2 etc.
2.  **Auditoria** – com SSH, não é possível efetuar a auditoria, os utilizadores que SSH tinha no cluster. Isto não ser o caso quando as tarefas são construídas através de pontos finais padrão como iria ser executados no contexto de utilizador. 



### <a name="beeline"></a>Utilizar Beeline 
Instalar Beeline no seu computador e ligar através da internet pública, utilize os seguintes parâmetros: 

```
- Connection string: -u 'jdbc:hive2://<clustername>.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2'
- Cluster login name: -n admin
- Cluster login password -p 'password'
```

Se tiver Beeline instalado localmente e ligar através de uma rede Virtual do Azure, utilize os seguintes parâmetros: 

```
- Connection string: -u 'jdbc:hive2://<headnode-FQDN>:10001/;transportMode=http'
```

Para localizar o nome de domínio completamente qualificado de um headnode, utilize as informações no HDInsight gerir utilizando o documento de API de REST do Ambari.














## <a name="users-of-domain-joined-hdinsight-clusters"></a>Utilizadores de clusters do HDInsight associados a um domínio
Um cluster do HDInsight que não está associado ao domínio, tem duas contas de utilizador que são criadas durante a criação do cluster:

* **Administrador do Ambari**: esta conta também é conhecido como *Hadoop utilizador* ou *utilizador HTTP*. Esta conta pode ser utilizada para iniciar sessão Ambari em https://&lt;clustername >. azurehdinsight.net. Também pode ser utilizado para executar consultas em vistas do Ambari, executar tarefas através de ferramentas externas (por exemplo, do PowerShell, Templeton, Visual Studio) e autenticar com o controlador ODBC do Hive e ferramentas de BI (por exemplo, Excel, PowerBI ou Tableau).

Um cluster do HDInsight associados a um domínio tem três novos utilizadores além de administrador do Ambari.

* **Administração de Ranger**: esta conta é a conta de administrador local do Apache Ranger. Não é um utilizador de domínio do Active Directory. Esta conta pode ser utilizada para configurar políticas e efetuar outros administradores de utilizadores ou administradores delegados (de modo a que os utilizadores podem gerir políticas). Por predefinição, o nome de utilizador é *admin* e a palavra-passe é o mesmo que a palavra-passe de administrador do Ambari. A palavra-passe pode ser atualizada da página de definições no Ranger.
* **Utilizador de domínio de administrador de cluster**: esta conta é um utilizador de domínio do Active Directory designado como o administrador de cluster de Hadoop, incluindo Ambari e Ranger. Tem de fornecer credenciais do utilizador durante a criação do cluster. Este utilizador tem os seguintes privilégios:

  * Associar computadores ao domínio e colocá-los na UO que especificar durante a criação do cluster.
  * Crie principais de serviço na UO que especificar durante a criação do cluster.
  * Criação de entradas DNS inversas.

    Tenha em atenção de que os outros utilizadores do AD têm também esses privilégios.

    Existem alguns pontos finais do cluster (por exemplo, Templeton) que não são geridos pelo Ranger e, por conseguinte, não são seguros. Estes pontos finais são bloqueados para todos os utilizadores, exceto o utilizador de domínio de administrador do cluster.
* **Regular**: durante a criação do cluster, pode fornecer vários grupos do Active Directory. Os utilizadores nestes grupos são sincronizados para Ranger e Ambari. Estes utilizadores são utilizadores de domínio e tem acesso a apenas geridos Ranger pontos finais (por exemplo, Hiveserver2). Todas as políticas RBAC e auditoria irá ser aplicável a estes utilizadores.

## <a name="roles-of-domain-joined-hdinsight-clusters"></a>Funções de clusters do HDInsight associados a um domínio
HDInsight associados a um domínio têm as seguintes funções:

* Administrador de cluster
* Operador de cluster
* Administrador de Serviços
* Operador de serviço
* Utilizador de cluster

**Para ver as permissões de uma destas funções**

1. Abra a IU de gestão do Ambari.  Consulte [abra a IU de gestão do Ambari](#open-the-ambari-management-ui).
2. No menu à esquerda, clique em **funções**.
3. Clique na marca de pergunta azul para ver as permissões:

    ![Permissões de funções de HDInsight associados a um domínio](./media/apache-domain-joined-manage/hdinsight-domain-joined-roles-permissions.png)

## <a name="open-the-ambari-management-ui"></a>Abra a IU de gestão do Ambari

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Abra o cluster do HDInsight. Consulte [clusters lista e mostrar](../hdinsight-administer-use-management-portal.md#list-and-show-clusters).
3. Clique em **Dashboard** no menu superior para abrir o Ambari.
4. Inicie sessão com o nome de utilizador de domínio de administrador de cluster e a palavra-passe do Ambari.
5. Clique em de **Admin** menu pendente do canto superior botão canto direito do rato e, em seguida, clique em **gerir Ambari**.

    ![HDInsight associados a um domínio gerir Ambari](./media/apache-domain-joined-manage/hdinsight-domain-joined-manage-ambari.png)

    A IU do aspeto de:

    ![Associado a um domínio HDInsight Ambari IU de gestão](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui.png)

## <a name="list-the-domain-users-synchronized-from-your-active-directory"></a>Lista os utilizadores sincronizados a partir do seu Active Directory
1. Abra a IU de gestão do Ambari.  Consulte [abra a IU de gestão do Ambari](#open-the-ambari-management-ui).
2. No menu à esquerda, clique em **utilizadores**. Deverá ver todos os utilizadores sincronizados a partir do Active Directory para o cluster do HDInsight.

    ![Associado a um domínio HDInsight Ambari IU lista utilizadores de gestão](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-users.png)

## <a name="list-the-domain-groups-synchronized-from-your-active-directory"></a>Lista os grupos de domínio sincronizados a partir do seu Active Directory
1. Abra a IU de gestão do Ambari.  Consulte [abra a IU de gestão do Ambari](#open-the-ambari-management-ui).
2. No menu à esquerda, clique em **grupos**. Deverá ver todos os grupos sincronizados a partir do Active Directory para o cluster do HDInsight.

    ![Associado a um domínio HDInsight Ambari IU lista grupos de gestão](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-groups.png)

## <a name="configure-hive-views-permissions"></a>Configurar permissões de vistas do Hive
1. Abra a IU de gestão do Ambari.  Consulte [abra a IU de gestão do Ambari](#open-the-ambari-management-ui).
2. No menu à esquerda, clique em **vistas**.
3. Clique em **HIVE** para mostrar os detalhes.

    ![Associado a um domínio de gestão HDInsight Ambari vistas do Hive de IU](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views.png)
4. Clique em de **vista do Hive** ligação para configurar as vistas do Hive.
5. Desloque para baixo até o **permissões** secção.

    ![Associado a um domínio de gestão HDInsight Ambari vistas do Hive de IU configurar permissões](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views-permissions.png)
6. Clique em **adicionar utilizador** ou **adicionar grupo**e, em seguida, especifique os utilizadores ou grupos que podem utilizar vistas do Hive.

## <a name="configure-users-for-the-roles"></a>Configurar os utilizadores para as funções
 Para ver uma lista de funções e as respetivas permissões, consulte [clusters do HDInsight associados a funções de domínios](#roles-of-domain---joined-hdinsight-clusters).

1. Abra a IU de gestão do Ambari.  Consulte [abra a IU de gestão do Ambari](#open-the-ambari-management-ui).
2. No menu à esquerda, clique em **funções**.
3. Clique em **adicionar utilizador** ou **adicionar grupo** atribuir utilizadores e grupos para diferentes funções.

## <a name="next-steps"></a>Passos Seguintes
* Para configurar um cluster do HDInsight associado a um domínio, veja [Configurar clusters do HDInsight associados a um domínio](apache-domain-joined-configure.md).
* Para configurar políticas do Hive e executar consultas do Hive, veja [Configurar políticas do Hive para clusters do HDInsight associados a um domínio](apache-domain-joined-run-hive.md).
