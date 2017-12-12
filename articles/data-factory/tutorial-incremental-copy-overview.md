---
title: Copiar dados de forma incremental com o Azure Data Factory | Microsoft Docs
description: 'Estes tutoriais mostram-lhe como copiar dados de forma incremental de um arquivo de dados de origem para um arquivo de dados de destino. O primeiro copia dados a partir de uma tabela. '
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/05/2017
ms.author: shlo
ms.openlocfilehash: 2ae6cb42685dfb227bd75f83e73dfdf646ab909f
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2017
---
# <a name="incrementally-load-data-from-a-source-data-store-to-a-destination-data-store"></a>Carrega dados de forma incremental a partir de um arquivo de dados de origem para um arquivo de dados de destino

Uma solução de integração de dados, que carrega dados incrementalmente (ou delta) após um carregamento de dados completos inicial é um cenário bastante utilizado. Os tutoriais nesta secção mostram formas diferentes de carregar dados de forma incremental através da utilização do Azure Data Factory versão 2.

## <a name="delta-data-loading-using-a-watermark"></a>Carregamento de dados delta com uma marca d'água
Neste caso, é possível definir uma marca d'água na base de dados de origem. Uma marca d'água é uma coluna com o último carimbo de data/hora atualizado ou uma chave de incrementação. A solução de carregamento delta carrega os dados alterados entre uma marca d'água antiga e uma nova marca d'água. O fluxo de trabalho para esta abordagem é descrito no diagrama seguinte: 

![Fluxo de trabalho para a utilização de uma marca d'água](media/tutorial-incremental-copy-overview/workflow-using-watermark.png)

Para obter instruções passo a passo, veja os seguintes tutoriais: 

- [Copiar dados de forma incremental de uma tabela na Base de Dados SQL do Azure para o Armazenamento de Blobs do Azure](tutorial-incremental-copy-powershell.md)
- [Copiar dados de forma incremental a partir de várias tabelas numa SQL Server no local para a Base de Dados SQL do Azure](tutorial-incremental-copy-multiple-tables-powershell.md)


## <a name="delta-data-loading-using-the-change-tracking-technology"></a>Carregamento de dados delta utilizando a tecnologia de Controlo de Alterações
A tecnologia de controlo de alterações é uma solução simples no SQL Server e na Base de Dados SQL do Azure que fornece um mecanismo de controlo de alterações eficiente para aplicações. Permite que uma aplicação facilmente identifique dados que foram inseridos, atualizados ou eliminados. 

O fluxo de trabalho para esta abordagem é descrito no diagrama seguinte:

![Fluxo de trabalho para utilizar o Controlo de Alterações](media/tutorial-incremental-copy-overview/workflow-using-change-tracking.png)

Para obter instruções passo a passo, veja o seguinte tutorial: <br/>
[Copiar dados de forma incremental da Base de Dados SQL do Azure para o Armazenamento de Blobs do Azure com tecnologia de Controlo de Alterações](tutorial-incremental-copy-change-tracking-feature-powershell.md)


## <a name="next-steps"></a>Passos seguintes
Avançar para o tutorial seguinte: 

> [!div class="nextstepaction"]
>[Copiar dados de forma incremental de uma tabela na Base de Dados SQL do Azure para o Armazenamento de Blobs do Azure](tutorial-incremental-copy-powershell.md)
