---
title: Arquitetura do Azure HDInsight associados a um domínio | Microsoft Docs
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
ms.openlocfilehash: f4380f5d6ec379d5807f697294623a672bd270ae
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34715246"
---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>Planear clusters do Hadoop associados a um domínio do Azure no HDInsight

O cluster do HDInsight standard é um cluster de utilizador único. É adequado à maioria das empresas em que as cargas de trabalho de dados de grande dimensão são criadas por pequenas equipas de aplicações. Cada utilizador pode ter outro cluster dedicado a próprio a pedido e destrui-lo quando não é necessária já. No entanto, muitas empresas começou a mover para um modelo no qual clusters sejam geridos pelas equipas de TI e aplicação de várias equipas de clusters de partilha. Assim, multiuser para o cluster for necessário acesso no Azure HDInsight para estes maiores empresas.

HDInsight baseia-se no fornecedor de identidade mais popular – do Active Directory (AD) de uma forma gerido. Ao integrar o HDInsight com [do Azure Active Directory Domain Services (AAD DS)](../../active-directory-domain-services/active-directory-ds-overview.md), podem aceder os clusters com as suas credenciais de domínio. As VMs no HDInsight estão associados a um domínio para o domínio fornecido, pelo que todos os serviços em execução no HDInsight (Ambari, thrift de Spark do servidor, Ranger, de ramo de registo do servidor e outros) funcionam perfeitamente para o utilizador autenticado. Os administradores podem, em seguida, criar políticas de autorização segura com Apache Ranger para fornecer controlo de acesso baseado em funções para recursos no cluster.


## <a name="integrate-hdinsight-with-active-directory"></a>Integrar o HDInsight no Active Directory

Hadoop open source depende do Kerberos para fornecer autenticação e segurança. Por conseguinte, nós de cluster do HDInsight estão associados a um domínio a um domínio gerido pelo AAD DS. Segurança de Kerberos está configurada para os componentes do Hadoop no cluster. Para cada um dos componentes do Hadoop, é criado automaticamente um principal de serviço. Máquina correspondente principal também é criada para cada máquina que está associada ao domínio. Para armazenar destes principais de serviços e máquinas, é necessário para fornecer uma unidade organizacional (UO) no controlador de domínio (AAD DS), onde são colocados destes principais. 

Para resumir, terá de configurar um ambiente com:

- Um domínio do Active Directory (gerido pelo AAD-DS)
- Proteja o LDAP (LDAPS) ativada no AAD DS.
- Rede PROPER conectividade a partir do HDInsight VNET a VNET AAD DS, caso opte separar as VNETs para os mesmos. Uma VM dentro da VNET HDI deve ter uma visão de linha para o AAD DS utilizando o VNET peering. Se HDI e AAD DS são implementados na mesma VNET, a conectividade é fornecida automaticamente e é necessária nenhuma ação adicional.
- Uma unidade organizacional (UO) [criado no AAD DS](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md)
- Uma conta de serviço que tem permissões para:
    - Crie principais de serviço na UO.
    - Associar computadores ao domínio e crie princípios de máquina na UO.

A seguinte captura de ecrã mostra um UO criado em contoso.com. Algumas das principais de serviço e principais máquina são apresentadas, bem como a captura de ecrã.

![UO de clusters do HDInsight de associados a um domínio](./media/apache-domain-joined-architecture/hdinsight-domain-joined-ou.png).

### <a name="different-domain-controllers-setup"></a>Configuração de controladores de domínio diferentes:
HDInsight atualmente suporta apenas AAD DS como controlador de domínio principal que o cluster irá comunicar com Kerberise o cluster. No entanto, também são possíveis outras setups AD complexas, desde leva a ativar AAD DS para acesso HDI.

- **[Azure Active Directory Domain Services (AAD DS)](../../active-directory-domain-services/active-directory-ds-overview.md)**: este serviço fornece um domínio gerido, que é totalmente compatível com o Windows Server Active Directory. Microsoft encarrega-se de gestão, a aplicação de patches e o domínio de uma configuração altamente Available(HA) de monitorização. Pode implementar o cluster sem se preocupar manter os controladores de domínio. Os utilizadores, grupos e as palavras-passe são sincronizadas entre os utilizadores de ativação de [unidirecional sincronização do AAD para o AAD DS], Directory(AAD) Active Directory do Azure para iniciar sessão para o cluster utilizando as mesmas credenciais empresariais. Para obter mais informações, consulte [como configurar associados a um domínio HDInsight clusters com o AAD DS](./apache-domain-joined-configure-using-azure-adds.md).
- **No local AD ou AD em VMs do IaaS**: Se tiver uma AD no local ou outra AD mais complexa setups para o seu domínio, pode sincronizar essas identidades para o AAD utilizando AD Connect e, em seguida, ativar inquilino do AAD-DS em que o AD. Uma vez que o Kerberos depende de hashes de palavra-passe, terá de [ativar a sincronização de hash de palavra-passe no AAD DS](../../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md). Se estiver a utilizar federação com a Federação do AD serviços (ADFS), opcionalmente, pode configurar sincronização de hash de palavra-passe como uma cópia de segurança no caso de falha da sua infraestrutura de AD FS. Para obter mais informações, consulte [ativar a sincronização de hash de palavra-passe com o AAD Connect sincronização](../../active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md). Utilizar no local AD ou AD em VMs de IaaS individualmente, sem AAD e AAD DS não é uma configuração suportada para o cluster HDI domínio a associar.

## <a name="next-steps"></a>Passos Seguintes
* [Configurar clusters do HDInsight associados a um domínio](apache-domain-joined-configure-using-azure-adds.md).
* [Configurar políticas de ramo de registo associados a um domínio para clusters do HDInsight](apache-domain-joined-run-hive.md).
* [Gerir clusters do HDInsight associados a um domínio](apache-domain-joined-manage.md). 
