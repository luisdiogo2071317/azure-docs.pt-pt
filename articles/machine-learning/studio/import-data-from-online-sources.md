---
title: Importar dados para Machine Learning Studio provenientes de origens de dados online | Documentos da Microsoft
description: Como importar os dados de formação do Azure Machine Learning Studio a partir de várias origens online.
keywords: Importar dados, formato de dados, os tipos de dados, origens de dados, dados de treinamento
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=hshapiro, author=heatherbshapiro)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: 701b93fe-765b-4d15-a1cf-9b607f17add6
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.openlocfilehash: 6412d431b0258d681d8d9bd8cb3a88798a0d81cd
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/20/2018
ms.locfileid: "52262904"
---
# <a name="import-data-into-azure-machine-learning-studio-from-various-online-data-sources-with-the-import-data-module"></a>Importar dados para o Azure Machine Learning Studio provenientes de várias origens de dados online com o módulo Importar Dados
Este artigo descreve o suporte para importação de dados online de várias origens e as informações necessárias para mover dados a partir destas origens para uma experimentação do Azure Machine Learning.

> [!NOTE]
> Este artigo fornece informações gerais sobre o [importar dados] [ import-data] módulo. Para obter mais informações sobre os tipos de dados pode aceder, formatos, parâmetros e respostas a perguntas comuns, consulte o tópico de referência de módulo para o [importar dados] [ import-data] módulo.
> 
> 

## <a name="introduction"></a>Introdução
Ao utilizar o [importar dados] [ import-data] módulo, pode acessar dados de uma das várias origens de dados online durante a execução sua experimentação [Azure Machine Learning Studio](https://studio.azureml.net/Home):

* Um URL de Web através de HTTP
* Hadoop com o HiveQL
* Armazenamento de blobs do Azure
* Tabela do Azure
* Base de dados SQL ou SQL Server numa VM do Azure do Azure
* Base de dados do SQL Server no local
* Um provedor de OData atualmente de feed de dados
* Azure Cosmos DB

Para aceder a origens de dados online na sua experimentação do Studio, adicione a [importar dados] [ import-data] módulo para sua, selecione o **origem de dados**e, em seguida, forneça os parâmetros necessários para aceder a os dados. As origens de dados online, que são suportadas estão descritas na tabela abaixo. Esta tabela também resume os formatos de ficheiro suportados e os parâmetros que são utilizados para aceder aos dados.

Tenha em atenção que uma vez que estes dados de treinamento são acedidos durante a execução de sua experiência, só está disponível desse experimento. Por comparação, os dados que tenham sido armazenados num módulo de conjunto de dados estão disponíveis para qualquer experimentação na sua área de trabalho.

> [!IMPORTANT]
> Atualmente, o [importar dados] [ import-data] e [exportar dados] [ export-data] módulos podem ler e escrever dados apenas a partir de armazenamento do Azure criado com clássica modelo de implementação. Em outras palavras, o novo tipo de conta de armazenamento de Blobs do Azure que oferece uma camada de acesso de armazenamento frequente ou de uma camada de acesso de armazenamento esporádico não ainda é suportado. 
> 
> Em geral, as contas de armazenamento do Azure que pode ter criado antes desta opção de serviço se tornar disponível não deve ser afetado. 
> Se precisar de criar uma nova conta, selecione **clássica** para a implementação de modelo, ou utilizar o Resource manager e selecione **fins gerais** vez **armazenamento de BLOBs** para  **Tipo de conta**. 
> 
> Para obter mais informações, consulte [armazenamento de Blobs do Azure: camadas de armazenamento frequente e esporádico](../../storage/blobs/storage-blob-storage-tiers.md).
> 
> 

## <a name="supported-online-data-sources"></a>Origens de dados online suportadas
O Azure Machine Learning **importar dados** módulo suporta as seguintes origens de dados:

| Origem de Dados | Descrição | Parâmetros |
| --- | --- | --- |
| URL de Web por meio de HTTP |Leia os dados em valores separados por vírgulas (CSV), valores separados por tabulações (TSV), o formato de ficheiro de relação de atributo (ARFF) e formatos de máquinas de Vetores de suporte (SVM luz), de qualquer URL da web que utiliza HTTP |<b>URL</b>: Especifica o nome completo do ficheiro, incluindo o URL do site e o nome de ficheiro, com qualquer extensão. <br/><br/><b>Formato de dados</b>: Especifica um os dados suportados formatos: CSV, TSV, ARFF ou SVM-light. Se os dados têm uma linha de cabeçalho, ele é usado para atribuir nomes de coluna. |
| Hadoop/HDFS |Leia os dados de armazenamento distribuído no Hadoop. Especifique os dados que pretende ao utilizar o HiveQL, uma linguagem de consulta do tipo SQL. Também pode ser utilizado o HiveQL para agregar dados e realizar a filtragem antes de adicionar os dados para Machine Learning Studio por data. |<b>Consulta de base de dados do Hive</b>: Especifica a consulta do Hive utilizada para gerar os dados.<br/><br/><b>URI do servidor de HCatalog </b> : especificado o nome do seu cluster com o formato  *&lt;o nome do cluster&gt;. azurehdinsight.net.*<br/><br/><b>Nome de conta de utilizador do Hadoop</b>: Especifica o nome de conta de utilizador do Hadoop utilizado para aprovisionar o cluster.<br/><br/><b>Palavra-passe de conta de utilizador Hadoop</b> : Especifica as credenciais utilizadas quando o aprovisionamento do cluster. Para obter mais informações, consulte [criar clusters Hadoop no HDInsight](../../hdinsight/hdinsight-provision-clusters.md).<br/><br/><b>Localização de dados de saída</b>: Especifica se os dados são armazenados num sistema de ficheiros distribuído Hadoop (HDFS) ou no Azure. <br/><ul>Se armazenar dados de saída no HDFS, especifique o URI do servidor HDFS. (Certifique-se de que utiliza o nome de cluster do HDInsight sem o prefixo HTTPS://). <br/><br/>Se armazenar os dados de saída no Azure, tem de especificar o nome da conta de armazenamento do Azure, a chave de acesso de armazenamento e o nome do contentor de armazenamento.</ul> |
| Base de dados SQL |Lê os dados armazenados numa base de dados SQL do Azure ou numa base de dados do SQL Server em execução numa máquina virtual do Azure. |<b>Nome do servidor de base de dados</b>: Especifica o nome do servidor no qual a base de dados está em execução.<br/><ul>Em caso de base de dados do Azure SQL, introduza o nome do servidor que é gerado. Normalmente, ela tem o formato  *&lt;generated_identifier&gt;. database.windows.net.* <br/><br/>Em caso de um SQL server alojado numa máquina Virtual do Azure, introduza *tcp:&lt;nome de DNS da Máquina Virtual&gt;, 1433*</ul><br/><b>Nome da base de dados </b>: Especifica o nome da base de dados no servidor. <br/><br/><b>Nome de conta de utilizador do servidor</b>: Especifica um nome de utilizador para uma conta que tenha permissões de acesso para a base de dados. <br/><br/><b>Palavra-passe de conta de utilizador servidor</b>: Especifica a palavra-passe da conta de utilizador.<br/><br/><b>Consulta de base de dados</b>: introduza uma instrução SQL que descreve os dados que pretende ler. |
| Base de dados SQL no local |Lê os dados armazenados numa base de dados SQL no local. |<b>Gateway de dados</b>: Especifica o nome do Gateway de gestão de dados instalado num computador em que ele pode acessar seu banco de dados do SQL Server. Para obter informações sobre como configurar o gateway, consulte [executar análises avançadas com o Azure Machine Learning utilizando dados de um servidor SQL no local](use-data-from-an-on-premises-sql-server.md).<br/><br/><b>Nome do servidor de base de dados</b>: Especifica o nome do servidor no qual a base de dados está em execução.<br/><br/><b>Nome da base de dados </b>: Especifica o nome da base de dados no servidor. <br/><br/><b>Nome de conta de utilizador do servidor</b>: Especifica um nome de utilizador para uma conta que tenha permissões de acesso para a base de dados. <br/><br/><b>Nome de utilizador e palavra-passe</b>: clique em <b>introduza valores</b> que introduza as credenciais da base de dados. Pode usar a autenticação integrada do Windows ou a autenticação do SQL Server, dependendo de como o SQL Server no local está configurado.<br/><br/><b>Consulta de base de dados</b>: introduza uma instrução SQL que descreve os dados que pretende ler. |
| Tabela do Azure |Lê dados a partir do serviço tabela no armazenamento do Azure.<br/><br/>Se leu grandes quantidades de dados com pouca frequência, utilize o serviço de tabela do Azure. Ele fornece flexível, não relacionais (NoSQL), a solução de armazenamento de elevada disponibilidade, económica e dimensionável em massa. |As opções de **importar dados** alterar dependendo se aceder a informações públicas ou uma conta de armazenamento privado que necessite de credenciais de início de sessão. Tal é determinado através da <b>tipo de autenticação</b> que pode ter um valor de "PublicOrSAS" ou "Conta", cada um com seu próprio conjunto de parâmetros. <br/><br/><b>Público ou assinatura partilhado de acesso (SAS) URI</b>: os parâmetros são:<br/><br/><ul><b>URI de tabela</b>: Especifica o público ou o URL de SAS para a tabela.<br/><br/><b>Especifica as linhas para verificar a existência de nomes de propriedade</b>: os valores são <i>TopN</i> para analisar o número especificado de linhas, ou <i>ScanAll</i> para obter todas as linhas na tabela. <br/><br/>Se os dados são homogêneos e previsível, recomenda-se que seleciona *TopN* e introduza um número para N. Para tabelas grandes, isso pode resultar em tempos de leitura mais rápidos.<br/><br/>Se os dados estão estruturados com conjuntos de propriedades que variam com base na profundidade e a posição da tabela, escolha o *ScanAll* opção para verificar todas as linhas. Isto garante a integridade da sua propriedade resultante e a conversão de metadados.<br/><br/></ul><b>Conta de armazenamento privado</b>: os parâmetros são: <br/><br/><ul><b>Nome da conta</b>: Especifica o nome da conta que contém a tabela de ler.<br/><br/><b>Chave da conta</b>: Especifica a chave de armazenamento associada à conta.<br/><br/><b>Nome da tabela</b> : Especifica o nome da tabela que contém os dados para ler.<br/><br/><b>Linhas para verificar a existência de nomes de propriedade</b>: os valores são <i>TopN</i> para analisar o número especificado de linhas, ou <i>ScanAll</i> para obter todas as linhas na tabela.<br/><br/>Se os dados forem homogêneos e previsível, recomendamos que seleciona *TopN* e introduza um número para N. Para tabelas grandes, isso pode resultar em tempos de leitura mais rápidos.<br/><br/>Se os dados estão estruturados com conjuntos de propriedades que variam com base na profundidade e a posição da tabela, escolha o *ScanAll* opção para verificar todas as linhas. Isto garante a integridade da sua propriedade resultante e a conversão de metadados.<br/><br/> |
| Armazenamento de Blobs do Azure |Lê os dados armazenados no serviço de BLOBs no armazenamento do Azure, incluindo imagens, texto não estruturado ou dados binários.<br/><br/>Pode utilizar o serviço de BLOBs para expor publicamente os dados ou para armazenar os dados da aplicação em privado. Pode aceder aos dados a partir de qualquer lugar usando conexões HTTP ou HTTPS. |As opções de **importar dados** módulo alteração dependendo se aceder a informações públicas ou uma conta de armazenamento privado que necessite de credenciais de início de sessão. Tal é determinado através da <b>tipo de autenticação</b> que pode ter um valor de "PublicOrSAS" ou "Conta".<br/><br/><b>Público ou assinatura partilhado de acesso (SAS) URI</b>: os parâmetros são:<br/><br/><ul><b>URI</b>: Especifica o público ou o URL de SAS para o blob de armazenamento.<br/><br/><b>Formato de arquivo</b>: Especifica o formato dos dados no serviço Blob. Os formatos suportados são CSV, TSV e ARFF.<br/><br/></ul><b>Conta de armazenamento privado</b>: os parâmetros são: <br/><br/><ul><b>Nome da conta</b>: Especifica o nome da conta que contém o blob que pretende ler.<br/><br/><b>Chave da conta</b>: Especifica a chave de armazenamento associada à conta.<br/><br/><b>Caminho para o contentor, diretório ou blob </b> : Especifica o nome do blob que contém os dados para ler.<br/><br/><b>Formato de ficheiro blob</b>: Especifica o formato dos dados no serviço blob. Os formatos de dados suportados são CSV, TSV, ARFF, CSV com uma codificação especificada e o Excel. <br/><br/><ul>Se o formato CSV ou TSV, certifique-se de que indicam se o ficheiro contém uma linha de cabeçalho.<br/><br/>Pode utilizar a opção do Excel para ler dados de livros do Excel. Na <i>formato de dados do Excel</i> opção, indique se os dados estão num intervalo de folha de cálculo do Excel ou numa tabela do Excel. Na <i>folha do Excel ou tabela embedded </i>opção, especifique o nome da folha de cálculo ou tabela que pretende ler a partir de.</ul><br/> |
| Fornecedor de Feeds de dados |Leia os dados de um provedor de feed de suporte. Atualmente apenas o formato de Open Data Protocol (OData) é suportado. |<b>Tipo de conteúdo de dados</b>: Especifica o formato OData.<br/><br/><b>URL de origem</b>: Especifica o URL completo para o feed de dados. <br/>Por exemplo, o seguinte URL lê a partir de dados Northwind de exemplo: http://services.odata.org/northwind/northwind.svc/ |

## <a name="next-steps"></a>Passos Seguintes

[Implantando serviços de web do Azure ML que utilizam módulos importar dados e exportação de dados](web-services-that-use-import-export-modules.md)


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C/
