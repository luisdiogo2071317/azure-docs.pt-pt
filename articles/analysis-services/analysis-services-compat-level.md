---
title: Modelo de dados de nível de compatibilidade no Azure Analysis Services | Microsoft Docs
description: Compreender o nível de compatibilidade do modelo de dados de tabela.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 231a7ea42560aac3fba0ccf27e27123320f219a3
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="compatibility-level-for-analysis-services-tabular-models"></a>Nível de compatibilidade para modelos em tabela do Analysis Services

*Nível de compatibilidade* refere-se a comportamentos de versão específico no motor do Analysis Services. Normalmente, operação alterações para o nível de compatibilidade com versões principais do SQL Server. Estas alterações também são implementadas no Azure Analysis Services para manter paridade entre ambas as plataformas. Alterações de nível de compatibilidade também afetam as funcionalidades disponíveis nos seus modelos em tabela. Por exemplo, DirectQuery e metadados de objeto em tabela tem implementações diferentes consoante o nível de compatibilidade. 

Serviços de análise do Azure suporta modelos em tabela nos níveis de compatibilidade 1200 e 1400.

O nível de compatibilidade mais recente é 1400. Este nível coincide com o SQL Server 2017 Analysis Services. O nível de compatibilidade 1400 principais funcionalidades incluem:

*  Nova infraestrutura para conectividade de dados e importação para modelos em tabela com suporte para personalizada APIs e TMSL scripting. Esta nova funcionalidade permite suporte para origens de dados adicionais, como o armazenamento de Blobs do Azure.
*  Transformação de dados e as capacidades de aplicação híbrida de dados através de expressões de obter dados e o M.
*  Medidas suportam uma propriedade de linhas de detalhe com uma expressão DAX. Esta propriedade permite que as ferramentas de cliente, como o Microsoft Excel para desagregar para dados detalhados de um relatório agregado. Por exemplo, quando os utilizadores visualizar vendas totais para uma região e o mês, pode ver os detalhes da encomenda associado. 
*  Segurança ao nível do objeto para nomes de tabelas e colunas, além dos dados nelas.
*  Suporte melhorado para hierarquias desalinhadas.
*  Monitorização de desempenho e melhoramentos.
  
## <a name="set-compatibility-level"></a>Nível de compatibilidade de conjunto 
 Quando criar um novo projeto de modelo em tabela no SSDT, pode especificar o nível de compatibilidade no **estruturador de modelos em tabela** caixa de diálogo. 
  
 ![ssas_tabularproject_compat1200](./media/analysis-services-compat-level/aas-tabularproject-compat.png)  
  
 Se selecionar a **não mostrar esta mensagem novamente** opção, todos os projetos subsequentes utilizam o nível de compatibilidade que especificou como predefinição. Pode alterar o nível de compatibilidade predefinido no SSDT no **ferramentas** > **opções**.  
  
 Para atualizar um projeto de modelo em tabela existente no SSDT, defina o **nível de compatibilidade** propriedade no modelo **propriedades** janela. Tenha em atenção, atualizar o nível de compatibilidade é irreversível.
  
## <a name="check-compatibility-level-for-a-tabular-model-database-in-sql-server-management-studio"></a>Verifique o nível de compatibilidade para uma base de dados do modelo em tabela no SQL Server Management Studio 
 No SSMS, clique no nome de base de dados > **propriedades** > **nível de compatibilidade**.  
  
## <a name="check-supported-compatibility-level-for-a-server-in-ssms"></a>Verifique o nível de compatibilidade suportado para um servidor no SSMS  
 No SSMS, faça duplo clique o nome do servidor > **propriedades** > **nível de compatibilidade suportado**.  
  
 Esta propriedade especifica o nível mais elevado de compatibilidade da base de dados que será executado no servidor (excluindo pré-visualização). Não é possível alterar o nível de compatibilidade suportado.  

## <a name="next-steps"></a>Passos Seguintes
  [Criar um modelo no portal do Azure](analysis-services-create-model-portal.md)   
  [Gerir do Analysis Services](analysis-services-manage.md)  
