---
title: Script de execução do Azure Data Lake U-SQL no seu computador local | Documentos da Microsoft
description: Saiba como utilizar o Azure Data Lake Tools para Visual Studio para executar tarefas de U-SQL no seu computador local.
services: data-lake-analytics
documentationcenter: ''
author: yanancai
manager: ''
editor: ''
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/03/2018
ms.author: yanacai
ms.openlocfilehash: a7f43c7e17f36d9b4e0767744eee9604c2628ea8
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37888971"
---
# <a name="run-u-sql-script-on-your-local-machine"></a>Execute o script de U-SQL no seu computador local

Ao desenvolver scripts U-SQL, é comum a ser executado o script U-SQL localmente conforme poupa o custo e o tempo. O Azure Data Lake Tools para Visual Studio oferece suporte para executar scripts U-SQL no seu computador local. 

## <a name="basic-concepts-for-local-run"></a>Conceitos básicos para execução local

Abaixo do gráfico mostra os componentes para executar local e como esses componentes mapeiam para executar de cloud.

|Componente|Executar local|Execução de cloud|
|---------|---------|---------|
|Armazenamento|Pasta de raiz de dados local|Conta padrão do Azure Data Lake Store|
|Computação|Motor de execução local do U-SQL|Serviço do Azure Data Lake Analytics|
|Ambiente de execução|Diretório de trabalho no computador local|Cluster do Azure Data Lake Analytics|

Mais de explicação para a execução Local componentes:

### <a name="local-data-root-folder"></a>Pasta de raiz de dados local

Pasta de raiz de dados local é um "armazenamento local" para a conta de computação local. Qualquer pasta no sistema de arquivos local no seu computador local pode ser uma pasta de raiz de dados local. É igual para a conta do Azure Data Lake Store predefinida de uma conta do Data Lake Analytics. Mudar para uma pasta de raiz de dados diferente é igual a mudar para uma conta de armazenamento predefinido diferente. 

A pasta raiz de dados é utilizada para:
- Store metadados, como bases de dados, tabelas, as funções com valor de tabela e assemblies.
- Procure os caminhos de entrada e saídos que são definidos como caminhos relativos no script de U-SQL. Utilizar caminhos relativos torna mais fácil de implantar seus scripts U-SQL para o Azure.

### <a name="u-sql-local-run-engine"></a>Motor de execução local do U-SQL

Motor de execução local do U-SQL é uma "conta de computação local" para as tarefas de U-SQL. Os utilizadores podem executar tarefas U-SQL localmente através do Azure Data Lake Tools para Visual Studio. Execução local também é suportada por meio de interfaces de programação e de linha de comandos do SDK do Azure Data Lake U-SQL. [Saiba mais sobre o SDK do Azure Data Lake U-SQL](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/).

### <a name="working-directory"></a>Diretório de trabalho

Ao executar um script de U-SQL, uma pasta de diretório de trabalho é necessário para colocar em cache os resultados de compilação, registos de execução e assim por diante. No Azure Data Lake Tools para Visual Studio, o diretório de trabalho é o diretório de trabalho do projeto U-SQL (normalmente localizados em `<U-SQL project root path>/bin/debug>`). O diretório de trabalho será limpa sempre que uma nova execução acionada.

## <a name="local-run-in-visual-studio"></a>Execução no Visual Studio local

Azure Data Lake Tools para Visual Studio tem um motor de execução de local incorporado e apresenta-lo como local de computação de conta. Para executar um script U-SQL localmente, selecione (máquina Local) ou a conta (projeto Local) na margem de editor de script lista pendente e clique em **submeter**.

![Data Lake Tools para o script de envio do Visual Studio para a conta local](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-submit-script-to-local-account.png) 
 
## <a name="local-run-with-local-machine-account"></a>Execução com a conta (máquina Local) local

Conta (máquina local) é uma conta de computação local compartilhado com uma única pasta de raiz de dados local, como a conta de armazenamento local. A pasta de raiz de dados está por predefinição, localizada em "C:\Users\<nome de utilizador > \AppData\Local\USQLDataRoot", também é configurável através de **ferramentas > Data Lake > Opções e definições**.

![Configurar o Data Lake Tools para Visual Studio raiz de dados locais](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-configure-local-data-root.png)
  
Um projeto U-SQL é necessário para execução local. Diretório de trabalho do projeto U-SQL é utilizado para o diretório de trabalho de execução local do U-SQL. Resultados de compilação, registos de execução e outros ficheiros relacionados com a execução de tarefa são gerados e armazenados na pasta de diretório de trabalho durante a execução local. Tenha em atenção que sempre que executar novamente o script, todos estes ficheiros no diretório de trabalho serão limpos e gerada novamente.

## <a name="local-run-with-local-project-account"></a>Execução com a conta (projeto Local) local

Conta (projeto local) é uma conta de computação local isolado de projeto para cada projeto com a pasta de raiz de dados local isolado. Cada projeto U-SQL ativo abrir no Explorador de soluções tem um correspondente `(Local-project: <project name>)` conta listada ambos na margem de editor de script do Explorador de servidores e U-SQL. 

A conta de (projeto Local) fornece um ambiente de desenvolvimento limpo e isolada para desenvolvedores. Ao contrário de conta (máquina Local) que tenha uma pasta raiz de dados local compartilhada, armazenar metadados e dados de entrada/saída para todas as tarefas de locais, conta (projeto Local) cria uma pasta de raiz de dados local temporária sob o diretório de trabalho do projeto U-SQL sempre que um script de U-SQL é executada. Esta pasta raiz de dados temporária removida quando acontece recompilação ou volte a executar. 

Projeto U-SQL fornece uma boa experiência para gerir esta execução ambiente por meio de referência do projeto e a propriedade de local isolado. Pode ambos configurar as origens de dados de entrada para scripts U-SQL no projeto, bem como os ambientes de banco de dados referenciada.

### <a name="manage-input-data-source-for-local-project-account"></a>Gerir a origem de dados de entrada para a conta (projeto Local)

O projeto de U-SQL se encarrega da criação de pastas de raiz de dados local e os dados definição se a conta de (projeto Local). Uma pasta de raiz de dados temporária é limpo e recriada sob o diretório de trabalho do projeto U-SQL sempre acontece de recompilação e de execução local. Todas as origens de dados configuradas pelo projeto U-SQL são copiadas para esta pasta temporária de raiz de dados local antes da execução de tarefa local. 

Pode configurar a pasta raiz de origens de dados por meio **clique com o botão direito do rato em projeto U-SQL > propriedades > origem de dados de teste**. Ao executar o script U-SQL na conta (projeto Local), todos os ficheiros e subpastas (incluindo ficheiros em subpastas) **origem de dados de teste** pasta são copiados para a pasta de raiz de dados temporária local. Após a conclusão da execução de tarefa local, resultados de saída também podem ser encontrados na pasta raiz de dados de local temporária no diretório de trabalho do projeto. Tenha em atenção que todas essas saídas serão eliminadas e limpos quando o projeto obtém reconstruído e limpos. 

![Ferramentas do Data Lake para Visual Studio configurar origem de dados de teste do projeto](./media/data-lake-analytics-data-lake-tools-local-run/data-lake-tools-configure-project-test-data-source.png)

### <a name="manage-referenced-database-environment-for-local-project-account"></a>Gerir o ambiente de base de dados referenciada para a conta (projeto Local) 

Se utiliza ou consultas com objetos de banco de dados U-SQL de consulta U-SQL, deve certificar os ambientes de banco de dados pronto localmente antes de executar este script de U-SQL localmente. Para a conta (projeto Local), as dependências do U-SQL da base de dados podem ser geridas pelas referências de projeto U-SQL. Pode adicionar referências de projeto de banco de dados U-SQL ao seu projeto de U-SQL. Antes de executar scripts U-SQL na conta (projeto Local), todas as bases de dados referenciadas são implantados na pasta de raiz de dados temporário local. E para cada execução, a pasta raiz de dados temporária é limpa como um novo ambiente isolado.

Artigos relacionados:
* [Saiba como gerir a definição de base de dados U-SQL através do projeto de banco de dados U-SQL](data-lake-analytics-data-lake-tools-develop-usql-database.md#reference-a-u-sql-database-project)
* [Saiba como gerir a referência de base de dados U-SQL no projeto U-SQL](data-lake-analytics-data-lake-tools-develop-usql-database.md)

## <a name="difference-between-local-machine-and-local-project-account"></a>Diferença entre (máquina Local) e a conta (projeto Local)

Conta (máquina local) tem como objetivo para simular uma conta do Azure Data Lake Analytics no computador local de usuários. Ele compartilha a mesma experiência com a conta do Azure Data Lake Analytics. (Projeto local) tem como objetivo fornecer um ambiente de desenvolvimento local fácil de utilizar que ajuda os utilizadores a implementar as referências de bases de dados e dados de entrada antes de executar o script localmente. Conta (máquina local) fornece um ambiente compartilhado permanente, que pode ser acessado por meio de todos os projetos. Conta (projeto local) fornece um ambiente de desenvolvimento isolado para cada projeto e ela é atualizada para cada execução. Com base na acima, conta (projeto Local) oferece uma experiência de desenvolvimento mais rápida ao aplicar as novas alterações rapidamente.

Pode encontrar mais diferença entre (máquina Local) e a conta (projeto Local) no gráfico da seguinte forma:

|Ângulo de diferença|(Computador local)|(Projeto local)|
|----------------|---------------|---------------|
|Acesso local|Pode ser acedido por todos os projetos|Apenas o projeto correspondente pode aceder a esta conta|
|Pasta raiz de dados local|Uma pasta local permanente. Configurado por meio de **ferramentas > Data Lake > Opções e definições**|Uma pasta temporária criada para cada projeto U-SQL, diretório de trabalho de execução local. A pasta removida quando acontece recompilação ou volte a executar|
|Dados de entrada para o script U-SQL|Caminho relativo na pasta raiz de dados de local permanente|Definido por meio **propriedade de projeto U-SQL > origem de dados de teste**. Todos os ficheiros, as subpastas são copiados para a pasta de raiz de dados temporária antes da execução local|
|Dados de saída para o script U-SQL|Caminho relativo na pasta raiz de dados de local permanente|Saída para a pasta de raiz de dados temporária. Os resultados são limpos quando acontece recompilação ou volte a executar.|
|Implementação de base de dados referenciada|Bases de dados referenciadas não são implementadas automaticamente quando em execução na conta (máquina Local). Mesmo para o envio para a conta do Azure Data Lake Analytics.|Bases de dados referenciadas são implementadas para a conta (projeto Local) automaticamente antes de execução local. Todos os ambientes de banco de dados são limpos e implantados novamente quando acontece recompilação ou volte a executar.|

## <a name="local-run-with-u-sql-sdk"></a>Execução com o SDK U-SQL local

Além de executar scripts U-SQL localmente no Visual Studio, também pode utilizar o SDK do Azure Data Lake U-SQL para executar scripts U-SQL localmente com interfaces de programação e de linha de comandos. Através dessas interfaces, pode automatizar a execução local do U-SQL e teste.

[Saiba mais sobre o SDK do Azure Data Lake U-SQL](data-lake-analytics-u-sql-sdk.md).

## <a name="next-steps"></a>Próximos Passos

- [SDK do Azure Data Lake U-SQL](data-lake-analytics-u-sql-sdk.md)
- [Como configurar o pipeline CI/CD para o Azure Data Lake Analytics](data-lake-analytics-cicd-overview.md)
- [Utilizar o projeto de banco de dados U-SQL para desenvolver a base de dados U-SQL](data-lake-analytics-data-lake-tools-develop-usql-database.md)
- [Como testar seu código do Azure Data Lake Analytics](data-lake-analytics-cicd-test.md)
