---
title: Configurar clusters do HDInsight associados a um domínio com o AAD DS
description: Saiba como configurar e clusters do HDInsight associados a um domínio através do Azure Active Directory Domain Services
services: hdinsight
author: omidm1
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: omidm
ms.openlocfilehash: 060ca8040f514ec1df48c2ca4568cbbb2a529267
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
---
# <a name="configure-domain-joined-hdinsight-clusters-using-azure-active-directory-domain-services"></a>Configurar clusters do HDInsight associados a um domínio através do Azure Active Directory Domain Services

Os clusters associados a um domínio fornecem empresa multiutilizador capacidades de segurança no HDInsight. Associado a um domínio clusters do HDInsight estão ligados a domínios do Active Directory, para que os utilizadores de domínio podem utilizar as respetivas credenciais de domínio para autenticar com os clusters e executar tarefas de macrodados. 

Neste artigo, irá aprender a configurar um cluster do HDInsight associados a um domínio utilizando o Azure Active Directory Domain Services.

> [!NOTE]
> Já não é suportado do Active Directory em VMs do IaaS do Azure.

## <a name="create-azure-adds"></a>Criar ADDS do Azure

Terá de criar um Azure do AD DS antes de poder criar um cluster do HDInsight. Para criar um Azure adiciona, consulte [ativar o Azure Active Directory Domain Services no portal do Azure](../../active-directory-domain-services/active-directory-ds-getting-started.md). 

> [!NOTE]
> Apenas os administradores do inquilino tem os privilégios para criar serviços de domínio. Se utilizar o Azure Data Lake armazenamento (ADLS) como armazenamento de predefinido para o HDInsight, em seguida, certifique-se o inquilino do Azure AD predefinido para ADLS mesmo que o domínio para o cluster do HDInsight. Para tal configurado para funcionar com o Azure Data Lake Store, autenticação multifator tiver de ser desativada para os utilizadores que terão acesso ao cluster.

Depois do serviço de domínio do AAD tiver sido aprovisionado, é necessário criar uma conta de serviço no AAD (que será sincronizado com o AAD DS) com as permissões adequadas para criar o cluster do HDInsight. Se já existir uma conta de serviço, terá de reposição da palavra-passe e aguarde até que sincroniza-se ao AAD DS (esta reposição resultará na criação do hash de palavra-passe do kerberos e pode demorar até 30 minutos). Esta conta de serviço deve ter o privillages seguintes:

- Associar computadores ao domínio e coloque principais máquina na UO que especificar durante a criação do cluster.
- Crie principais de serviço na UO que especificar durante a criação do cluster.

Tem de ativar o LDAP seguro para o domínio de gerido dos serviços de domínio do Azure AD. Para ativar o LDAP seguro, consulte [configurar segura LDAP (LDAPS) para um serviços de domínio do Azure AD geridos domínio](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="create-a-domain-joined-hdinsight-cluster"></a>Criar um cluster do HDInsight associados a um domínio

O passo seguinte consiste em Criar cluster do HDInsight com o AAD DS e a conta de serviço que criou na secção anterior.

É mais fácil colocar o serviço de domínio do Azure AD e o cluster do HDInsight a mesma network(VNet) virtual do Azure. No caso de estarem em VNets diferentes, tem de elemento ambas as VNets. Para obter mais informações, consulte [peering de rede Virtual](../../virtual-network/virtual-network-peering-overview.md).

Quando cria um cluster do HDInsight associados a um domínio, tem de fornecer os parâmetros seguintes:

- **Nome de domínio**: O nome de domínio que estão associado com o Azure AD DS. Por exemplo, contoso.onmicrosoft.com
- **Nome de utilizador de domínio**: A conta de serviço no Azure AD DC que é criado na secção anterior. Por exemplo, hdiadmin@contoso.onmicrosoft.com. Este utilizador de domínio será o administrador deste cluster do HDInsight associados a um domínio.
- **Palavra-passe do domínio**: A palavra-passe da conta de serviço.
- **Unidade organizacional**: O nome distinto da UO que pretende utilizar com o cluster do HDInsight. Por exemplo: UO = HDInsightOU, DC = contoso, DC = onmicrosohift, DC = com. Se essa UO não existir, o cluster do HDInsight tenta criar essa UO. 
- **URL de LDAPS**: por exemplo, ldaps://contoso.onmicrosoft.com:636
- **Grupo de utilizadores de acesso**: os grupos de segurança cujos utilizadores que pretende sincronizar para o cluster. Por exemplo, HiveUsers. Se pretender especificar vários grupos de utilizadores, separadas por vírgula ','.
 
> [!NOTE]
> Uma vez Apache Zeppelin utiliza o nome de domínio para autenticar a conta de serviço administrativas, a conta de serviço tem de ter o mesmo nome de domínio como o sufixo UPN para Apache Zeppelin funcionar corretamente.
 
A seguinte captura de ecrã mostra as configurações no portal do Azure:

![Configuração de serviços de domínio do Active Directory associados a um domínio do Azure HDInsight](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png).


## <a name="next-steps"></a>Passos Seguintes
* Para configurar políticas do Hive e executar consultas do Hive, veja [Configurar políticas do Hive para clusters do HDInsight associados a um domínio](apache-domain-joined-run-hive.md).
* Para utilizar o SSH para ligar a clusters do HDInsight associados a um domínio, consulte [utilizar o SSH com Hadoop baseado em Linux no HDInsight a partir do Linux, Unix ou OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

