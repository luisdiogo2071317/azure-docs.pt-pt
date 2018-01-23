---
title: Sincronizar os utilizadores do Azure Active Directory para um cluster - Azure HDInsight | Microsoft Docs
description: Sincronize os utilizadores autenticados do Azure Active Directory para um cluster.
services: hdinsight
documentationcenter: 
author: ashishthaps
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/19/2018
ms.author: ashishth
ms.openlocfilehash: ad1586a6e358dfb1ca2391474ecdd9bee2f6226d
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2018
---
# <a name="synchronize-azure-active-directory-users-to-an-hdinsight-cluster"></a>Sincronizar os utilizadores do Azure Active Directory para um cluster do HDInsight

[Associado a um domínio clusters do HDInsight](hdinsight-domain-joined-introduction.md) pode utilizar a autenticação forte com utilizadores do Azure Active Directory (Azure AD), bem como utilizar *controlo de acesso baseado em funções* políticas (RBAC). Como adicionar utilizadores e grupos para o Azure AD, pode sincronizar os utilizadores que necessitam de aceder ao cluster.

## <a name="prerequisites"></a>Pré-requisitos

Se que ainda não tiver feito, [criar um cluster do HDInsight associados a um domínio](hdinsight-domain-joined-configure.md).

## <a name="add-new-azure-ad-users"></a>Adicionar novo Azure os utilizadores do AD

Para ver os anfitriões, abra a IU da Web do Ambari. Cada nó será atualizado com novas definições de atualização automáticas.

1. No [portal do Azure](https://portal.azure.com), navegue para o diretório do Azure AD associado ao seu cluster associados a um domínio.

2. Selecione **todos os utilizadores** no menu da esquerda, em seguida, selecione **novo utilizador**.

    ![Todos os painel de utilizadores](./media/hdinsight-sync-aad-users-to-cluster/aad-users.png)

3. Preencha o formulário de utilizador de novo. Selecione os grupos que criou para atribuir permissões baseadas em cluster. Neste exemplo, crie um grupo denominado "HiveUsers", para que pode atribuir os novos utilizadores. O [instruções de exemplo](hdinsight-domain-joined-configure.md) para criar um cluster associados a um domínio incluem a adição de dois grupos, `HiveUsers` e `AAD DC Administrators`.

    ![Painel do novo utilizador](./media/hdinsight-sync-aad-users-to-cluster/aad-new-user.png)

4. Selecione **Criar**.

## <a name="use-the-ambari-rest-api-to-synchronize-users"></a>Utilize a API de REST do Ambari para sincronizar os utilizadores

Grupos de utilizador especificados durante o processo de criação do cluster são sincronizados a essa hora. Sincronização do utilizador automaticamente ocorre uma vez a cada hora. Para sincronizar os utilizadores imediatamente, ou para sincronizar um grupo que não seja os grupos especificados durante a criação do cluster, utilize a API de REST do Ambari.

O método seguinte utiliza POST com a API de REST do Ambari. Para obter mais informações, consulte [gerir clusters do HDInsight utilizando a API de REST do Ambari](hdinsight-hadoop-manage-ambari-rest-api.md).

1. [Ligar ao cluster com SSH](hdinsight-hadoop-linux-use-ssh-unix.md). A partir do painel de descrição geral para o seu cluster no portal do Azure, selecione o **Secure Shell (SSH)** botão.

    ![Secure Shell (SSH)](./media/hdinsight-sync-aad-users-to-cluster/ssh.png)

2. Copie o apresentadas `ssh` comando e cole-o cliente SSH. Introduza o ssh palavra-passe de utilizador quando lhe for pedido.

3. Após a autenticação, introduza o seguinte comando:

    ```bash
    curl -u admin:<YOUR PASSWORD> -sS -H "X-Requested-By: ambari" \
    -X POST -d '{"Event": {"specs": [{"principal_type": "groups", "sync_type": "existing"}]}}' \
    "https://<YOUR CLUSTER NAME>.azurehdinsight.net/api/v1/ldap_sync_events"
    ```
    
    A resposta deve ter o seguinte aspeto:

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

4. Para ver o estado de sincronização, execute uma nova `curl` de comandos, utilizando o `href` valor devolvido do comando anterior:

    ```bash
    curl -u admin:<YOUR PASSWORD> http://hn0-hadoop.<YOUR DOMAIN>.com:8080/api/v1/ldap_sync_events/1
    ```
    
    A resposta deve ter o seguinte aspeto:
    
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

5. Este resultado mostra que o estado é **concluída**, foi criado um novo utilizador e o utilizador foi atribuído uma associação. Neste exemplo, o utilizador está atribuído a sincronizados "HiveUsers" grupo de LDAP, uma vez que o utilizador foi adicionado para esse mesmo grupo no Azure AD.

> [!NOTE]
> O método anterior sincroniza apenas os grupos do Azure AD especificados no **grupo de utilizadores de acesso** propriedade das definições do domínio durante a criação do cluster. Para obter mais informações, consulte [criar um cluster do HDInsight](domain-joined/apache-domain-joined-configure.md).

## <a name="verify-the-newly-added-azure-ad-user"></a>Verifique se foram recentemente adicionadas utilizador do Azure AD

Abra o [IU da Web do Ambari](hdinsight-hadoop-manage-ambari.md) para verificar se o novo utilizador do Azure AD foi adicionado. Aceder à IU da Web do Ambari, navegando até  **`https://<YOUR CLUSTER NAME>.azurehdinsight.net`** . Introduza o nome de utilizador de administrador de cluster e a palavra-passe.

1. A partir do dashboard do Ambari, selecione **gerir Ambari** sob o **admin** menu.

    ![Gerir Ambari](./media/hdinsight-sync-aad-users-to-cluster/manage-ambari.png)

2. Selecione **utilizadores** sob o **utilizador + grupo gestão** grupo de menu no lado esquerdo da página.

    ![Item de menu de utilizadores](./media/hdinsight-sync-aad-users-to-cluster/users-link.png)

3. O novo utilizador deve estar listado na tabela de utilizadores. O tipo está definido como `LDAP` vez `Local`.

    ![Página utilizadores](./media/hdinsight-sync-aad-users-to-cluster/users.png)

## <a name="log-in-to-ambari-as-the-new-user"></a>Inicie sessão no Ambari como o novo utilizador

Quando o utilizador novo (ou qualquer outro utilizador de domínio) inicia sessão no Ambari, utilizam as respetivas credenciais de nome e domínio de utilizador do AD do Azure completo.  Ambari apresenta um alias do utilizador, que é o nome a apresentar do utilizador no Azure AD. O novo utilizador de exemplo com o nome de utilizador `hiveuser3@contoso.com`. No Ambari, este novo utilizador aparece como `hiveuser3` , mas os utilizadores iniciam sessão Ambari como `hiveuser3@contoso.com`.

## <a name="see-also"></a>Consulte também

* [Configurar políticas de Hive no HDInsight associados a um domínio](hdinsight-domain-joined-run-hive.md)
* [Gerir clusters do HDInsight associados a um domínio](hdinsight-domain-joined-manage.md)
* [Autorizar os utilizadores a Ambari](hdinsight-authorize-users-to-ambari.md)
