---
title: Exportar uma base de dados do U-SQL com o Azure Data Lake Tools para Visual Studio | Microsoft Docs
description: Saiba como utilizar o Azure Data Lake Tools para Visual Studio para exportar uma base de dados do U-SQL e importe-o automaticamente para uma conta local.
services: data-lake-analytics
documentationcenter: 
author: yanancai
manager: 
editor: 
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/27/2017
ms.author: yanacai
ms.openlocfilehash: 441606258f9541c9552925e7c0cbc9b3a9effb4d
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2017
---
# <a name="export-a-u-sql-database"></a>Exportar uma base de dados do U-SQL

Neste artigo, saiba como utilizar [ferramentas do Azure Data Lake para Visual Studio](http://aka.ms/adltoolsvs) para exportar uma base de dados do U-SQL como um script único do U-SQL e de recursos transferidos. Pode importar a base de dados exportada para uma conta local no mesmo processo.

Os clientes, normalmente, mantêm vários ambientes de desenvolvimento, teste e produção. Nestes ambientes alojados em ambos os uma conta local, de computador local um programador e numa conta do Azure Data Lake Analytics no Azure. 

Quando desenvolver e otimizar as consultas de U-SQL em ambientes de desenvolvimento e teste, os programadores, muitas vezes, tem de recriar a trabalhar numa base de dados de produção. O Assistente de exportação de base de dados ajuda a acelerar este processo. Utilizando o assistente, os programadores podem clonar o ambiente de base de dados existente e dados de exemplo para outras contas de Data Lake Analytics.

## <a name="export-steps"></a>Passos de exportação

### <a name="step-1-export-the-database-in-server-explorer"></a>Passo 1: Exportar a base de dados no Explorador de servidores

Todas as contas de Data Lake Analytics que tem permissões para estão listadas no Explorador de servidores. Para exportar a base de dados:

1. No Explorador de servidores, expanda a conta que contém a base de dados que pretende exportar.
2. Faça duplo clique na base de dados e, em seguida, selecione **exportar**. 
   
    ![Explorador de servidores - exportar a base de dados](./media/data-lake-analytics-data-lake-tools-export-database/export-database.png)

     Se o **exportar** opção de menu não estiver disponível, terá de [a ferramenta de atualização para a versão lasted](http://aka.ms/adltoolsvs).

### <a name="step-2-configure-the-objects-that-you-want-to-export"></a>Passo 2: Configurar os objetos que pretende exportar

Se tiver apenas numa pequena parte de uma base de dados grande, pode configurar um subconjunto de objetos que pretende exportar no Assistente de exportação. 

A ação de exportação é concluída ao executar uma tarefa de U-SQL. Por conseguinte, exportar a partir de uma conta do Azure implica alguns custos.

![Assistente de exportação de base de dados - selecione Exportar objetos](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard.png)

### <a name="step-3-check-the-objects-list-and-other-configurations"></a>Passo 3: Verificar a lista de objetos e outras configurações

Neste passo, pode verificar os objetos selecionados a **lista de objetos de exportação** caixa. Se existirem erros, selecione **anterior** para voltar atrás e configurar corretamente os objetos que pretende exportar.

Pode também configurar outras definições para o destino de exportação. Descrições de configuração estão listadas na seguinte tabela:

|Configuração|Descrição|
|-------------|-----------|
|Nome de destino|Este nome indica onde pretende guardar os recursos de base de dados exportada. Os exemplos são assemblagens, ficheiros adicionais e dados de exemplo. É criada uma pasta com este nome na sua pasta de raiz de dados local.|
|Diretório do projeto|Este caminho define onde pretende guardar o script U-SQL exportado. Todas as definições de objetos de base de dados são guardadas nesta localização.|
|Apenas o esquema|Se selecionar esta opção, apenas as definições de base de dados e recursos (como assemblagens e ficheiros adicionais) são exportados.|
|Esquema e de dados|Se selecionar esta opção, as definições de base de dados, recursos e os dados são exportados. As linhas N superiores de tabelas são exportadas.|
|Importar automaticamente a base de dados Local|Se selecionar esta opção, a base de dados exportada é importado automaticamente para a base de dados local ao exportar é concluído.|

![Assistente de exportação de base de dados - lista de objetos de exportação e outras configurações](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard-configuration.png)

### <a name="step-4-check-the-export-results"></a>Passo 4: Verificar os resultados de exportação

Quando exportar estiver concluída, pode ver os resultados exportados na janela de registo no assistente. O exemplo seguinte mostra como encontrar exportados U-SQL script e de base de dados de recursos, incluindo as assemblagens, ficheiros adicionais e dados de exemplo:

![Assistente de exportação de base de dados - resultados de exportação](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard-completed.png)

## <a name="import-the-exported-database-to-a-local-account"></a>Importar a base de dados exportada para uma conta local

A forma mais conveniente para importar a base de dados exportada é para selecionar o **importar para o Local da base de dados automaticamente** caixa de verificação durante o processo de exportar no passo 3. Se não selecionar esta caixa, em primeiro lugar, localize o script U-SQL exportado no registo de exportação. Em seguida, execute o script de U-SQL localmente para importar a base de dados para a conta local.

## <a name="import-the-exported-database-to-a-data-lake-analytics-account"></a>Importar a base de dados exportada para uma conta de Data Lake Analytics

Para importar a base de dados para outra conta de Data Lake Analytics:

1. Carregar os recursos exportados, incluindo as assemblagens, ficheiros adicionais e dados de exemplo, para a conta do Azure Data Lake Store predefinida da conta do Data Lake Analytics que pretende importar para. Pode encontrar a pasta de recurso exportado na pasta raiz de dados local. Carregue toda a pasta raiz da conta de Data Lake Store predefinida.
2. Quando o carregamento estiver concluído, submeta o script de U-SQL exportado para a conta de Data Lake Analytics que pretende importar a base de dados.

## <a name="known-limitations"></a>Limitações conhecidas

Atualmente, se selecionar a **esquema e dados** opção no passo 3, a ferramenta executa uma tarefa de U-SQL para exportar os dados armazenados nas tabelas. Por este motivo, os processo de exportação de dados podem ser lentos e pode implicar custos. 

## <a name="next-steps"></a>Passos seguintes

* [Saiba mais sobre bases de dados do U-SQL](https://msdn.microsoft.com/library/azure/mt621299.aspx) 
* [Testar e depurar tarefas U-SQL utilizando a execução local e o SDK U-SQL do Azure Data Lake](data-lake-analytics-data-lake-tools-local-run.md)


