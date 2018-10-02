---
title: Configurar um cluster do HDInsight com o Enterprise Security Package com o Azure AD-DS
description: Saiba como definir e configurar um cluster do HDInsight Enterprise Security Package com o Azure Active Directory Domain Services.
services: hdinsight
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 6cfe587abadf8350fecc497b1af1cea9700f4f28
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/02/2018
ms.locfileid: "48018731"
---
# <a name="configure-a-hdinsight-cluster-with-enterprise-security-package-by-using-azure-active-directory-domain-services"></a>Configurar um cluster do HDInsight com o Enterprise Security Package com o Azure Active Directory Domain Services

Os clusters de pacote de segurança da empresa (ESP) fornecem acesso de vários utilizadores em clusters do HDInsight do Azure. Clusters do HDInsight com o ESP estão ligados a um domínio para que os utilizadores de domínio possam utilizar as credenciais de domínio para autenticar com os clusters e executar tarefas de macrodados. 

Neste artigo, irá aprender a configurar um cluster do HDInsight com ESP, utilizando o Azure Active Directory Domain Services (Azure AD-DS).

>[!NOTE]
>O ESP é GA no HDI 3.6 para Spark, interativo e Hadoop. O ESP para tipos de clusters de HBase e o Kafka está em pré-visualização.

## <a name="enable-azure-ad-ds"></a>Ativar o Azure AD DS

Ativar o Azure AD-DS é um pré-requisito antes de poder criar um cluster do HDInsight com ESP. Para obter mais informações, consulte [ativar o Azure Active Directory Domain Services no portal do Azure](../../active-directory-domain-services/active-directory-ds-getting-started.md). 

Quando o Azure AD-DS está ativado, todos os utilizadores e objetos de iniciar a sincronização do Azure Active Directory (AAD) para o Azure AD-DS por predefinição. O comprimento da operação de sincronização depende do número de objetos no AAD. A sincronização pode demorar alguns dias para centenas de milhares de objetos. 

Os clientes podem optar por sincronizar apenas os grupos que precisam de acesso para os clusters do HDInsight. Esta opção de apenas determinados grupos de sincronização é chamada *âmbito de sincronização*. Ver [configurar um âmbito sincronização do Azure AD ao seu domínio gerido](https://docs.microsoft.com/en-us/azure/active-directory-domain-services/active-directory-ds-scoped-synchronization) para obter instruções.

> [!NOTE]
> Apenas os administradores de inquilinos tem os privilégios para criar uma instância do Azure AD-DS. Autenticação multifator tem de ser desativada apenas para os utilizadores que irão aceder ao cluster.

Quando ativar o LDAP seguro, coloque o nome de domínio no nome do requerente ou nome alternativo do requerente no certificado. Por exemplo, se o nome de domínio for *contoso.com*, certifique-se de que nome exato existe no seu nome de requerente do certificado ou nome alternativo do requerente. Para obter mais informações, consulte [configurar LDAP seguro para um Azure AD-DS o domínio gerido](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="check-aad-ds-health-status"></a>Verificar estado de funcionamento do DS do AAD

Ver o estado de funcionamento do seu Azure Active Directory Domain Services, selecionando **estado de funcionamento** sob a **gerir** categoria. Certificar-se de que o estado do AAD-DS está verde (em execução) e a sincronização estiver concluída.

![Estado de funcionamento de serviços de domínio do Active Directory do Azure](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-health.png)

## <a name="add-managed-identity"></a>Adicionar identidade gerida

Crie uma identidade gerida atribuído ao utilizador se ainda não tiver um. Ver [Create, lista, delete ou o atribuir uma função para uma identidade gerida atribuído ao utilizador com o portal do Azure](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal) para obter instruções. 

A identidade gerida é usada para simplificar as operações de serviços de domínio. Esta identidade tem acesso para ler, criar, modificar e eliminar operações de serviços de domínio que são necessários para o HDInsight Enterprise Security Package como a criação de UOs e os princípios de serviço.

Depois de ativar o Azure AD-DS, criar uma identidade gerida atribuído ao utilizador e atribuí-lo para o **contribuinte de serviços de domínio do HDInsight** função no controlo de acesso do Azure AD DS.

![Controlo de acesso de serviços de domínio do Active Directory do Azure](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-configure-managed-identity.png)

Atribuir uma identidade gerida para o **contribuinte de serviços de domínio do HDInsight** função garante que a identidade tem acesso adequado para executar determinadas operações de serviços de domínio no domínio do AAD-DS. Para obter mais informações, consulte [o que há de identidades geridas para recursos do Azure](../../active-directory/managed-identities-azure-resources/overview.md).

## <a name="create-a-hdinsight-cluster-with-esp"></a>Criar um cluster do HDInsight com ESP

A próxima etapa é criar o cluster do HDInsight com ESP ativado com o Azure AD-DS.

É mais fácil colocar a instância do Azure AD-DS e o cluster do HDInsight na mesma rede virtual do Azure. Se optar por colocá-los em redes virtuais diferentes, deve configurar o peering essas redes virtuais para que as VMs de HDInsight têm uma linha Visual para o controlador de domínio para ingressar em VMs. Para obter mais informações, consulte [peering de rede Virtual](../../virtual-network/virtual-network-peering-overview.md). Para testar se peering é feito corretamente, Junte-se a uma VM para o HDInsight VNET/subrede e enviar pings para o nome de domínio ou execute **ldp.exe** ao domínio de acesso DS do AAD.

Quando criar um cluster do HDInsight, terá a opção de ativar o Enterprise Security Package no separador personalizado. 

![Rede e de segurança do Azure HDInsight](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-security-networking.png)

Depois de ativar o ESP, configurações incorretas comuns relacionados com o Azure AD-DS serão automaticamente detetadas e validadas.

![Validação do domínio de pacote de segurança do Azure HDInsight Enterprise](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-esp-domain-validate.png)

Detecção antecipada economiza tempo, permitindo que corrija os erros antes de criar o cluster.

![Pacote de segurança do Azure HDInsight Enterprise falha na validação do domínio](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-esp-domain-validate-failed.png)

Quando cria um cluster do HDInsight com ESP, deve fornecer os seguintes parâmetros:

- **Utilizador de administrador de cluster**: Escolha um administrador para o cluster de sincronização do Azure AD-DS. Esta conta tem de ser já sincronizado e está disponível no AAD-DS.

- **Grupos de acesso de cluster**: os grupos de segurança, cujos usuários que quer sincronizar para o cluster devem estar disponíveis no Azure AD-DS. Por exemplo, HiveUsers grupo. Para obter mais informações, consulte [criar um grupo e adicionar membros no Azure Active Directory](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

- **URL de LDAPS**: um exemplo é ldaps://contoso.com:636.

Captura de ecrã seguinte mostra um configurações bem-sucedida no portal do Azure:

![Configuração do HDInsight ESP Active Directory Domain Services do Azure](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png).

A identidade gerida que criou no pode ser escolhida no menu pendente atribuído ao utilizador a identidade gerida quando criar um novo cluster.

![Configuração do HDInsight ESP Active Directory Domain Services do Azure](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-identity-managed-identity.png).


## <a name="next-steps"></a>Passos Seguintes
* Para configurar políticas do Hive e executar consultas do Hive, consulte [configurar políticas do Hive para HDInsight clusters com ESP](apache-domain-joined-run-hive.md).
* Para utilizar o SSH para ligar a clusters do HDInsight com ESP, consulte [utilizar o SSH com Hadoop baseado em Linux no HDInsight do Linux, Unix ou OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
