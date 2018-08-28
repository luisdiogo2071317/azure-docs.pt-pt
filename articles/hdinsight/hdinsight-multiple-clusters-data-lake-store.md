---
title: Utilizar vários clusters do HDInsight com uma conta do Azure Data Lake Store - Azure
description: Saiba como utilizar mais de um cluster do HDInsight com uma única conta do Data Lake Store
keywords: armazenamento do hdinsight, hdfs, dados estruturados, dados não estruturados, data lake store
services: hdinsight,storage
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: jasonh
ms.openlocfilehash: 0e74406d79484483053ca7d4b89b096c3f70e298
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43095129"
---
# <a name="use-multiple-hdinsight-clusters-with-an-azure-data-lake-store-account"></a>Utilizar vários clusters do HDInsight com uma conta do Azure Data Lake Store

A partir do HDInsight versão 3.5, pode criar clusters do HDInsight com contas do Azure Data Lake Store como o sistema de ficheiros predefinido.
Data Lake Store suporta armazenamento ilimitado, que o torna ideal não apenas para o alojamento de grandes quantidades de dados. mas também para o alojamento de vários HDInsight clusters que a partilha de uma única conta do Data Lake Store. Para obter instruções sobre como criar um cluster do HDInsight com Data Lake Store como armazenamento, consulte [início rápido: configurar clusters no HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).

Este artigo fornece recomendações para o Data Lake armazenam o administrador para configurar um único e partilhado conta Data Lake Store que podem ser utilizadas em vários **Active Directory** clusters do HDInsight. Estas recomendações aplicam-se ao alojamento de vários clusters do Hadoop seguros, bem como não segura numa conta do Data Lake store partilhado.


## <a name="data-lake-store-file-and-folder-level-acls"></a>ACLs de nível de arquivo do Data Lake Store e pasta

O restante deste artigo parte do princípio de que tem um bom conhecimento do nível de arquivo e pasta ACLs no Azure Data Lake Store, que é descrito detalhadamente no [controlo de acesso no Azure Data Lake Store](../data-lake-store/data-lake-store-access-control.md).

## <a name="data-lake-store-setup-for-multiple-hdinsight-clusters"></a>Configuração do Data Lake Store para múltiplos clusters do HDInsight
Vamos ver mais uma hierarquia de pastas de dois níveis para explicar as recomendações para a utilização de clusters do HDInsight mutilple com uma conta do Data Lake Store. Considere a tem uma conta do Data Lake Store com a estrutura de pastas **/clusters/finance**. Com esta estrutura, todos os clusters necessários para a organização do departamento financeiro podem utilizar /clusters/finance como localização de armazenamento. No futura, se outra organização, digamos que pretende criar o HDInsight clusters com a mesma conta do Data Lake Store, foi possível criar/clusters/marketing, de Marketing. Por enquanto, vamos simplesmente usar **/clusters/finance**.

Para ativar esta estrutura de pastas para ser usado com eficiência pelos clusters do HDInsight, o administrador do Data Lake Store tem de atribuir as permissões apropriadas, conforme descrito na tabela. As permissões mostradas na tabela correspondem às ACLs de acesso e não de ACLs padrão. 


|Pasta  |Permissões  |Utilizador proprietário  |Grupo proprietário  | Utilizador nomeado | Permissões de utilizador nomeado | Grupo nomeado | Permissões de grupo nomeado |
|---------|---------|---------|---------|---------|---------|---------|---------|
|/ | rwxr-x--x  |administrador |administrador  |Principal de serviço |--x  |FINGRP   |r-x         |
|/clusters | rwxr-x--x |administrador |administrador |Principal de serviço |--x  |FINGRP |r-x         |
|/ clusters/Finanças | rwxr-x--t |administrador |FINGRP  |Principal de serviço |rwx  |-  |-     |

Na tabela,

- **administrador** é o criador e o administrador da conta do Data Lake Store.
- **Principal de serviço** é o principal de serviço do Azure Active Directory (AAD) associado à conta.
- **FINGRP** é um grupo de utilizadores criado neste que contenha os utilizadores da organização do departamento financeiro.

Para obter instruções sobre como criar uma aplicação do AAD (que também cria um Principal de serviço), consulte [criar uma aplicação AAD](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application). Para obter instruções sobre como criar um grupo de utilizadores no AAD, consulte [gerir grupos no Azure Active Directory](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

Alguns pontos importantes a considerar.

- Estrutura de pastas de nível de dois (**/clusters/Finanças/**) tem de ser criado e provisionado com permissões adequadas pelo administrador do Data Lake Store **antes de** com a conta de armazenamento para clusters. Essa estrutura não é criada automaticamente durante a criação de clusters.
- O exemplo acima recomenda a configuração do grupo proprietário da **/clusters/finance** como **FINGRP** e permitindo **r-x** acesso a FINGRP para a hierarquia de toda a pasta a partir de raiz. Isto garante que os membros de FINGRP podem navegar a estrutura de pastas a partir de raiz.
- No caso de quando os principais de serviço do AAD diferente pode criar clusters **/clusters/finance**, o sticky bit (quando definidos na **Finanças** pasta) garante que as pastas criadas por um Principal de serviço Não é possível eliminar por outros.
- Assim que a estrutura de pastas e as permissões estiverem em vigor, o processo de criação de cluster do HDInsight cria um loaction específicos do cluster de armazenamento em **/clusters/Finanças/**. Por exemplo, poderia ser o armazenamento para um cluster com o nome fincluster01 **/clusters/finance/fincluster01**. A propriedade e permissões para as pastas criadas pelo cluster do HDInsight é mostrado na tabela aqui.

    |Pasta  |Permissões  |Utilizador proprietário  |Grupo proprietário  | Utilizador nomeado | Permissões de utilizador nomeado | Grupo nomeado | Permissões de grupo nomeado |
    |---------|---------|---------|---------|---------|---------|---------|---------|
    |/clusters/finanace/fincluster01 | rwxr-x---  |Principal de serviço |FINGRP  |- |-  |-   |-  | 
   


## <a name="recommendations-for-job-input-and-output-data"></a>Recomendações para dados de entrada e saídos da tarefa

Recomendamos que os dados de entrada para uma tarefa e os resultados de uma tarefa ser armazenados numa pasta fora **/clusters**. Isto garante que, mesmo que a pasta de específicos do cluster é eliminada para recuperar algum espaço de armazenamento, as tarefas entradas e saídas ainda estão disponíveis para utilização futura. Nesse caso, certifique-se de que a hierarquia de pastas para armazenar as tarefa entradas e saídas permite um nível apropriado de acesso para o Principal de serviço.

## <a name="limit-on-clusters-sharing-a-single-storage-account"></a>Limite para o compartilhamento de uma única conta de armazenamento de clusters

O limite no número de clusters que pode partilhar uma única conta do Data Lake Store depende da carga de trabalho a ser executada nesses clusters. Ter demasiadas clusters ou cargas de trabalho muito pesadas nos clusters que partilham uma conta de armazenamento pode fazer com que a conta entrada/saída de armazenamento ficar bloqueados.

## <a name="support-for-default-acls"></a>Suporte para ACLs padrão

Ao criar um Principal de serviço com acesso de utilizador com o nome (conforme mostrado na tabela acima), recomendamos **não** adicionar o utilizador com o nome com uma ACL predefinida. Aprovisionamento de utilizador com o nome de acesso usando resultados de ACLs padrão na atribuição de 770 permissões para o utilizador proprietário, grupo proprietário e outros. Apesar deste valor predefinido de 770 não distância permissões de proprietário-utilizador (7) ou o grupo de proprietário (7), ele faz com que todas as permissões para outras pessoas (0). Isso resulta num problema conhecido com um uso-caso específico que é abordado em detalhe no [conhecidos de problemas e soluções alternativas](#known-issues-and-workarounds) secção.

## <a name="known-issues-and-workarounds"></a>Problemas conhecidos e soluções alternativas

Esta secção lista os problemas conhecidos para utilizar o HDInsight com Data Lake Store e suas soluções alternativas.

### <a name="publicly-visible-localized-yarn-resources"></a>Visíveis publicamente recursos localizados de YARN

Quando é criada uma nova conta de armazenamento do Azure Data Lake, o diretório de raiz é automaticamente aprovisionado com bits de permissão de ACL de acesso definidos como 770. A pasta de raiz ao proprietário utilizador está definido para o utilizador que criou a conta (o administrador do Data Lake Store) e o grupo proprietário está definido para o grupo principal do utilizador que criou a conta. Sem acesso é fornecido para "outros".

Estas definições são conhecidas a afetar um específico HDInsight casos de utilização capturado na [YARN 247](https://hwxmonarch.atlassian.net/browse/YARN-247). Submissões de tarefas poderá falhar com uma mensagem de erro semelhante a este:

    Resource XXXX is not publicly accessible and as such cannot be part of the public cache.

Conforme indicado no JIRA YARN ligado anteriormente, durante a localização de recursos de público, o localizador valida que todos os recursos de pedido são públicos, de fato, verificando as respetivas permissões no sistema de ficheiros remoto. Qualquer LocalResource que não se ajusta a condição for rejeitado para a localização. A verificação de permissões, inclui o acesso de leitura para o ficheiro para "outros". Este cenário não funciona out-of-the-box quando alojar os clusters do HDInsight no Azure Data Lake, uma vez que o Azure Data Lake nega todo o acesso a "outros" no nível de pasta de raiz.

#### <a name="workaround"></a>Solução
Conjunto de leitura-permissões de execução para **outras pessoas** através da hierarquia, por exemplo, na **/**, **/clusters** e   **/clusters/Finanças** conforme mostrado na tabela acima.

## <a name="see-also"></a>Consulte também

* [Início rápido: Configurar clusters no HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md)


