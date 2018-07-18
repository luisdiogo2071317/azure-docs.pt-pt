---
title: Arquitetura de Azure HDInsight associado a um domínio | Documentos da Microsoft
description: Saiba como planear o HDInsight associado a um domínio.
services: hdinsight
documentationcenter: ''
author: omidm1
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7dc6847d-10d4-4b5c-9c83-cc513cf91965
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: ''
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: omidm
ms.openlocfilehash: 1f51a1fbb38bc27d15b7a45ca4783508d863fee5
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39112632"
---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>Planear clusters do Hadoop associados a um domínio do Azure no HDInsight

O cluster standard do Azure HDInsight é um cluster de utilizador único. É adequado para a maioria das empresas que têm mais pequenas equipas de aplicações cargas de trabalho de dados grandes. Cada utilizador pode criar um cluster dedicado a pedido e destruí-lo quando não é mais necessária. 

Muitas empresas terem sido movidos para um modelo no qual os clusters são geridos pelas equipas de TI e aplicação de várias equipas partilham esses clusters. Estas empresas maiores tem acesso multiusuário, a cada cluster no Azure HDInsight.

HDInsight depende de um fornecedor de identidade populares--Active Directory – de forma gerida. Ao integrar o HDInsight com [do Azure Active Directory Domain Services (Azure AD DS)](../../active-directory-domain-services/active-directory-ds-overview.md), pode acessar os clusters com as suas credenciais de domínio. 

As máquinas virtuais (VMs) no HDInsight são associados ao seu domínio fornecido a um domínio. Para isso, todos os serviços em execução no HDInsight (Ambari, do Hive thrift de Spark do servidor, Ranger, servidor e outros) funcionam de forma totalmente integrada para o usuário autenticado. Os administradores podem, em seguida, criar políticas de autorização segura com o Apache Ranger para fornecer controlo de acesso baseado em funções para recursos no cluster.


## <a name="integrate-hdinsight-with-active-directory"></a>Integrar o HDInsight no Active Directory

Código aberto Hadoop depende de Kerberos para autenticação e segurança. Portanto, nós de cluster do HDInsight são associados a um domínio a um domínio gerido pelo Azure AD DS. Segurança do Kerberos está configurada para os componentes do Hadoop no cluster. 

Para cada componente do Hadoop, é criado automaticamente um principal de serviço. Uma máquina correspondente principal também é criada para cada máquina que está associada ao domínio. Para armazenar estes serviços e entidades de segurança do computador, tem de fornecer uma unidade organizacional (UO) no controlador de domínio (Azure AD DS), onde essas entidades são colocadas. 

Para resumir, terá de configurar um ambiente com:

- Um domínio do Active Directory (gerenciado pelo Azure AD DS).
- Secure LDAP (LDAPS) ativado no Azure AD DS.
- Funcionamento em rede conectividade apropriada da rede virtual do HDInsight para a rede virtual do Azure AD DS, se escolher as redes virtuais separadas para os mesmos. Uma VM dentro da rede virtual do HDInsight deve ter uma linha Visual para o Azure AD DS através do peering da rede virtual. Se o HDInsight e Azure AD DS é implementado na mesma rede virtual, a conectividade é fornecida automaticamente e é necessária nenhuma ação adicional.
- Uma unidade Organizacional [criado no Azure AD DS](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md).
- Uma conta de serviço que tem permissões para:
    - Crie principais de serviço na UO.
    - Associar máquinas ao domínio e criar principais de máquina na UO.

Captura de ecrã seguinte mostra uma UO criada em contoso.com. Também mostra alguns dos principais de serviço e entidades de segurança da máquina.

![Unidade de organização para os clusters do HDInsight associados a um domínio](./media/apache-domain-joined-architecture/hdinsight-domain-joined-ou.png).

## <a name="set-up-different-domain-controllers"></a>Configurar controladores de domínio diferente
Atualmente, o HDInsight suporta apenas o Azure AD DS como controlador de domínio principal que o cluster utiliza para a comunicação de Kerberos. Mas outras configurações complexas do Active Directory são possíveis, desde que essa configuração leva a ativar o Azure AD DS para o acesso do HDInsight.

### <a name="azure-active-directory-domain-services"></a>Azure Active Directory Domain Services
[O Azure AD DS](../../active-directory-domain-services/active-directory-ds-overview.md) fornece um domínio gerido, que é totalmente compatível com o Windows Server Active Directory. Microsoft se encarrega de gerir, a aplicação de patches e a monitorizar o domínio numa configuração de elevada disponibilidade (HA). Pode implementar o cluster sem se preocupar em manter os controladores de domínio. 

Os utilizadores, grupos e as palavras-passe são sincronizadas a partir do Azure Active Directory (Azure AD). A sincronização unidirecional da sua instância do Azure AD para o Azure AD DS permite aos utilizadores iniciar sessão para o cluster, utilizando as mesmas credenciais empresariais. 

Para obter mais informações, consulte [clusters do HDInsight associados a um domínio de configurar com o Azure AD DS](./apache-domain-joined-configure-using-azure-adds.md).

### <a name="on-premises-active-directory-or-active-directory-on-iaas-vms"></a>No local do Active Directory ou Active Directory em VMs de IaaS

Se tiver uma instância do Active Directory no local ou configurações mais complexas do Active Directory para o seu domínio, pode sincronizar essas identidades para o Azure AD com o Azure AD Connect. Em seguida, pode ativar o Azure AD DS no inquilino do Active Directory. 

Uma vez que Kerberos se baseia nos hashes de palavra-passe, terá [ativar a sincronização de hash de palavra-passe no Azure AD DS](../../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md). Se estiver a utilizar o federação com os serviços de Federação do Active Directory (AD FS), pode, opcionalmente, configurar sincronização de hash de palavra-passe como uma cópia de segurança no caso de falha de infraestrutura do AD FS. Para obter mais informações, consulte [ativar a sincronização de hash de palavra-passe com o Azure AD Connect sync](../../active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md). 

Utilização no local do Active Directory ou do Active Directory em VMs de IaaS sozinho, sem o Azure AD e o Azure AD DS, não é uma configuração suportada para clusters do HDInsight associados a um domínio.

## <a name="next-steps"></a>Passos Seguintes
* [Configurar clusters do HDInsight associados a um domínio](apache-domain-joined-configure-using-azure-adds.md)
* [Configurar políticas do Hive para clusters do HDInsight associados a um domínio](apache-domain-joined-run-hive.md)
* [Gerir clusters do HDInsight associados a um domínio](apache-domain-joined-manage.md) 
