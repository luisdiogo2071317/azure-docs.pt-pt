---
title: Nível de compatibilidade no Azure Analysis Services do modelo de dados | Documentos da Microsoft
description: Noções básicas sobre o nível de compatibilidade do modelo de dados tabulares.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 31ca6deef6d81ca7beb08f6df1a15d52ef381a46
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2019
ms.locfileid: "54190396"
---
# <a name="compatibility-level-for-analysis-services-tabular-models"></a>Nível de compatibilidade para modelos em tabela do Analysis Services

*Nível de compatibilidade* refere-se a versão específica comportamentos no motor do Analysis Services. Normalmente, coincidir alterações para o nível de compatibilidade com versões principais do SQL Server. Essas alterações também são implementadas no Azure Analysis Services para manter a paridade entre as duas plataformas. Nível de compatibilidade também altera as funcionalidades de efeito disponíveis nos seus modelos em tabela. Por exemplo, DirectQuery e metadados do objeto em tabela têm implementações diferentes dependendo do nível de compatibilidade. Nível de compatibilidade é especificado no projeto de modelo em tabela no Visual Studio (SSDT). Modelos em tabela criado no e importados a partir do Power BI Desktop são no apenas o nível de compatibilidade 1400.

O Azure Analysis Services suporta modelos em tabela nos níveis de compatibilidade 1200 e 1400. 

> [!NOTE]
> Setembro de 2018 do Power BI Desktop e versões posteriores têm um nível de compatibilidade de 1465. pbix. Este nível de compatibilidade é suportado no Azure Analysis Services. No entanto, o importar um ficheiro do Power BI Desktop não é recomendado para ambientes de produção. Para obter mais informações, consulte [importar um ficheiro do Power BI Desktop](analysis-services-import-pbix.md).

O nível de compatibilidade mais recente é 1400. Este nível coincide com o SQL Server 2017 Analysis Services. Principais recursos do nível de compatibilidade 1400 incluem:

*  Novos recursos para conectividade de dados e importação com suporte para as APIs de JOSÉ e scripts TMSL. 
*  Transformação de dados e recursos de mashup de dados usando expressões de obter dados e M.
*  Medidas suportam uma propriedade de linhas de detalhe com uma expressão DAX. Esta propriedade permite que as ferramentas de cliente, como o Microsoft Excel para desagregar dados detalhados a partir de um relatório de agregados. Por exemplo, quando os usuários exibiam total de vendas para uma região e o mês, pode ver os detalhes do pedido associados. 
*  Segurança de nível de objeto para nomes de tabela e coluna, além dos dados dentro dos mesmos.
*  Suporte avançado para hierarquias desbalanceadas.
*  Monitoramento de desempenho e melhorias.
 
## <a name="set-compatibility-level"></a>Nível de compatibilidade de conjunto

 Ao criar um novo projeto de modelo em tabela no SSDT, pode especificar o nível de compatibilidade no **model designer em tabela** caixa de diálogo. 
  
 ![ssas_tabularproject_compat1200](./media/analysis-services-compat-level/aas-tabularproject-compat.png)  
  
 Se selecionar a **não mostrar esta mensagem novamente** opção, todos os projetos subseqüentes utilizam o nível de compatibilidade que especificou como predefinição. Pode alterar o nível de compatibilidade predefinido no SSDT, no **ferramentas** > **opções**.  
  
 Para atualizar um projeto de modelo em tabela existente no SSDT, defina o **nível de compatibilidade** propriedade no modelo **propriedades** janela. Lembre-se, atualizar o nível de compatibilidade é irreversível.
  
## <a name="check-compatibility-level-for-a-tabular-model-database-in-sql-server-management-studio"></a>Nível de compatibilidade de verificação para uma base de dados de modelo em tabela no SQL Server Management Studio 

 No SSMS, faça duplo clique o nome de base de dados > **propriedades** > **nível de compatibilidade**.  
  
## <a name="check-supported-compatibility-level-for-a-server-in-ssms"></a>Verifique o nível de compatibilidade suportado para um servidor no SSMS  

 No SSMS, faça duplo clique no nome do servidor > **propriedades** > **nível de compatibilidade suportado**.  
  
 Esta propriedade especifica o maior nível de compatibilidade de uma base de dados que vai ser executado no servidor (excluindo a pré-visualização). Não é possível alterar o nível de compatibilidade suportado.  

## <a name="next-steps"></a>Passos Seguintes

  [Criar um modelo no portal do Azure](analysis-services-create-model-portal.md)   
  [Gerir os serviços de análise](analysis-services-manage.md)  
