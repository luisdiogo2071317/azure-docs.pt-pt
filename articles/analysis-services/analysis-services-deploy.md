---
title: Implementar Azure Analysis Services com o SSDT | Microsoft Docs
description: Saiba como implementar um modelo de tabela num servidor Azure Analysis Services com o SSDT.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 5cbad33bf98e4138b94916a8f87cc7e34901b3da
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34595663"
---
# <a name="deploy-a-model-from-ssdt"></a>Implementar um modelo a partir de SSDT
Depois de criar um servidor na sua subscrição do Azure, está pronto para implementar uma base de dados de modelo de tabela no mesmo. Pode usar as SQL Server Data Tools (SSDT) para criar e implementar um projeto de modelo de tabela em que está a trabalhar. 

## <a name="prerequisites"></a>Pré-requisitos
Para começar, precisa do seguinte:

* **Servidor Analysis Services** no Azure. Para obter mais informações, consulte [Criar um servidor Azure Analysis](analysis-services-create-server.md).
* **Projeto de modelo de tabela** no SSDT ou modelo de tabela existente no nível de compatibilidade 1200 ou posterior. Nunca criou um? Experimente o [tutorial do modelo de tabela de vendas na Internet Adventure Works](https://msdn.microsoft.com/library/hh231691.aspx).
* **Gateway local** - se uma ou mais origens de dados se encontrarem no local da rede da sua organização, é necessário instalar um [gateway de dados no local](analysis-services-gateway.md). O gateway é necessário para que o servidor na nuvem se ligue às suas origens de dados no local para processar e atualizar os dados no modelo.

> [!TIP]
> Antes de implementar, certifique-se de que pode processar os dados nas tabelas. No SSDT, clique em **Modelo** > **Processar** > **Processar tudo**. Se o processamento falhar, a implementação não é efetuada com êxito.
> 
> 

## <a name="get-the-server-name"></a>Obter o nome do servidor

No **portal do Azure** > servidor > **Descrição geral** > **Nome do servidor**, copie o nome do servidor.
   
![Obter o nome do servidor no Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)

## <a name="to-deploy-from-ssdt"></a>Para implementar a partir do SSDT

1. No SSDT > **Explorador de Soluções**, clique com o botão direito do rato no projeto > **Propriedades**. Em seguida, em **Implementação** > **Servidor** cole o nome do servidor.   
   
    ![Colar o nome do servidor na propriedade de implementação do servidor](./media/analysis-services-deploy/aas-deploy-deployment-server-property.png)
2. Em **Explorador de Soluções**, clique com o botão direito do rato em **Propriedades** e, em seguida, clique em **Implementar**. Ser-lhe-á pedido que inicie sessão no Azure.
   
    ![Implementar no servidor](./media/analysis-services-deploy/aas-deploy-deploy.png)
   
    O estado de implementação é exibido na janela de saída e na implementação.
   
    ![Estado da implementação](./media/analysis-services-deploy/aas-deploy-status.png)

E é tudo o que é preciso!


## <a name="troubleshooting"></a>Resolução de problemas
Se a implementação falhar durante a implementação de metadados, é provável que se deva ao facto de não ser possível estabelecer ligação entre o SSDT e o seu servidor. Verifique se é possível estabelecer ligação ao seu servidor com o SSMS. Em seguida, verifique se a propriedade de implementação do servidor é a correta para o projeto.

Se a implementação falhar numa tabela, provavelmente não foi possível estabelecer ligação entre o servidor e a origem de dados. Se a sua origem de dados se encontra na rede da sua organização, certifique-se de que instala um [gateway de dados no local](analysis-services-gateway.md).

## <a name="next-steps"></a>Passos Seguintes
Agora que tem o modelo de tabela implementado no seu servidor, está pronto para se estabelecer ligação com o mesmo. Pode [ligá-lo com o SSMS](analysis-services-manage.md) para o gerir. E pode [ligá-lo através de uma ferramenta de cliente](analysis-services-connect.md) como o Power BI, o Power BI Desktop ou o Excel e começar a criar relatórios.

