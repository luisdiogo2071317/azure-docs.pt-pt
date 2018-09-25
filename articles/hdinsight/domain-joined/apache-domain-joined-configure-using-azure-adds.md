---
title: Configurar um cluster do HDInsight com o Enterprise Security Package com o Azure AD-DS
description: Saiba como definir e configurar um cluster do HDInsight Enterprise Security Package com o Azure Active Directory Domain Services
services: hdinsight
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: a5b377381fd540c2a9f1d85e0cb7edce32c2dae8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46968378"
---
# <a name="configure-a-hdinsight-cluster-with-enterprise-security-package-by-using-azure-active-directory-domain-services"></a>Configurar um cluster do HDInsight com o Enterprise Security Package com o Azure Active Directory Domain Services

Os clusters de pacote de segurança da empresa (ESP) fornecem acesso de vários utilizadores em clusters do HDInsight do Azure. Clusters do HDInsight com o ESP estão ligados a um domínio para que os utilizadores de domínio possam utilizar as credenciais de domínio para autenticar com os clusters e executar tarefas de macrodados. 

Neste artigo, irá aprender a configurar um cluster do HDInsight com ESP, utilizando o Azure Active Directory Domain Services (Azure AD-DS).

>[!NOTE]
>ESP está disponível no HDI 3.6 + para Spark, interativo e Hadoop. O ESP para tipos de clusters de HBase está em pré-visualização.


## <a name="enable-azure-ad-ds"></a>Ativar o Azure AD DS

Ativar o Azure AD-DS é um pré-requisito antes de poder criar um cluster do HDInsight com ESP. Para obter mais informações, consulte [ativar o Azure Active Directory Domain Services no portal do Azure](../../active-directory-domain-services/active-directory-ds-getting-started.md). 

> [!NOTE]
> Apenas os administradores de inquilinos tem os privilégios para criar uma instância do Azure AD-DS. Se utilizar Gen1 de armazenamento do Azure Data Lake como armazenamento predefinido para o HDInsight, certifique-se de que o inquilino do Azure AD predefinido para a geração 1 de armazenamento do Data Lake é a mesmo que o domínio para o cluster do HDInsight. Porque Hadoop depende de Kerberos e a autenticação básica, autenticação multifator tem de ser desativada para os utilizadores que irão aceder ao cluster.

LDAP seguro destina-se um domínio gerido do Azure AD-DS. Ao ativar o LDAPS, coloque o nome de domínio no nome do requerente ou nome alternativo do requerente no certificado. Para obter mais informações, consulte [configurar LDAP seguro para um Azure AD-DS o domínio gerido](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="add-managed-identity"></a>Adicionar identidade gerida

Depois de ativado o Azure AD-DS, criar uma identidade gerida e atribuí-lo para o **contribuinte de serviços de domínio do HDInsight** função no controlo de acesso do Azure AD DS.

![Controlo de acesso de serviços de domínio do Active Directory do Azure](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-configure-managed-identity.png)

Para obter mais informações, consulte [o que há de identidades geridas para recursos do Azure](../../active-directory/managed-identities-azure-resources/overview.md).

## <a name="create-a-hdinsight-cluster-with-esp"></a>Criar um cluster do HDInsight com ESP

A próxima etapa é criar o cluster do HDInsight com ESP ativado com o Azure AD-DS.

É mais fácil colocar a instância do Azure AD-DS e o cluster do HDInsight na mesma rede virtual do Azure. Se optar por colocá-los em redes virtuais diferentes, deve configurar o peering essas redes virtuais para que as VMs de HDInsight têm uma linha Visual para o controlador de domínio para ingressar em VMs. Para obter mais informações, consulte [peering de rede Virtual](../../virtual-network/virtual-network-peering-overview.md).

Quando criar um cluster do HDInsight, terá a opção de ativar o Enterprise Security Package ligar o seu cluster com o Azure AD-DS. 

![Rede e de segurança do Azure HDInsight](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-security-networking.png)

Depois de ativar o ESP, configurações incorretas comuns relacionados com o Azure AD-DS serão automaticamente detetadas e validadas.

![Validação do domínio de pacote de segurança do Azure HDInsight Enterprise](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-esp-domain-validate.png)

Detecção antecipada economiza tempo, permitindo que corrija os erros antes de criar o cluster.

![Pacote de segurança do Azure HDInsight Enterprise falha na validação do domínio](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-esp-domain-validate-failed.png)

Quando cria um cluster do HDInsight com ESP, deve fornecer os seguintes parâmetros:

- **Utilizador de administrador de cluster**: Escolha um administrador para o cluster de sincronização do Azure AD-DS.

- **Grupos de acesso de cluster**: os grupos de segurança, cujos usuários que quer sincronizar para o cluster devem ser sincronizado e estão disponíveis no Azure AD-DS. Por exemplo, HiveUsers. Se pretender especificar múltiplos grupos de utilizador, separadas por ponto e vírgula ";". O grupo (s) tem de existir no diretório antes de aprovisionamento. Para obter mais informações, consulte [criar um grupo e adicionar membros no Azure Active Directory](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md). Se o grupo não existir, ocorrerá um erro: "HiveUsers de grupo não foi encontrada no Active Directory".

- **URL de LDAPS**: um exemplo é ldaps://contoso.onmicrosoft.com:636.

    > [!IMPORTANT]
    > Introduza o URL completo, incluindo "ldaps: / /" e o número de porta (: 636).

Captura de ecrã seguinte mostra as configurações no portal do Azure:

   ![Configuração do HDInsight ESP Active Directory Domain Services do Azure](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png).


## <a name="next-steps"></a>Passos Seguintes
* Para configurar políticas do Hive e executar consultas do Hive, consulte [configurar políticas do Hive para HDInsight clusters com ESP](apache-domain-joined-run-hive.md).
* Para utilizar o SSH para ligar a clusters do HDInsight com ESP, consulte [utilizar o SSH com Hadoop baseado em Linux no HDInsight do Linux, Unix ou OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

