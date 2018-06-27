---
title: HDInsight - associados a um domínio clusters do HDInsight - Azure
description: Saiba mais...
services: hdinsight
author: omidm1
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7dc6847d-10d4-4b5c-9c83-cc513cf91965
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/26/2018
ms.author: omidm
ms.openlocfilehash: 3fd3a4b8982fe2170726df03bdc884e658d0b0c2
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37019493"
---
# <a name="an-introduction-to-hadoop-security-with-domain-joined-hdinsight-clusters"></a>Uma introdução ao Hadoop segurança com clusters do HDInsight associados a um domínio

Até hoje, o Azure HDInsight suportava apenas um único administrador local de utilizadores. Isto resultava muito bem para equipas ou departamentos de aplicações mais pequenos. Como o Hadoop baseado em cargas de trabalho adquiridas popularidade mais no setor da empresa, a necessidade de capacidades de nível empresarial, como a autenticação baseada no Active Directory, suporte de vários utilizador e controlo de acesso baseado em funções está cada vez mais importantes. Através da utilização de clusters do HDInsight associados a um domínio, pode criar um cluster do HDInsight associado a um domínio do Active Directory, configurar uma lista de empregados da empresa que podem autenticar-se através do Azure Active Directory para iniciar sessão no cluster do HDInsight. Qualquer pessoa fora da empresa não pode iniciar sessão nem aceder ao cluster do HDInsight. O administrador de empresa pode configurar o controlo de acesso baseado em funções para utilizar o Hive segurança [Apache Ranger](http://hortonworks.com/apache/ranger/), assim restringir o acesso a dados apenas sucederia conforme necessário. Por fim, o administrador pode auditar o acesso aos dados por parte dos empregados e quaisquer alterações efetuadas às políticas de controlo de acesso, alcançando um elevado grau de governação dos recursos empresariais.

> [!NOTE]
> As novas funcionalidades descritas neste artigo são disponíveis na pré-visualização apenas nos seguintes tipos de cluster: Hadoop, Spark e consulta interativo. Oozie está agora ativada em clusters associados a um domínio. Para poder aceder a Oozie utilizadores de IU da web deverá ativar [túnel](../hdinsight-linux-ambari-ssh-tunnel.md)

## <a name="benefits"></a>Benefícios
Segurança da empresa contém quatro principais pillars – segurança de perímetro, a autenticação, autorização e encriptação.

![Pilares de vantagens de clusters do HDInsight associados a um domínio](./media/apache-domain-joined-introduction/hdinsight-domain-joined-four-pillars.png).

### <a name="perimeter-security"></a>Segurança de Perímetro
A segurança de perímetro no HDInsight é assegurada através de redes virtuais e do serviço de Gateway. Hoje em dia, um administrador de empresa pode criar um cluster do HDInsight no interior de uma rede virtual e utilizar grupos de segurança de rede (regras de firewall) para restringir o acesso à rede virtual. Apenas os endereços IP definidos nas regras da firewall de entrada poderão comunicar com o cluster do HDInsight, fornecendo segurança de perímetro. Outra camada de segurança de perímetro é assegurada através do serviço de Gateway. O Gateway é o serviço, que atua como primeira linha de defesa para qualquer pedido de entrada para o cluster do HDInsight. Aceita o pedido, valida-o e apenas nessa altura permite que seja transmitido a outros nós do cluster, fornecendo segurança de perímetro para outros nós de nomes e dados do cluster.

### <a name="authentication"></a>Autenticação
Um administrador de empresa pode criar um cluster do HDInsight associados a um domínio, num [rede virtual](https://azure.microsoft.com/services/virtual-network/). Os nós do cluster do HDInsight serão associados ao domínio gerido pela empresa. Isto é assegurado através da utilização de [Azure Active Directory Domain Services](../../active-directory-domain-services/active-directory-ds-overview.md). Todos os nós do cluster estão associados a um domínio gerido pela empresa. Com esta configuração, os empregados da empresa podem iniciar sessão nos nós do cluster através das respetivas credenciais de domínio. Também podem utilizar as respetivas credenciais de domínio para autenticar com outros pontos finais aprovados, como as vistas Ambari, ODBC, JDBC, PowerShell e REST APIs para interagir com o cluster. O administrador tem controlo total sobre a limitação do número de utilizadores que interagem com o cluster através destes pontos finais.

### <a name="authorization"></a>Autorização
Uma melhor prática seguida pela maioria das empresas é que nem todos os empregados têm acesso a todos os recursos da empresa. Da mesma forma, com esta versão, o administrador pode definir políticas de controlo de acesso baseado em funções para os recursos de cluster. Por exemplo, o administrador pode configurar o [Apache Ranger](http://hortonworks.com/apache/ranger/) para definir políticas de controlo de acesso para o Hive. Esta funcionalidade garante que os empregados poderão aceder apenas à quantidade de dados de que precisam para terem sucesso nas suas tarefas. O acesso SSH ao cluster também está restrito apenas ao administrador.

### <a name="auditing"></a>Auditoria
Juntamente com a proteção dos recursos do cluster do HDInsight contra utilizadores não autorizados e a proteção dos dados, é necessário auditar todo o acesso aos recursos do cluster e os dados para controlar o acesso não autorizado ou não intencional dos recursos. O administrador pode ver e comunicar todo o acesso a recursos de cluster do HDInsight e os dados. O administrador também pode ver e comunicar todas as alterações às políticas de controlo de acesso efetuadas nos pontos finais suportados do Ranger Apache. Um cluster do HDInsight associado a um domínio utiliza a IU familiar do Ranger Apache para procurar os registos de auditoria. No back-end, o Ranger utiliza o [Apache Solr](http://hortonworks.com/apache/solr/) para armazenar e procurar os registos.

### <a name="encryption"></a>Encriptação
A proteção de dados é importante para cumprir os requisitos de segurança e de conformidade organizacionais e, juntamente com a restrição do acesso aos dados de empregados não autorizados, deve também fornecer proteção através de encriptação. Ambos os arquivos de dados para clusters do HDInsight, o Azure Storage Blob e o Armazenamento do Azure Data Lake, suportam [encriptação de dados](../../storage/common/storage-service-encryption.md) inativos transparente do lado do servidor. Proteger clusters do HDInsight perfeitamente funcionarão com esta encriptação do lado do servidor de dados na capacidade de rest.

## <a name="next-steps"></a>Passos Seguintes
* [Planear para clusters do HDInsight associados a um domínio](apache-domain-joined-architecture.md).
* [Configurar clusters do HDInsight associados a um domínio](apache-domain-joined-configure.md).
* [Gerir clusters do HDInsight associados a um domínio](apache-domain-joined-manage.md).
* [Configurar políticas de ramo de registo para clusters do HDInsight associados a um domínio](apache-domain-joined-run-hive.md).
* [Utilizar o SSH com o HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

