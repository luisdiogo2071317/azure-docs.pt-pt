---
title: Configurar um cluster do HDInsight associados a um domínio com o Azure AD DS
description: Saiba como definir e configurar um cluster do HDInsight associados a um domínio com o Azure Active Directory Domain Services
services: hdinsight
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 07/17/2018
ms.openlocfilehash: 17924b0a00f4605d41492768b0124c583664aca6
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43042146"
---
# <a name="configure-a-domain-joined-hdinsight-cluster-by-using-azure-active-directory-domain-services"></a>Configurar um cluster do HDInsight associados a um domínio com o Azure Active Directory Domain Services

Os clusters associados a um domínio fornecem acesso multiusuário em clusters do HDInsight do Azure. Clusters do HDInsight associados a um domínio estão ligadas a um domínio para que os utilizadores de domínio possam utilizar as credenciais de domínio para autenticar com os clusters e executar tarefas de macrodados. 

Neste artigo, irá aprender a configurar um cluster do HDInsight associados a um domínio com o Azure Active Directory Domain Services (Azure AD DS).

## <a name="enable-azure-ad-ds"></a>Ativar o Azure AD DS

Ativar o Azure AD DS é um pré-requisito antes de poder criar um cluster do HDInsight associados a um domínio. Para obter mais informações, consulte [ativar o Azure Active Directory Domain Services no portal do Azure](../../active-directory-domain-services/active-directory-ds-getting-started.md). 

> [!NOTE]
> Apenas os administradores de inquilinos tem os privilégios para criar uma instância do Azure AD DS. Se utilizar Gen1 de armazenamento do Azure Data Lake como armazenamento predefinido para o HDInsight, certifique-se de que o inquilino do Azure AD predefinido para a geração 1 de armazenamento do Data Lake é a mesmo que o domínio para o cluster do HDInsight. Porque Hadoop depende de Kerberos e a autenticação básica, autenticação multifator tem de ser desativada para os utilizadores que irão aceder ao cluster.

Depois de aprovisionar a instância do Azure AD DS, crie uma conta de serviço no Azure Active Directory (Azure AD) com as permissões corretas. Se esta conta de serviço já existir, repor a palavra-passe e aguarde até que sincronizar para o Azure AD DS. Esta reposição resultará na criação do hash de palavra-passe de Kerberos, e pode demorar até 30 minutos a sincronizar com o Azure AD DS. 

A conta de serviço tem os seguintes privilégios:

- Associar máquinas ao domínio e coloque principais de máquina na UO que especificou durante a criação do cluster.
- Crie principais de serviço na UO que especificou durante a criação do cluster.

> [!NOTE]
> Uma vez que Apache Zeppelin utiliza o nome de domínio para autenticar a conta de serviço administrativa, a conta de serviço *tem* têm o mesmo nome de domínio como o sufixo UPN para Apache Zeppelin funcionar corretamente.

Para saber mais sobre UOs e como gerenciá-las, veja [criar uma UO num domínio gerido do Azure AD DS](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md). 

LDAP seguro destina-se um domínio gerido do Azure AD DS. Para obter mais informações, consulte [configurar o LDAP seguro para um Azure AD DS o domínio gerido](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="create-a-domain-joined-hdinsight-cluster"></a>Criar um cluster do HDInsight associados a um domínio

A próxima etapa é criar o cluster do HDInsight com o Azure AD DS e a conta de serviço que criou na secção anterior.

É mais fácil colocar a instância do Azure AD DS e o cluster do HDInsight na mesma rede virtual do Azure. Se optar por colocá-los em redes virtuais diferentes, deve configurar o peering essas redes virtuais para que as VMs de HDInsight têm uma linha Visual para o controlador de domínio para ingressar em VMs. Para obter mais informações, consulte [peering de rede Virtual](../../virtual-network/virtual-network-peering-overview.md).

Quando cria um cluster do HDInsight associados a um domínio, tem de fornecer os seguintes parâmetros:

- **Nome de domínio**: O nome de domínio que está associada com o Azure AD DS. Um exemplo é contoso.onmicrosoft.com.

- **Nome de utilizador de domínio**: A conta de serviço no controlador de domínio do Azure adiciona gerida domínio que criou na secção anterior. Um exemplo é hdiadmin@contoso.onmicrosoft.com. Este utilizador de domínio será o administrador deste cluster do HDInsight.

- **Palavra-passe do domínio**: A palavra-passe da conta de serviço.

- **Unidade organizacional**: O nome único da UO que pretende utilizar com o cluster do HDInsight. Um exemplo é UO = HDInsightOU, DC = contoso, DC = onmicrosoft, DC = com. Se essa UO não existir, o cluster de HDInsight tenta criar a UO com os privilégios que tenha da conta de serviço. Por exemplo, se a conta de serviço está no grupo de administradores do Azure AD DS, tem as permissões adequadas para criar uma UO. Caso contrário, terá de criar a UO pela primeira vez e dar o controle de total de conta de serviço sobre nessa UO. Para obter mais informações, consulte [criar uma UO num domínio gerido do Azure AD DS](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md).

    > [!IMPORTANT]
    > Incluir todos os DCs, separados por vírgulas, depois da unidade Organizacional (por exemplo, UO = HDInsightOU, DC = contoso, DC = onmicrosoft, DC = com).

- **URL de LDAPS**: um exemplo é ldaps://contoso.onmicrosoft.com:636.

    > [!IMPORTANT]
    > Introduza o URL completo, incluindo "ldaps: / /" e o número de porta (: 636).

- **Grupo de utilizadores de acesso**: os grupos de segurança cujos usuários que quer sincronizar para o cluster. Por exemplo, HiveUsers. Se pretender especificar múltiplos grupos de utilizador, separadas por ponto e vírgula ";". O grupo (s) tem de existir no diretório antes de aprovisionamento. Para obter mais informações, consulte [criar um grupo e adicionar membros no Azure Active Directory](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md). Se o grupo não existir, ocorrerá um erro: "HiveUsers de grupo não foi encontrada no Active Directory".

Captura de ecrã seguinte mostra as configurações no portal do Azure:

   ![Configuração de serviços de domínio do Active Directory associados a um domínio do Azure HDInsight](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png).


## <a name="next-steps"></a>Passos Seguintes
* Para configurar políticas do Hive e executar consultas do Hive, consulte [configurar políticas do Hive para clusters do HDInsight associados a um domínio](apache-domain-joined-run-hive.md).
* Para utilizar o SSH para ligar a clusters do HDInsight associados a um domínio, consulte [utilizar o SSH com Hadoop baseado em Linux no HDInsight do Linux, Unix ou OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

