---
title: Sincronizar utilizadores do Azure Active Directory para um cluster - Azure HDInsight
description: Sincronize utilizadores autenticados do Azure Active Directory para um cluster.
services: hdinsight
author: ashishthaps
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/19/2018
ms.author: ashishth
ms.openlocfilehash: 08ae8bb1f1ac9b718996d1d4715f28d025aeebcb
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/07/2018
ms.locfileid: "39591613"
---
# <a name="synchronize-azure-active-directory-users-to-an-hdinsight-cluster"></a>Sincronizar utilizadores do Azure Active Directory para um cluster do HDInsight

[Clusters do HDInsight associados a um domínio](hdinsight-domain-joined-introduction.md) podem utilizar a autenticação forte com utilizadores do Azure Active Directory (Azure AD), bem como utilizar *controlo de acesso baseado em funções* políticas (RBAC). À medida que adiciona utilizadores e grupos para o Azure AD, pode sincronizar os utilizadores que necessitam de aceder ao seu cluster.

## <a name="prerequisites"></a>Pré-requisitos

Se ainda não tiver feito, [criar um cluster do HDInsight associados a um domínio](hdinsight-domain-joined-configure.md).

## <a name="add-new-azure-ad-users"></a>Adicionar o novo Azure os utilizadores do AD

Para ver os seus anfitriões, abra a interface do Usuário de Web do Ambari. Cada nó será atualizado com novas definições de atualização automáticas.

1. Na [portal do Azure](https://portal.azure.com), navegue para o diretório do Azure AD associado com o seu cluster associado a um domínio.

2. Selecione **todos os utilizadores** a partir do menu do lado esquerdo, em seguida, selecione **novo utilizador**.

    ![Painel de todos os utilizadores](./media/hdinsight-sync-aad-users-to-cluster/aad-users.png)

3. Preencha o formulário de utilizador novo. Selecione os grupos que criou para atribuir permissões baseadas em cluster. Neste exemplo, crie um grupo chamado "HiveUsers", para que pode atribuir novos utilizadores. O [instruções de exemplo](hdinsight-domain-joined-configure.md) para criar um cluster associado a um domínio incluem a adição de dois grupos, `HiveUsers` e `AAD DC Administrators`.

    ![Novo painel de utilizador](./media/hdinsight-sync-aad-users-to-cluster/aad-new-user.png)

4. Selecione **Criar**.

## <a name="use-the-ambari-rest-api-to-synchronize-users"></a>Utilizar a API de REST do Ambari para sincronizar os utilizadores

Grupos de utilizadores especificados durante o processo de criação do cluster são sincronizados nesse momento. Sincronização do usuário ocorre automaticamente uma vez por hora. Para sincronizar os utilizadores imediatamente ou, para sincronizar um grupo que não seja os grupos especificados durante a criação do cluster, utilize a API de REST do Ambari.

O método seguinte utiliza o POST com a API de REST do Ambari. Para obter mais informações, consulte [clusters do HDInsight gerir com a API de REST do Ambari](hdinsight-hadoop-manage-ambari-rest-api.md).

1. [Ligar ao cluster com SSH](hdinsight-hadoop-linux-use-ssh-unix.md). No painel de descrição geral do seu cluster no portal do Azure, selecione o **Secure Shell (SSH)** botão.

    ![Secure Shell (SSH)](./media/hdinsight-sync-aad-users-to-cluster/ssh.png)

2. Copiar apresentados `ssh` de comandos e colá-lo no seu cliente SSH. Introduza o ssh palavra-passe de utilizador quando lhe for pedido.

3. Após a autenticação, introduza o seguinte comando:

    ```bash
    curl -u admin:<YOUR PASSWORD> -sS -H "X-Requested-By: ambari" \
    -X POST -d '{"Event": {"specs": [{"principal_type": "groups", "sync_type": "existing"}]}}' \
    "https://<YOUR CLUSTER NAME>.azurehdinsight.net/api/v1/ldap_sync_events"
    ```
    
    A resposta deve ter este aspeto:

    ```json
    {
      "resources" : [
        {
          "href" : "http://hn0-hadoop.<YOUR DOMAIN>.com:8080/api/v1/ldap_sync_events/1",
          "Event" : {
            "id" : 1
          }
        }
      ]
    }
    ```

4. Para ver o estado de sincronização, execute uma nova `curl` comando com o `href` valor devolvido do comando anterior:

    ```bash
    curl -u admin:<YOUR PASSWORD> http://hn0-hadoop.<YOUR DOMAIN>.com:8080/api/v1/ldap_sync_events/1
    ```
    
    A resposta deve ter este aspeto:
    
    ```json
    {
      "href" : "http://hn0-hadoop.YOURDOMAIN.com:8080/api/v1/ldap_sync_events/1",
      "Event" : {
        "id" : 1,
        "specs" : [
          {
            "sync_type" : "existing",
            "principal_type" : "groups"
          }
        ],
        "status" : "COMPLETE",
        "status_detail" : "Completed LDAP sync.",
        "summary" : {
          "groups" : {
            "created" : 0,
            "removed" : 0,
            "updated" : 0
          },
          "memberships" : {
            "created" : 1,
            "removed" : 0
          },
          "users" : {
            "created" : 1,
            "removed" : 0,
            "skipped" : 0,
            "updated" : 0
          }
        },
        "sync_time" : {
          "end" : 1497994072182,
          "start" : 1497994071100
        }
      }
    }
    ```

5. Este resultado mostra que o estado é **COMPLETE**, foi criado um novo utilizador e o utilizador foi atribuído uma associação. Neste exemplo, é atribuída ao utilizador para o sincronizados "HiveUsers" grupo de LDAP, uma vez que o utilizador foi adicionado para esse mesmo grupo no Azure AD.

> [!NOTE]
> O método anterior sincroniza apenas os grupos do Azure AD especificados na **grupo de utilizadores de acesso** propriedade das definições de domínio durante a criação do cluster. Para obter mais informações, consulte [criar um cluster do HDInsight](domain-joined/apache-domain-joined-configure.md).

## <a name="verify-the-newly-added-azure-ad-user"></a>Certifique-se recentemente adicionado utilizador do Azure AD

Abra o [IU Web do Ambari](hdinsight-hadoop-manage-ambari.md) para verificar se o novo utilizador do Azure AD foi adicionado. Aceder à IU Web do Ambari, navegando até **`https://<YOUR CLUSTER NAME>.azurehdinsight.net`**. Introduza o nome de utilizador de administrador de cluster e a palavra-passe.

1. A partir do dashboard do Ambari, selecione **gerir Ambari** sob a **administrador** menu.

    ![Gerir Ambari](./media/hdinsight-sync-aad-users-to-cluster/manage-ambari.png)

2. Selecione **os utilizadores** sob a **gestão de grupos de utilizador +** grupo de menu no lado esquerdo da página.

    ![Item de menu de utilizadores](./media/hdinsight-sync-aad-users-to-cluster/users-link.png)

3. O novo utilizador deve estar listado na tabela Users. O tipo está definido como `LDAP` vez `Local`.

    ![Página usuários](./media/hdinsight-sync-aad-users-to-cluster/users.png)

## <a name="log-in-to-ambari-as-the-new-user"></a>Inicie sessão no Ambari como o novo utilizador

Quando o utilizador novo (ou qualquer outro utilizador de domínio) inicia sessão Ambari, utilizam as credenciais de domínio e nome de utilizador do AD Azure completo.  Ambari apresenta um alias do utilizador, o que é o nome a apresentar do utilizador no Azure AD. O novo utilizador de exemplo tem o nome de utilizador `hiveuser3@contoso.com`. No Ambari, este novo utilizador aparece como `hiveuser3` , mas os utilizadores iniciam sessão Ambari como `hiveuser3@contoso.com`.

## <a name="see-also"></a>Consulte também

* [Configurar políticas do Hive no HDInsight associado a um domínio](hdinsight-domain-joined-run-hive.md)
* [Gerir clusters do HDInsight associados a um domínio](hdinsight-domain-joined-manage.md)
* [Autorizar os utilizadores a Ambari](hdinsight-authorize-users-to-ambari.md)
