---
title: Configurar clusters do HDInsight associados a um domínio com o AAD DS
description: Saiba como configurar e clusters do HDInsight associados a um domínio através do Azure Active Directory Domain Services
services: hdinsight
author: omidm1
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: omidm
ms.openlocfilehash: 8064940e0d0f035010a2521752d6f32f3f9ccd9f
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "34849837"
---
# <a name="configure-domain-joined-hdinsight-clusters-using-azure-active-directory-domain-services"></a>Configurar clusters do HDInsight associados a um domínio através do Azure Active Directory Domain Services

Os clusters de associados a um domínio fornecem acesso a vários utilizador nos clusters do HDInsight. Associado a um domínio clusters do HDInsight estão ligados a um domínio, para que os utilizadores de domínio podem utilizar as respetivas credenciais de domínio para autenticar com os clusters e executar tarefas de macrodados. 

Neste artigo, irá aprender a configurar um cluster do HDInsight associados a um domínio utilizando o Azure Active Directory Domain Services.

## <a name="create-azure-adds"></a>Criar ADDS do Azure

Ativar os serviços de domínio do Azure AD (AAD-DS) é um pré-requisito antes de poder criar um cluster do HDInsight associados a um domínio. Para ativar uma instância de AAD DS, consulte [como ativar o AAD-DS no portal do Azure](../../active-directory-domain-services/active-directory-ds-getting-started.md). 

> [!NOTE]
> Apenas os administradores do inquilino tem os privilégios para criar uma instância de AAD DS. Se utilizar o Azure Data Lake armazenamento (ADLS) como armazenamento de predefinido para o HDInsight, em seguida, certifique-se o inquilino do Azure AD predefinido para ADLS mesmo que o domínio para o cluster do HDInsight. Sincde Hadoop depende de Kerberos e a autenticação básica, autenticação multifator tiver de ser desativada para os utilizadores que terão acesso ao cluster.

Depois da instância do AAD DS tenha sido aprovisionada, terá de criar uma conta de serviço no AAD (que será sincronizado com o AAD DS) com as permissões corretas. Se já existir uma conta de serviço, terá de repor a palavra-passe e aguarde até que sincroniza-se ao AAD DS (esta reposição resultará na criação do hash de palavra-passe do kerberos e pode demorar até 30 min a sincronizar para o AAD DS). Esta conta de serviço deve ter os seguintes privilégios:

- Associar computadores ao domínio e coloque principais máquina na UO que especificar durante a criação do cluster.
- Crie principais de serviço na UO que especificar durante a criação do cluster.

> [!NOTE]
> Uma vez Apache Zeppelin utiliza o nome de domínio para autenticar a conta de serviço administrativas, a conta de serviço tem de ter o mesmo nome de domínio como o sufixo UPN para Apache Zeppelin funcionar corretamente.

Para obter mais informações sobre UOs e como geri-los, consulte [criará uma unidade Organizacional num DS AAD](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md). 

LDAP seguro de domínio geridos do AAD-DS é necessário. Para ativar o LDAP seguro, consulte [como configurar o LDAP seguro (LDAPS) para um DS AAD geridos domínio](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="create-a-domain-joined-hdinsight-cluster"></a>Criar um cluster do HDInsight associados a um domínio

O passo seguinte consiste em Criar cluster do HDInsight com o AAD-DS e a conta de serviço que criou na secção anterior.

É mais fácil colocar o AAD-DS e o cluster do HDInsight a mesma network(VNet) virtual do Azure. Caso tenha optado por colocá-los em VNets diferentes, tem de elemento esses VNets para que as VMs de HDI tenham uma visão de linha para o controlador de domínio para efetuar a adesão as VMs. Para obter mais informações, consulte [peering de rede Virtual](../../virtual-network/virtual-network-peering-overview.md).

Quando cria um cluster do HDInsight associados a um domínio, tem de fornecer os parâmetros seguintes:

- **Nome de domínio**: O nome de domínio que estão associado com o AAD DS. Por exemplo, contoso.onmicrosoft.com
- **Nome de utilizador de domínio**: A conta de serviço no domínio gerida que é criado na secção anterior. Por exemplo, hdiadmin@contoso.onmicrosoft.com. Este utilizador de domínio será um administrador deste cluster do HDInsight.
- **Palavra-passe do domínio**: A palavra-passe da conta de serviço.
- **Unidade organizacional**: O nome distinto da UO que pretende utilizar com o cluster do HDInsight. Por exemplo: UO = HDInsightOU, DC = contoso, DC = onmicrosohift, DC = com. Se não existir essa UO, cluster do HDInsight tenta criar a unidade Organizacional utilizando a conta de serviço tem privilégios. (Por exemplo, se a conta de serviço está no grupo de administradores do AAD DS, tem as permissões adequadas para criar uma UO, caso contrário, poderá ter de criar primeiro a unidade Organizacional e atribua o controlo total de conta de serviço através de nessa UO primeiro - consulte [criará uma unidade Organizacional num AAD-DS](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md)).

    > [!IMPORTANT]
    > É importante incluir todos o DCs sepearated por vírgula após a UO (por exemplo, UO = HDInsightOU, DC = contoso, DC = onmicrosohift, DC = com).

- **URL de LDAPS**: por exemplo, ldaps://contoso.onmicrosoft.com:636

    > [!IMPORTANT]
    > Introduza o url completo, incluindo o ldaps: / / e número de porta (: 636)

- **Grupo de utilizadores de acesso**: os grupos de segurança cujos utilizadores que pretende sincronizar para o cluster. Por exemplo, HiveUsers. Se pretender especificar vários grupos de utilizadores, separadas por vírgula ','.
 
A seguinte captura de ecrã mostra as configurações no portal do Azure:

![Configuração de serviços de domínio do Active Directory associados a um domínio do Azure HDInsight](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png).


## <a name="next-steps"></a>Passos Seguintes
* Para configurar políticas do Hive e executar consultas do Hive, veja [Configurar políticas do Hive para clusters do HDInsight associados a um domínio](apache-domain-joined-run-hive.md).
* Para utilizar o SSH para ligar a clusters do HDInsight associados a um domínio, consulte [utilizar o SSH com Hadoop baseado em Linux no HDInsight a partir do Linux, Unix ou OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

