---
title: "Configurar clusters do HDInsight associados a um domínio - Azure | Microsoft Docs"
description: "Saiba como configurar e clusters do HDInsight associados a um domínio"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: cgronlun
editor: cgronlun
tags: 
ms.assetid: 0cbb49cc-0de1-4a1a-b658-99897caf827c
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/24/2018
ms.author: saurinsh
ms.openlocfilehash: 6284b246c071fb99a8b47845aca34b6262e5b856
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2018
---
# <a name="configure-domain-joined-hdinsight-sandbox-environment"></a>Configurar o ambiente de sandbox HDInsight associados a um domínio

Saiba como configurar um cluster do Azure HDInsight com autónomo do Active Directory e [Apache Ranger](http://hortonworks.com/apache/ranger/) para tirar partido da autenticação forte e de acesso baseado em funções abundante políticas de controlo (RBAC). Para obter mais informações, consulte [clusters do HDInsight associados a um domínio introduzir](apache-domain-joined-introduction.md).

Sem domínio associado a um cluster do HDInsight, cada cluster só pode ter uma conta de utilizadores de HTTP do Hadoop e uma conta de utilizador SSH.  A autenticação de vários utilizador pode ser conseguida utilizando:

-   Autónoma do Active Directory em execução no IaaS do Azure.
-   Azure Active Directory.

Utilizar uma autónoma do Active Directory em execução no IaaS do Azure é descrito neste artigo. É a arquitetura mais simples de que um cliente pode seguir para obter suporte de vários utilizador no HDInsight. Este artigo abrange duas abordagens para esta configuração:

- Opção 1: Utilize um modelo de gestão de recursos do Azure para criar o diretório de ativa autónomo e o cluster do HDInsight.
- Opção 2: Todo o processo é dividido nos seguintes passos:
    - Crie um Active Directory através de um modelo.
    - LDAPS o programa de configuração.
    - Criar grupos e utilizadores do AD
    - Criar cluster do HDInsight

> [!IMPORTANT]
> Oozie não está ativado no HDInsight associados a um domínio.

## <a name="prerequisite"></a>Pré-requisito
* Subscrição do Azure

## <a name="option-1-one-step-approach"></a>Opção 1: abordagem de um passo
Nesta secção, abra um modelo de gestão de recursos do Azure do portal do Azure. O modelo é utilizado para criar uma autónoma do Active Directory e um cluster do HDInsight. Atualmente, pode criar cluster de Hadoop associados a um domínio, o cluster do Spark e o cluster de consulta interativo.

1. Clique na imagem seguinte para abrir o modelo no portal do Azure. O modelo está localizado nos [modelos de Início Rápido do Azure](https://azure.microsoft.com/resources/templates/).
   
    Para criar um cluster do Spark:

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/http%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fdomain-joined%2Fspark%2Ftemplate.json" target="_blank"><img src="../hbase/media/apache-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>

    Para criar um cluster de consulta interativo:

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/http%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fdomain-joined%2Finteractivequery%2Ftemplate.json" target="_blank"><img src="../hbase/media/apache-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>

    Para criar um cluster do Hadoop:

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/http%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fdomain-joined%2Fhadoop%2Ftemplate.json" target="_blank"><img src="../hbase/media/apache-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Introduza os valores, selecione **concordo com os termos e condições indicadas acima**, selecione **afixar ao dashboard**e, em seguida, clique em **Compra**. Coloque o cursor de rato sobre o início de sessão explicação junto aos campos para ver as descrições. A maioria dos valores foram preenchida. Pode utilizar os valores predefinidos ou os seus próprios valores.

    - **Grupo de recursos**: introduza um nome de grupo de recursos do Azure.
    - **Localização**: Escolha uma localização que esteja perto de si.
    - **Novo nome da conta de armazenamento**: introduza um nome de conta do Storage do Azure. Esta nova conta de armazenamento é utilizada pelo PDC, o BDC e o cluster do HDInsight, como a conta do storage predefinida.
    - **Nome de utilizador de Admin**: introduza o nome de utilizador de administrador de domínio.
    - **Palavra-passe de administrador**: introduza a palavra-passe de administrador do domínio.
    - **Nome de domínio**: O nome predefinido é *contoso.com*.  Se alterar o nome de domínio, tem também de atualizar o **proteger o certificado de LDAP** campo e o **DN de unidade organizacional** campo.
    - **Prefixo DNS**: introduza o prefixo DNS para o endereço IP público utilizado pelo balanceador de carga.
    - **Nome do cluster**: introduza o nome de cluster do HDInsight.
    - **Tipo de cluster**: não altere este valor. Se pretender alterar o tipo de cluster, utilize o modelo específico no último passo.
    - **Proteger a palavra-passe de certificados de Ldap**: Utilize o valor predefinido, a menos que altere o campo de certificado seguro LDAP.
    Alguns dos valores estão hard-coded no modelo, por exemplo, a contagem de instâncias de nó de trabalho é dois.  Para alterar os valores codificados, clique em **Editar modelo**.

    ![Modelo de edição do HDInsight cluster associados a um domínio](./media/apache-domain-joined-configure/hdinsight-domain-joined-edit-template.png)

Depois do modelo for concluído com êxito, 23 recursos são criados no grupo de recursos.

## <a name="option-2-multi-step-approach"></a>Opção 2: abordagem de vários passo

Existem quatro passos nesta secção:

1. Crie um Active Directory através de um modelo.
2. LDAPS o programa de configuração.
3. Criar grupos e utilizadores do AD
4. Criar cluster do HDInsight

### <a name="create-an-active-directory"></a>Criar um Active Directory

Modelo Azure Resource Manager torna mais fácil criar recursos do Azure. Esta secção, irá utilizar um [modelo de início rápido do Azure](https://azure.microsoft.com/resources/templates/active-directory-new-domain-ha-2-dc/) para criar uma nova floresta e domínio com duas máquinas virtuais. As duas máquinas de virtuais servir como um controlador de domínio principal e um controlador de domínio de cópia de segurança.

**Para criar um domínio com dois controladores de domínio**

1. Clique na imagem seguinte para abrir o modelo no portal do Azure.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Factive-directory-new-domain-ha-2-dc%2Fazuredeploy.json" target="_blank"><img src="./media/apache-domain-joined-configure/deploy-to-azure.png" alt="Deploy to Azure"></a>

    O modelo tem o seguinte:

    ![Associado a um domínio de HDInsight criar as máquinas virtuais de domínio de floresta](./media/apache-domain-joined-configure/hdinsight-domain-joined-create-arm-template.png)

2. Introduza os seguintes valores:

    - **Subscrição**: selecione uma subscrição do Azure.
    - **Nome do grupo de recursos**: escreva um nome de grupo de recursos.  Um grupo de recursos é utilizado para gerir os recursos do Azure que estão relacionados com um projeto.
    - **Localização**: selecione uma localização do Azure que está perto de si.
    - **Nome de utilizador de Admin**: Este é o nome de utilizador de administrador de domínio. Este utilizador não é a conta de utilizador HTTP de cluster do HDInsight. Esta é a conta que utiliza em todo o tutorial.
    - **Palavra-passe de administrador**: introduza a palavra-passe para o administrador de domínio.
    - **Nome de domínio**: O nome de domínio tem de ser um nome de duas partes. Por exemplo: contoso.com, ou contoso. local ou hdinsight.test.
    - **Prefixo DNS**: escreva um prefixo de DNS
    - **Porta de RDP PDC**: (utilizar o valor predefinido para este tutorial)
    - **Porta de RDP BDC**: (utilizar o valor predefinido para este tutorial)
    - **localização de artefactos**: (utilizar o valor predefinido para este tutorial)
    - **token SAS de localização de artefactos**: (deixe vazio para este tutorial.)

Demora cerca de 20 minutos para criar os recursos.

### <a name="setup-ldaps"></a>LDAPS o programa de configuração

O acesso protocolo LDAP (Lightweight Directory) é utilizado para leem e escrevem para o AD.

**Para estabelecer ligação com o PDC utilizando o ambiente de trabalho remoto**

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Abra o grupo de recursos e, em seguida, abra a máquina de virtual do domínio principal (PDC) do controlador. O nome predefinido do PDC é adPDC. 
3. Clique em **Connect** para estabelecer ligação com o PDC utilizando o ambiente de trabalho remoto.

    ![Associado a um domínio de HDInsight estabelecer a ligação de ambiente de trabalho remoto do PDC](./media/apache-domain-joined-configure/hdinsight-domain-joined-remote-desktop-pdc.png)


**Para adicionar os serviços de certificado do Active Directory**

4. Abra **Gestor de servidor** se não estiver aberto.
5. Clique em **gerir**e, em seguida, clique em **para adicionar funções e funcionalidades**.

    ![Associado a um domínio do HDInsight para adicionar funções e funcionalidades](./media/apache-domain-joined-configure/hdinsight-domain-joined-add-roles.png)
5. A partir de "Antes de começar", clique em **seguinte**.
6. Selecione **instalação baseada em funções ou baseada em funcionalidade**e, em seguida, clique em **seguinte**.
7. Selecione o PDC e, em seguida, clique em **seguinte**.  O nome predefinido do PDC é adPDC.
8. Selecione **os serviços de certificados do Active Directory**.
9. Clique em **adicionar funcionalidades** da caixa de diálogo de pop-up.
10. Siga o assistente, utilize as predefinições para o resto do procedimento.
11. Clique em **Fechar** para fechar o assistente.

**Para configurar o certificado do AD**

1. No Gestor de servidores, clique no ícone de notificação amarelo e, em seguida, clique em **serviços de configurar o certificado do Active Directory**.

    ![Associado a um domínio de HDInsight configurar o certificado do AD](./media/apache-domain-joined-configure/hdinsight-domain-joined-configure-ad-certificate.png)

2. Clique em **serviços de função** à esquerda, selecione **autoridade de certificação**e, em seguida, clique em **seguinte**.
3. Siga o assistente, utilize as predefinições para o resto do procedimento (clique **configurar** no último passo).
4. Clique em **Fechar** para fechar o assistente.

### <a name="optional-create-ad-users-and-groups"></a>(Opcional) Criar grupos e utilizadores do AD

**Para criar utilizadores e grupos no AD**
1. Ligar para o PDC utilizando o ambiente de trabalho remoto
1. Abra **utilizadores e computadores do Active Directory**.
2. Selecione o seu nome de domínio no painel esquerdo.
3. Clique em de **criar um novo utilizador no contentor atual** ícone no menu superior.

    ![Associado a um domínio de HDInsight criar utilizadores](./media/apache-domain-joined-configure/hdinsight-domain-joined-create-ad-user.png)
4. Siga as instruções para criar alguns utilizadores. Por exemplo, hiveuser1 e hiveuser2.
5. Clique em de **criar um novo grupo no contentor atual** ícone no menu superior.
6. Siga as instruções para criar um grupo chamado **HDInsightUsers**.  Este grupo é utilizado quando cria um cluster do HDInsight mais tarde no tutorial.

> [!IMPORTANT]
> Tem de reiniciar a máquina virtual do PDC antes de criar um cluster do HDInsight associados a um domínio.

### <a name="create-an-hdinsight-cluster-in-the-vnet"></a>Criar um cluster do HDInsight na VNet

Nesta secção, utilize o portal do Azure para adicionar um cluster do HDInsight para a rede virtual que criou utilizando o modelo do Resource Manager anteriormente no tutorial. Este artigo abrange apenas as informações específicas para a configuração de cluster associados a um domínio.  Para informações gerais, consulte [baseado em Linux criar clusters HDInsight no portal do Azure](../hdinsight-hadoop-create-linux-clusters-portal.md).  

**Para criar um cluster de HDInsight associados a um domínio**

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Abra o grupo de recursos que criou utilizando o modelo do Resource Manager anteriormente no tutorial.
3. Adicione um cluster do HDInsight para o grupo de recursos.
4. Selecione **personalizada** opção:

    ![Opção de criar HDInsight associado a um domínio personalizado](./media/apache-domain-joined-configure/hdinsight-domain-joined-portal-custom-configuration-option.png)

    Existem seis secções utilizando a opção de configuração personalizada: Noções básicas, armazenamento, aplicações, Cluster tamanho, as definições avançadas e resumo.
5. No **Noções básicas** secção:

    - Tipo de cluster: selecione **pacote de segurança empresarial**. Atualmente o pacote de segurança de empresa só pode ser ativado para os seguintes tipos de cluster: Hadoop, consulta interativa e Spark.

        ![Pacote de segurança da empresa associado de domínio de HDInsight](./media/apache-domain-joined-configure/hdinsight-creation-enterprise-security-package.png)
    - Nome de utilizador de início de sessão do cluster: Este é o utilizador de HTTP do Hadoop. Esta conta é diferente da conta de administrador de domínio.
    - Grupo de recursos: selecione o grupo de recursos que criou anteriormente utilizando o modelo do Resource Manager.
    - Localização: A localização tem de ser igual à que utilizou ao criar a vnet e os controladores de domínio com o modelo do Resource Manager.

6. No **definições avançadas** secção:

    - Definições de domínio:

        ![HDInsight domínio associado a um domínio de definições avançadas](./media/apache-domain-joined-configure/hdinsight-domain-joined-portal-advanced-domain-settings.png)
        
        - Nome de domínio: introduza o nome de domínio que utilizou no [criar um Active Directory](#create-an-active-directory).
        - Nome de utilizador de domínio: introduza o nome de utilizador de administrador do AD que utilizou no [criar um Active Directory](#create-an-active-directory).
        - A unidade organizacional: Consulte a captura de ecrã para obter um exemplo.
        - URL de LDAPS: Consulte a captura de ecrã para obter um exemplo
        - Grupo de utilizadores de acesso: introduza o nome do grupo de utilizador que criou no [criar AD utilizadores e grupos](#optionally-createad-users-and-groups)
    - Rede virtual: selecione a rede virtual que criou no [criar um Active Directory](#create-an-active-directory). O nome predefinido utilizado no modelo é **adVNET**.
    - Sub-rede: É o nome predefinido utilizado no modelo **adSubnet**.



Depois de concluir o tutorial, pode querer eliminar o cluster. Com o HDInsight, os dados são armazenados no Storage do Azure, pelo que pode eliminar um cluster em segurança quando este não está a ser utilizado. Também lhe é cobrado o valor de um cluster do HDInsight mesmo quando não o está a utilizar. Uma vez que os custos do cluster são muito superiores aos custos do armazenamento, faz sentido do ponto de vista económico eliminar os clusters quando não estiverem a ser utilizados. Para instruções de eliminação de um cluster, consulte [clusters do Hadoop gerir no HDInsight ao utilizar o portal do Azure](../hdinsight-administer-use-management-portal.md#delete-clusters).

## <a name="next-steps"></a>Passos Seguintes
* Para configurar políticas do Hive e executar consultas do Hive, veja [Configurar políticas do Hive para clusters do HDInsight associados a um domínio](apache-domain-joined-run-hive.md).
* Para utilizar o SSH para ligar a clusters do HDInsight associados a um domínio, consulte [utilizar o SSH com Hadoop baseado em Linux no HDInsight a partir do Linux, Unix ou OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

