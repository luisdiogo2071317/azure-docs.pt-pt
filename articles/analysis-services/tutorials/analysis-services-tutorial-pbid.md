---
title: Tutorial - Ligar ao Azure Analysis Services com o Power BI Desktop | Microsoft Docs
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: tutorial
ms.date: 07/03/2018
ms.author: owend
ms.reviewer: owend
ms.openlocfilehash: 4096b9f77ba841abfcb4f29f2827aefacf22103f
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37446593"
---
# <a name="tutorial-connect-with-power-bi-desktop"></a>Tutorial: Ligar com o Power BI Desktop

Neste tutorial, utiliza o Power BI Desktop para ligar à base de dados do modelo de exemplo adventureworks no seu servidor. As tarefas que concluiu simulam uma ligação de utilizador típica ao modelo e criação de um relatório básico a partir de dados do modelo.

> [!div class="checklist"]
> * Obter o nome do seu servidor a partir do portal
> * Ligar com o Power BI Desktop
> * Criar um relatório básico

## <a name="prerequisites"></a>Pré-requisitos

- [Adicionar a base dados do modelo de exemplo adventureworks](../analysis-services-create-sample-model.md) ao seu servidor.
- Tenha [*permissões*](../analysis-services-server-admins.md) de leitura para a base de dados do modelo de exemplo adventureworks.
- [Instale o Power BI Desktop mais recente](https://powerbi.microsoft.com/desktop).

## <a name="log-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure
Neste tutorial, inicia sessão no portal para obter apenas o nome do servidor. Normalmente, os utilizadores iriam obter o nome do servidor a partir do administrador do servidor.

Inicie sessão no [portal](https://portal.azure.com/).

## <a name="get-server-name"></a>Obter nome do servidor
Para ligar ao seu servidor do Power BI Desktop, primeiro precisa do nome do servidor. Pode obter o nome do servidor a partir do portal.

No **portal do Azure** > servidor > **Descrição geral** > **Nome do servidor**, copie o nome do servidor.
   
   ![Obter o nome do servidor no Azure](./media/analysis-services-tutorial-pbid/aas-copy-server-name.png)

## <a name="connect-in-power-bi-desktop"></a>Ligar no Power BI Desktop

1. No Power BI Desktop, clique em **Obter Dados** > **Azure** > **Base de dados do Azure Analysis Services**.

   ![Ligar em Obter dados](./media/analysis-services-tutorial-pbid/aas-pbid-connect-aasserver.png)

2. No **Servidor**, cole o nome do servidor e, em seguida, na **Base de dados**, introduza **adventureworks** e clique em **OK**.

   ![Especificar a base de dados e o modelo do servername](./media/analysis-services-tutorial-pbid/aas-pbid-connect-aas-servername.png)

3. Quando lhe for pedido, introduza as suas credenciais de início de sessão. A conta que introduzir tem de ter, pelo menos, permissões de leitura para a base de dados do modelo de exemplo adventureworks.

    O modelo adventureworks abre-se no Power BI Desktop com um relatório em branco na vista de Relatório. A lista **Campos** apresenta todos os objetos de modelo não oculto. O estado de ligação é apresentado no canto inferior direito.

4. Em **VISUALIZAÇÕES**, selecione **Gráfico de Barras Agrupadas** e, em seguida, clique em **Formato** (ícone de rolo de pintura) e ative as **Etiquetas de dados**. 

   ![Visualizações](./media/analysis-services-tutorial-pbid/aas-pbid-visualizations-report.png)

5. Na tabela **CAMPOS** > **Vendas da Internet**, selecione as medidas **Total de Vendas da Internet** e **Margem**. Na tabela **Categoria do Produto**, selecione **Nome da Categoria de Produto**.

   ![Concluir relatório](./media/analysis-services-tutorial-pbid/aas-pbid-complete-report.png)

    Dispense alguns minutos para explorar o modelo de exemplo adventureworks ao criar visualizações diferentes e repartir em dados e métricas. Quando estiver satisfeito com o relatório, não se esqueça de o guardar.

## <a name="clean-up-resources"></a>Limpar recursos

Se já não for preciso, não guarde o relatório ou elimine o ficheiro se o tiver guardado.

## <a name="next-steps"></a>Passos seguintes
Neste tutorial, aprendeu a utilizar o Power BI Desktop para ligar a um modelo de dados num servidor e criar um relatório básico. Se não estiver familiarizado sobre como criar um modelo de dados, veja o [tutorial de modelação de dados tabular de Vendas da Internet do Adventure Works](aas-adventure-works-tutorial.md).