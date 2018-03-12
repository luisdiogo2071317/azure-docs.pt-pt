---
title: "Descrição geral do Microsoft Azure Data Lake Analytics | Microsoft Docs"
description: "O Data Lake Analytics é um serviço do Azure Big Data que lhe permite utilizar dados para orientar a sua empresa com as informações obtidas a partir dos dados na cloud, independentemente do tamanho ou da localização."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 1e1d443a-48a2-47fb-bc00-bf88274222de
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/23/2017
ms.author: saveenr
ms.openlocfilehash: 316c35fa4b04bdb251c2b9ae14f6b32f4e4bf939
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/05/2018
---
# <a name="overview-of-microsoft-azure-data-lake-analytics"></a>Descrição geral do Microsoft Azure Data Lake Analytics

## <a name="what-is-azure-data-lake-analytics"></a>O que é o Azure Data Lake Analytics?
O Azure Data Lake Analytics é um serviço de tarefa de análise no local que simplifica os macrodados. Em vez de implementar, configurar e otimizar hardware, escreve consultas para transformar os dados e extrair informações valiosas. O serviço de análise pode processar tarefas de qualquer dimensionamento instantaneamente definindo a quantidade de potência necessária. Apenas paga a tarefa quando estiver a ser executada, tornando-a económica. O suporte do serviço de análise inclui U-SQL, uma linguagem que unifica as vantagens do SQL com o poder do código imperativo. O U-SQL permite-lhe analisar os dados através do Data Lake Store, o SQL Server no Azure, a Base de Dados SQL do Azure e o Azure SQL Data Warehouse.

## <a name="dynamic-scaling"></a>Dimensionamento dinâmico
  
O Data Lake Analytics é criado para dimensionamento e desempenho da cloud.  Aprovisiona dinamicamente os recursos e permite-lhe fazer análises sobre terabytes ou até mesmo exabytes de dados. Quando a tarefa é concluída, reduz os recursos automaticamente e paga apenas a potência de processamento utilizada. À medida que aumenta ou diminui o tamanho dos dados armazenados ou a quantidade de recursos de computação utilizados, não tem de reescrever o código. Pode concentrar-se apenas na lógica de negócio e não em como processar e armazenar grandes conjuntos de dados.

## <a name="develop-faster-debug-and-optimize-smarter-using-familiar-tools"></a>Desenvolver com maior rapidez, depurar e otimizar de forma mais inteligente com ferramentas familiares
  
O Data Lake Analytics tem uma profunda integração com o Visual Studio, pelo que pode utilizar ferramentas familiares para executar, depurar e otimizar o seu código. As visualizações das tarefas U-SQL permitem-lhe ver o modo de execução do código relativamente ao dimensionamento, para que possa identificar facilmente estrangulamentos de desempenho e otimizar os custos.


## <a name="u-sql-simple-and-familiar-powerful-and-extensible"></a>U-SQL: simples e familiar, poderoso e extensível
  
O Data Lake Analytics inclui U-SQL, uma linguagem de consulta que expande a natureza familiar, simples e declarativa do SQL Server com o poder expressivo do C#. A linguagem U-SQL foi criada com base no mesmo runtime distribuído que alimenta os sistemas de macrodados na Microsoft. Milhões de programadores SQL e .NET podem agora processar e analisar os seus dados com as capacidades que já têm.

## <a name="integrates-seamlessly-with-your-it-investments"></a>Integração total com os seus investimentos de TI
  
O Data Lake Analytics pode utilizar os investimentos de TI existentes para identidade, gestão, segurança e armazenamento de dados. Esta abordagem simplifica a governação de dados e facilita a expansão das aplicações de dados atuais. O Data Lake Analytics está integrada no Active Directory para gestão de utilizadores e permissões e é fornecida com monitorização e auditoria incorporadas.

## <a name="affordable-and-cost-effective"></a>Económica e acessível

O Data Lake Analytics é uma solução económica para executar cargas de trabalho de macrodados. Paga por tarefa quando os dados são processados. Não é necessário nenhum hardware, licenças ou contratos de suporte específicos do serviço. O sistema é aumentado ou reduzido verticalmente, de forma automática, à medida que a tarefa é iniciada e concluída, o que significa que nunca paga mais do que aquilo que precisa. [Saiba mais sobre como controlar os custos e poupar dinheiro](https://1drv.ms/f/s!AvdZLquGMt47h213Hg3rhl-Tym1c).
    
## <a name="works-with-all-your-azure-data"></a>Funciona com todos os Dados do Azure
  
O Data Lake Analytics pode ser utilizado com o Azure Data Lake Store a fim de proporcionar desempenho, débito e paralelização mais elevados, bem como com o Azure Storage Blob, a Base de Dados SQL do Azure e o Azure Warehouse.

## <a name="next-steps"></a>Passos seguintes
 
  * Introdução ao Data Lake Analytics com [portal do Azure portal](data-lake-analytics-get-started-portal.md) | [Azure PowerShell](data-lake-analytics-get-started-powershell.md) | [CLI](data-lake-analytics-get-started-cli2.md)
  * Efetue a gestão do Azure Data Lake Analytics com [portal do Azure](data-lake-analytics-manage-use-portal.md) | [Azure PowerShell](data-lake-analytics-manage-use-powershell.md) | [CLI](data-lake-analytics-manage-use-cli.md) | [Azure .NET SDK](data-lake-analytics-manage-use-dotnet-sdk.md) | [Node.js](data-lake-analytics-manage-use-nodejs.md)
  * [Como controlar os custos e poupar dinheiro com o Data Lake Analytics](https://1drv.ms/f/s!AvdZLquGMt47h213Hg3rhl-Tym1c)
