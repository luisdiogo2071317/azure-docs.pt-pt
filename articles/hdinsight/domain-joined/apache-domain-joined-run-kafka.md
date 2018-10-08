---
title: Configurar as políticas de Kafka no HDInsight com o Pacote de Segurança Enterprise - Azure
description: Saiba como configurar as políticas do Apache Ranger para Kafka no Azure HDInsight com o Pacote de Segurança Enterprise.
services: hdinsight
ms.service: hdinsight
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.topic: tutorial
ms.date: 09/24/2018
ms.openlocfilehash: 1a8f04f39568816252175fc9e0893f1ab3e2cdc6
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47224822"
---
# <a name="tutorial-configure-kafka-policies-in-hdinsight-with-enterprise-security-package-preview"></a>Tutorial: configurar as políticas de Kafka no HDInsight com o Pacote de Segurança Enterprise (Pré-visualização)

Saiba como configurar as políticas do Apache Ranger para clusters do Kafka do Pacote de Segurança Enterprise (ESP). Os clusters do ESP estão ligados a um domínio, permitindo que os utilizadores sejam autenticados com credenciais do domínio. Neste tutorial, vai criar duas políticas do Ranger para restringir o acesso aos tópicos `sales*` e `marketingspend`.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar utilizadores de domínio
> * Criar políticas do Ranger
> * Crie tópicos num cluster do Kafka
> * Testar as políticas do Ranger

## <a name="before-you-begin"></a>Antes de começar

* Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/).

* Inicie sessão no [portal do Azure](https://portal.azure.com/).

* Crie um [cluster Kafka no HDInsight com o Pacote de Segurança Enterprise](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-run-hive).

## <a name="connect-to-apache-ranger-admin-ui"></a>Ligar à IU do Apache Ranger Admin

1. Num browser, ligue à interface de utilizador do Ranger Admin através do URL `https://<ClusterName>.azurehdinsight.net/Ranger/`. Não se esqueça de alterar `<ClusterName>` para o nome do cluster do Kafka.

    > [!NOTE] 
    > As credenciais do Ranger não são iguais às credenciais de cluster do Hadoop. Para impedir que os browsers utilizem credenciais em cache do Hadoop, utilize uma nova janela do browser InPrivate para ligar à IU do Ranger Admin.

2. Inicie sessão com as suas credenciais de administrador do Azure Active Directory (AD). As credenciais de administrador do Azure AD não iguais às credenciais de cluster do HDInsight ou às credenciais SSH de nó HDInsight do Linux.

   ![IU do Apache Ranger Admin](./media/apache-domain-joined-run-kafka/apache-ranger-admin-login.png)

## <a name="create-domain-users"></a>Criar utilizadores de domínio

Visite [Criar um cluster do HDInsight com o Pacote de Segurança Enterprise](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-configure-using-azure-adds#create-a-domain-joined-hdinsight-cluster), para saber como criar os utilizadores de domínio **sales_user** e **marketing_user**. Num cenário de produção, os utilizadores de domínio são provenientes do seu inquilino do Active Directory.

## <a name="create-ranger-policy"></a>Criar política do Ranger 

Crie uma política do Ranger para **sales_user** e **marketing_user**.

1. Abra a **IU do Ranger Admin**.

2. Clique em **\<ClusterName>_kafka** em **Kafka**. Poderá ser apresentada uma política pré-configurada.

3. Clique em **Adicionar Nova Política** e introduza os seguintes valores:

   |**Definição**  |**Valor sugerido**  |
   |---------|---------|
   |Nome da Política  |  política hdi sales*   |
   |Tópico   |  sales* |
   |Selecionar Utilizador  |  sales_user1 |
   |Permissões  | publicar, consumir, criar |

   Os carateres universais seguintes podem ser incluídos no nome do tópico:

   * “*” indica nenhuma ou mais ocorrências de carateres.
   * “?” indica um caráter único.

   ![Criar Política da IU do Apache Ranger Admin](./media/apache-domain-joined-run-kafka/apache-ranger-admin-create-policy.png)   

   >[!NOTE] 
   >Aguarde um momento enquanto o Ranger sincroniza com o Azure AD, se um utilizador de domínio não for preenchido automaticamente em **Selecionar Utilizador**.

4. Clique em **Adicionar** para guardar a política.

5. Clique em **Adicionar Nova Política** e, em seguida, introduza os seguintes valores:

   |**Definição**  |**Valor sugerido**  |
   |---------|---------|
   |Nome da Política  |  política hdi marketing   |
   |Tópico   |  marketingspend |
   |Selecionar Utilizador  |  marketing_user1 |
   |Permissões  | publicar, consumir, criar |

   ![Criar Política da IU do Apache Ranger Admin](./media/apache-domain-joined-run-kafka/apache-ranger-admin-create-policy-2.png)  

6. Clique em **Adicionar** para guardar a política.

## <a name="create-topics-in-a-kafka-cluster-with-esp"></a>Criar tópicos num cluster do Kafka com o ESP

Para criar dois tópicos, **salesevents** e **marketingspend**:

1. Utilize o seguinte comando para abrir uma ligação SSH ao cluster:

   ```bash
   ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net
   ```

   Substitua `SSHUSER` pelo utilizador SSH do seu cluster e `CLUSTERNAME` pelo nome do seu cluster. Se tal lhe for pedido, introduza a palavra-passe da conta de utilizador SSH. Para obter mais informações sobre como utilizar `scp` com o HDInsight, veja [Utilizar SSH com o HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix).

   Num cenário de produção, os utilizadores de domínio configurados durante a criação do cluster podem aceder por SSH ao cluster.

2. Utilize os comandos seguintes para guardar o nome do cluster numa variável e instalar um utilitário de análise JSON `jq`. Quando lhe for pedido, introduza o nome do cluster do Kafka.

   ```bash
   sudo apt -y install jq
   read -p 'Enter your Kafka cluster name:' CLUSTERNAME
   ```

3. Utilize os comandos seguintes para obter os anfitriões de mediador do Kafka e os anfitriões do Zookeeper. Quando lhe for pedido, introduza a palavra-passe da conta de administrador do cluster.

   ```bash
   export KAFKAZKHOSTS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`; \
   
   export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`; \
   ```

4. Execute os seguintes comandos: 

   ```bash
   java -jar -Djava.security.auth.login.config=/usr/hdp/current/kafka-broker/config/kafka_client_jaas.conf kafka-producer-consumer.jar create salesevents $KAFKABROKERS
   
   java -jar -Djava.security.auth.login.config=/usr/hdp/current/kafka-broker/config/kafka_client_jaas.conf kafka-producer-consumer.jar create marketingspend $KAFKABROKERS
   ```

   >[!NOTE] 
   >Apenas o proprietário de processo do serviço Kafka, como a raiz, pode escrever nos znodes `/config/topics` do Zookeeper. As políticas do Ranger não são impostas quando um utilizador sem privilégios cria um tópico. Isto acontece porque o script `kafka-topics.sh` comunica diretamente com o Zookeeper para criar o tópico. As entradas são adicionadas aos nós do Zookeeper, enquanto os observadores do lado do mediador monitorizam e criam tópicos em conformidade. A autorização não pode ser feita por meio do plug-in do Ranger, e o comando acima é executado com `sudo` através do mediador do Kafka.


## <a name="test-the-ranger-policies"></a>Testar as políticas do Ranger

Com base nas políticas do Ranger configuradas, o **sales_user** pode produzir/consumir o tópico **salesevents**, mas não o tópico **marketingspend**. Por outro lado, o **marketing_user** pode produzir/consumir o tópico **marketingspend**, mas não o tópico **salesevents**.

1. Abra uma nova ligação SSH ao cluster. Utilize o seguinte comando para iniciar sessão como **sales_user1**:

   ```bash
   ssh sales_user1@CLUSTERNAME-ssh.azurehdinsight.net
   ```

2. Execute o seguinte comando:

   ```bash
   export KAFKA_OPTS="-Djava.security.auth.login.config=/usr/hdp/current/kafka-broker/config/kafka_client_jaas.conf"
   ```

3. Utilize os nomes do mediador e do Zookeeper da secção anterior para definir as seguintes variáveis de ambiente:

   ```bash
   export KAFKABROKERS=<brokerlist>:9092 
   ```

   Exemplo: `export KAFKABROKERS=wn0-khdicl.contoso.com:9092,wn1-khdicl.contoso.com:9092`

   ```bash
   export KAFKAZKHOSTS=<zklist>:2181
   ```

   Exemplo: `export KAFKAZKHOSTS=zk1-khdicl.contoso.com:2181,zk2-khdicl.contoso.com:2181`

4. Certifique-se de que o **sales_user1** pode produzir para o tópico **salesevents**.
   
   Execute o seguinte comando para iniciar a consola-produtora para o tópico **salesevents**:

   ```bash
   /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic salesevents --security-protocol SASL_PLAINTEXT
   ```

   Em seguida, introduza algumas mensagens na consola. Prima **Ctrl + C** para sair da consola-produtora.

5. Execute o seguinte comando para consumir do tópico **salesevents**:

   ```bash
   /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $KAFKAZKHOSTS --topic salesevents --security-protocol PLAINTEXTSASL --from-beginning
   ```
 
6. Certifique-se de que as mensagens que introduziu no passo anterior serão apresentadas e que o **sales_user1** não consegue produzir para o tópico **marketingspend**.

   Na mesma janela ssh, conforme apresentado acima, execute o seguinte comando para produzir para o tópico **marketingspend**:

   ```bash
   /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic marketingspend --security-protocol SASL_PLAINTEXT
   ```

   Um erro de autorização ocorre e pode ser ignorado. 

7. Tenha em atenção que o **marketing_user1** não pode consumir do tópico **salesevents**.

   Repita os passos 1 a 3 acima, mas desta vez, como **marketing_user1**.

   Execute o seguinte comando para consumir do tópico **salesevents**:

   ```bash
   /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $KAFKAZKHOSTS --topic marketingspend --security-protocol PLAINTEXTSASL --from-beginning
   ```

   As mensagens anteriores não podem ser vistas.

8. Veja os eventos de acesso de auditoria da IU do Ranger.

   ![Auditoria à Política da IU do Ranger](./media/apache-domain-joined-run-kafka/apache-ranger-admin-audit.png)

## <a name="next-steps"></a>Passos seguintes

* [Traga a sua própria chave para o Kafka](https://docs.microsoft.com/azure/hdinsight/kafka/apache-kafka-byok)
* [Uma introdução à segurança do Hadoop com o Pacote de Segurança Enterprise](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-introduction)
