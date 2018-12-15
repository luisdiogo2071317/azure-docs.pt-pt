---
title: Arquitetura do HDInsight do Azure com o Enterprise Security Package
description: Saiba como planear a segurança do HDInsight com o Enterprise Security Package.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: omidm
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 50c5838f576b6fd6775373f2dbe3c46d751545c1
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/15/2018
ms.locfileid: "53437593"
---
# <a name="use-enterprise-security-package-in-hdinsight"></a>Utilizar o pacote de segurança empresarial no HDInsight

O cluster standard do Azure HDInsight é um cluster de utilizador único. É adequado para a maioria das empresas que têm mais pequenas equipas de aplicações cargas de trabalho de dados grandes. Cada utilizador pode criar um cluster dedicado a pedido e destruí-lo quando não é mais necessária. 

Muitas empresas terem sido movidos para um modelo no qual os clusters são geridos pelas equipas de TI e aplicação de várias equipas partilham esses clusters. Estas empresas maiores tem acesso multiusuário, a cada cluster no Azure HDInsight.

HDInsight depende de um fornecedor de identidade populares--Active Directory – de forma gerida. Ao integrar o HDInsight com [do Azure Active Directory Domain Services (Azure AD DS)](../../active-directory-domain-services/active-directory-ds-overview.md), pode acessar os clusters com as suas credenciais de domínio. 

As máquinas virtuais (VMs) no HDInsight são associados ao seu domínio fornecido a um domínio. Então, todos os serviços em execução no HDInsight (Apache Ambari, servidor Apache Hive, Apache Ranger, servidor do Apache Spark thrift e outros) funcionam perfeitamente para o usuário autenticado. Os administradores podem, em seguida, criar políticas de autorização segura com o Apache Ranger para fornecer controlo de acesso baseado em funções para recursos no cluster.

## <a name="integrate-hdinsight-with-active-directory"></a>Integrar o HDInsight no Active Directory

Código aberto Apache Hadoop depende de Kerberos para autenticação e segurança. Por conseguinte, nós de cluster do HDInsight com o pacote de segurança da empresa (ESP) estão associados a um domínio gerido pelo Azure AD DS. Segurança do Kerberos está configurada para os componentes do Hadoop no cluster. 

Os itens seguintes são criados automaticamente:
- um principal de serviço para cada componente do Hadoop 
- um principal de máquina para cada máquina que está associado ao domínio
- uma unidade organizacional (UO) para cada cluster para armazenar estas principais de serviços e da máquinas 

Para resumir, terá de configurar um ambiente com:

- Um domínio do Active Directory (gerenciado pelo Azure AD DS).
- Secure LDAP (LDAPS) ativado no Azure AD DS.
- Funcionamento em rede conectividade apropriada da rede virtual do HDInsight para a rede virtual do Azure AD DS, se escolher as redes virtuais separadas para os mesmos. Uma VM dentro da rede virtual do HDInsight deve ter uma linha Visual para o Azure AD DS através do peering da rede virtual. Se o HDInsight e Azure AD DS é implementado na mesma rede virtual, a conectividade é fornecida automaticamente e é necessária nenhuma ação adicional.

## <a name="set-up-different-domain-controllers"></a>Configurar controladores de domínio diferente
Atualmente, o HDInsight suporta apenas o Azure AD DS como controlador de domínio principal que o cluster utiliza para a comunicação de Kerberos. Mas outras configurações complexas do Active Directory são possíveis, desde que essa configuração leva a ativar o Azure AD DS para o acesso do HDInsight.

### <a name="azure-active-directory-domain-services"></a>Azure Active Directory Domain Services
[O Azure AD DS](../../active-directory-domain-services/active-directory-ds-overview.md) fornece um domínio gerido, que é totalmente compatível com o Windows Server Active Directory. Microsoft se encarrega de gerir, a aplicação de patches e a monitorizar o domínio numa configuração de elevada disponibilidade (HA). Pode implementar o cluster sem se preocupar em manter os controladores de domínio. 

Os utilizadores, grupos e as palavras-passe são sincronizadas a partir do Azure Active Directory (Azure AD). A sincronização unidirecional da sua instância do Azure AD para o Azure AD DS permite aos utilizadores iniciar sessão para o cluster, utilizando as mesmas credenciais empresariais. 

Para obter mais informações, consulte [clusters do HDInsight configurar com ESP com o Azure AD DS](./apache-domain-joined-configure-using-azure-adds.md).

### <a name="on-premises-active-directory-or-active-directory-on-iaas-vms"></a>No local do Active Directory ou Active Directory em VMs de IaaS

Se tiver uma instância do Active Directory no local ou configurações mais complexas do Active Directory para o seu domínio, pode sincronizar essas identidades para o Azure AD com o Azure AD Connect. Em seguida, pode ativar o Azure AD DS no inquilino do Active Directory. 

Uma vez que Kerberos se baseia nos hashes de palavra-passe, terá [ativar a sincronização de hash de palavra-passe no Azure AD DS](../../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md). Se estiver a utilizar o federação com os serviços de Federação do Active Directory (AD FS), pode, opcionalmente, configurar sincronização de hash de palavra-passe como uma cópia de segurança no caso de falha de infraestrutura do AD FS. Para obter mais informações, consulte [ativar a sincronização de hash de palavra-passe com o Azure AD Connect sync](../../active-directory/hybrid/how-to-connect-password-hash-synchronization.md). 

Utilizar o Active Directory no local ou no Active Directory em VMs de IaaS sozinho, sem o Azure AD e o Azure AD DS, não é uma configuração suportada para clusters do HDInsight com ESP.

## <a name="next-steps"></a>Passos Seguintes

* [Configurar clusters do HDInsight com ESP](apache-domain-joined-configure-using-azure-adds.md)
* [Configurar políticas do Apache Hive para clusters do HDInsight com o ESP](apache-domain-joined-run-hive.md)
* [Gerir clusters do HDInsight com o ESP](apache-domain-joined-manage.md) 
