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
ms.date: 04/17/2018
ms.author: omidm
ms.openlocfilehash: 20d6dbad6fa1914c8b12f47bb48f6efba3895887
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>Planear clusters do Hadoop associados a um domínio do Azure no HDInsight

O cluster do HDInsight standard é um cluster de utilizador único. É adequado à maioria das empresas em que as cargas de trabalho de dados de grande dimensão são criadas por pequenas equipas de aplicações. Como Hadoop adquiridos popularidade, muitas empresas começou a mover para um modelo no qual clusters sejam geridos pelas equipas de TI e aplicação de várias equipas de clusters de partilha. Assim, as funcionalidades que envolvem clusters multiutilizador estão entre as funcionalidades mais pedidas no Azure HDInsight.

Em vez de criar a sua própria multiuser autenticação e autorização, o HDInsight baseia-se no fornecedor de identidade mais popular – do Active Directory (AD). A funcionalidade de segurança poderosas no AD pode ser utilizada para gerir a autenticação multiuser no HDInsight. Ao integrar o HDInsight com o AD, pode comunicar com os clusters com as suas credenciais do AD. As VMs no HDInsight estão associados a um domínio ao seu AD, sendo que como HDInsight mapeia um utilizador do AD para um utilizador local do Hadoop, por isso, todos os serviços em execução no HDInsight (Ambari, thrift de Spark do servidor, Ranger, de ramo de registo do servidor e outros) funcionam perfeitamente para o utilizador autenticado. Os administradores podem, em seguida, criar políticas de autorização segura com Apache Ranger para fornecer controlo de acesso baseado em funções para recursos no HDInsight.


## <a name="integrate-hdinsight-with-active-directory"></a>Integrar o HDInsight no Active Directory

Quando a integração do HDInsight com o Active Directory, nós de cluster do HDInsight estão associados a um domínio a um domínio do AD. Segurança de Kerberos está configurada para os componentes do Hadoop no cluster. Para cada um dos componentes do Hadoop, é criado um principal de serviço no Active Directory. Máquina correspondente principal também é criada para cada máquina que está associada ao domínio. Estes principais de serviço e principais máquina podem clutter do Active Directory. Como resultado, é necessário para fornecer uma unidade organizacional (UO) no Active Directory, onde são colocados destes principais. 

Para resumir, tem de configurar um ambiente com:

- Um controlador de domínio do Active Directory com LDAPS configurado.
- Conectividade de rede Virtual do HDInsight para o seu controlador de domínio do Active Directory.
- Uma unidade organizacional criado no Active Directory.
- Uma conta de serviço que tem permissões para:

    - Crie principais de serviço na UO.
    - Associar computadores ao domínio e crie princípios de máquina na UO.

A seguinte captura de ecrã mostra um UO criado em contoso.com. Algumas das principais de serviço e principais máquina são apresentadas, bem como a captura de ecrã.

![UO de clusters do HDInsight de associados a um domínio](./media/apache-domain-joined-architecture/hdinsight-domain-joined-ou.png).

### <a name="the-way-of-bringing-your-own-active-directory-domain-controllers"></a>A forma de colocar os seus próprios controladores de domínio do Active Directory

- **Os serviços de domínio do Active Directory do Azure**: este serviço fornece um domínio do Active Directory gerido, que é totalmente compatível com o Windows Server Active Directory. Microsoft encarrega-se de gerir, a aplicação de patches e a monitorização de domínio do AD. Pode implementar o cluster sem se preocupar manter os controladores de domínio. Os utilizadores, grupos e as palavras-passe são sincronizadas a partir do Azure Active Directory, permitindo aos utilizadores iniciar sessão para o cluster utilizando as respetivas credenciais empresariais. Para obter mais informações, consulte [clusters do HDInsight de configurar-associados a um domínio com o Azure Active Directory Domain Services](./apache-domain-joined-configure-using-azure-adds.md).


## <a name="next-steps"></a>Passos Seguintes
* Para gerir clusters do HDInsight associados a um domínio, veja [Manage domain-joined HDInsight clusters (Gerir clusters do HDInsight associados a um domínio)](apache-domain-joined-manage.md).
* Para configurar políticas do Hive e executar consultas do Hive, veja [Configurar políticas do Hive para clusters do HDInsight associados a um domínio](apache-domain-joined-run-hive.md).
* Para executar consultas do Hive através de SSH nos clusters do HDInsight associados a um domínio, consulte [utilizar o SSH com o HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).
