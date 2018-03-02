---
title: Exportar um modelo do Azure Resource Manager | Microsoft Docs
description: Utilize o Azure Resource Manager para exportar um modelo a partir de um grupo de recursos existente.
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 5f5ca940-eef8-4125-b6a0-f44ba04ab5ab
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/23/2018
ms.author: tomfitz
ms.openlocfilehash: 7ac553a3608df41548f845e27c545ff63886e37c
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/27/2018
---
# <a name="export-an-azure-resource-manager-template-from-existing-resources"></a>Exportar um modelo do Azure Resource Manager a partir de recursos existentes
Neste artigo, vai aprender a exportar um modelo do Resource Manager a partir de recursos existentes na sua subscrição. Pode utilizar esse modelo gerado para compreender melhor a sintaxe do modelo.

Existem duas formas de exportar um modelo:

* Pode exportar o **modelo que foi utilizado para a implementação**. O modelo exportado inclui todos os parâmetros e variáveis exatamente como apareciam no modelo original. Esta abordagem é útil se tiver implementado recursos através do portal e agora pretende ver o modelo para criar esses recursos. Este modelo pode ser utilizado imediatamente. 
* Pode exportar um **modelo gerado que representa o estado atual do grupo de recursos**. O modelo exportado não é baseado em qualquer modelo utilizado para implementação. Em vez disso, cria um modelo que é um "instantâneo" ou "cópia de segurança" do grupo de recursos. O modelo exportado tem muitos valores codificados e provavelmente não tantos parâmetros como normalmente seriam definidos. Utilize esta opção para voltar a implementar recursos para o mesmo grupo de recursos. Para utilizar este modelo para outro grupo de recursos, poderá ter de significativamente modificá-lo.

Este artigo mostra ambas as abordagens através do portal.

## <a name="deploy-resources"></a>Implementar recursos
Vamos começar por implementar recursos no Azure que pode utilizar para exportar como modelo. Se já tiver um grupo de recursos na sua subscrição que pretende exportar para um modelo, pode ignorar esta secção. O resto deste artigo assume que implementou a aplicação web e a solução de base de dados do SQL Server apresentados nesta secção. Se utilizar uma solução diferente, a sua experiência poderá ser ligeiramente diferente, mas os passos para exportar um modelo são os mesmos. 

1. No [portal do Azure](https://portal.azure.com), selecione **crie um recurso**.
   
      ![Selecionar novo](./media/resource-manager-export-template/new.png)
2. Procure **aplicação Web + SQL** e selecione-a partir das opções disponíveis.
   
      ![Aplicação web de pesquisa e o SQL Server](./media/resource-manager-export-template/webapp-sql.png)

3. Selecione **Criar**.

      ![Selecione criar](./media/resource-manager-export-template/create.png)

4. Forneça os valores necessários para a aplicação Web e a base de dados SQL. Selecione **Criar**.

      ![Forneça o valor de SQL Server e web](./media/resource-manager-export-template/provide-web-values.png)

A implementação pode demorar um minuto. Após a conclusão da implementação, a sua subscrição contém a solução.

## <a name="view-template-from-deployment-history"></a>Ver o modelo no histórico de implementações
1. Vá para o grupo de recursos para o novo grupo de recursos. Tenha em atenção que o portal mostra o resultado da última implementação. Selecione essa ligação.
   
      ![Grupo de recursos](./media/resource-manager-export-template/select-deployment.png)
2. Será apresentado um histórico das implementações do grupo. No seu caso, o portal apresenta provavelmente apenas uma implementação. Selecione essa implementação.
   
     ![Última implementação](./media/resource-manager-export-template/select-history.png)
3. O portal apresenta um resumo da implementação. O resumo inclui o estado da implementação e as respetivas operações, bem como os valores que forneceu para os parâmetros. Para ver o modelo que utilizou para a implementação, selecione **Ver modelo**.
   
     ![Ver resumo da implementação](./media/resource-manager-export-template/view-template.png)
4. O Resource Manager obtém os seguintes sete ficheiros para si:
   
   1. **Modelo** - O modelo que define a infraestrutura para a sua solução. Quando criou a conta do Storage através do portal, o Resource Manager utilizou um modelo para a implementar e guardou esse modelo para consulta futura.
   2. **Parâmetros** - Um ficheiro de parâmetros que pode utilizar para transmitir valores durante a implementação. Contém os valores que indicou durante a primeira implementação. Pode alterar qualquer um destes valores quando reimplementar o modelo.
   3. **CLI** - Um ficheiro de script da interface de linha de comandos do Azure (CLI) que pode utilizar para implementar o modelo.
   3. **CLI 2.0** - Um ficheiro de script da interface de linha de comandos (CLI) do Azure que pode utilizar para implementar o modelo.
   4. **PowerShell** - Um ficheiro de script do Azure PowerShell que pode utilizar para implementar o modelo.
   5. **.NET** - Uma classe .NET que pode utilizar para implementar o modelo.
   6. **.NET** - Uma classe Ruby que pode utilizar para implementar o modelo.
      
      Por predefinição, o portal apresenta o modelo.
      
       ![Ver modelo](./media/resource-manager-export-template/see-template.png)
      
Este é o modelo que foi utilizado para criar a aplicação Web e a base de dados SQL. Repare que contém parâmetros que lhe permitem indicar valores diferentes durante a implementação. Para saber mais sobre a estrutura de um modelo, consulte [Criação de modelos do Azure Resource Manager](resource-group-authoring-templates.md).

## <a name="export-the-template-from-resource-group"></a>Exportar o modelo a partir do grupo de recursos
Se tiver manualmente alterado os recursos ou adicionou recursos em múltiplas implementações, a obtenção de um modelo do histórico de implementação não reflete o estado atual do grupo de recursos. Esta secção mostra-lhe como exportar um modelo que reflita o estado atual do grupo de recursos. Destina-se como um instantâneo do grupo de recursos, pode utilizar para voltar para o mesmo grupo de recursos. Para utilizar o modelo exportado para outras soluções, significativamente tem de o modificar.

> [!NOTE]
> Não é possível exportar um modelo para um grupo de recursos que tem mais de 200 recursos.
> 
> 

1. Para ver o modelo para um grupo de recursos, selecione **Script de automatização**.
   
      ![Exportar grupo de recursos](./media/resource-manager-export-template/select-automation.png)
   
     O Resource Manager avalia os recursos do grupo de recursos e gera um modelo para os mesmos. Nem todos os tipos de recursos suportam a função de modelo de exportação. Poderá ver um erro que indica que existe um problema com a exportação. Pode saber como lidar com esses problemas na secção [Corrigir problemas de exportação](#fix-export-issues).
2. Vai ver novamente os seis ficheiros que pode utilizar para reimplementar a solução. No entanto, desta vez, o modelo é ligeiramente diferente. Tenha em atenção que o modelo gerado contém menos parâmetros do que o modelo da secção anterior. Além disso, repare que neste modelo, em vez de aceitarem valores de parâmetros, muitos dos valores (como a localização e os valores do SKU) estão hard-coded. Antes de reutilizar este modelo, pode ser útil editá-lo para dar melhor uso aos parâmetros. 
   
3. Tem algumas opções para continuar a trabalhar com este modelo. Pode transferir o modelo e trabalhar no mesmo localmente com um editor JSON. Em alternativa, pode guardar o modelo na sua biblioteca e trabalhar no mesmo através do portal.
   
     Se estiver familiarizado com um editor JSON, como [Código VS](https://code.visualstudio.com/) ou [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md), pode preferir transferir o modelo localmente e utilizar esse editor. Para trabalhar localmente, selecione **Transferir**.
   
      ![Transferir modelo](./media/resource-manager-export-template/download-template.png)
   
     Se não estiverem configurado com um editor de JSON, poderá preferir editar o modelo através do portal. O resto deste artigo assume que já guardar o modelo para a sua biblioteca no portal. No entanto, realiza as mesmas alterações de sintaxe no modelo quer trabalhe localmente com um editor JSON quer trabalhe através do portal. Para trabalhar através do portal, selecione **Adicionar à biblioteca**.
   
      ![Adicionar à biblioteca](./media/resource-manager-export-template/add-to-library.png)
   
     Ao adicionar um modelo para a biblioteca, dê o modelo de um nome e descrição. Em seguida, selecione **Guardar**.
   
     ![Conjunto de valores de modelo](./media/resource-manager-export-template/save-library-template.png)
4. Para ver um modelo guardado na biblioteca, selecione **Mais serviços**, escreva **Modelos** para filtrar os resultados e selecione **Modelos**.
   
      ![Encontre modelos](./media/resource-manager-export-template/find-templates.png)
5. Selecione o modelo com o nome que guardou.
   
      ![Selecionar modelo](./media/resource-manager-export-template/select-saved-template.png)

## <a name="customize-the-template"></a>Personalizar o modelo
O modelo exportado funciona bem se quiser criar a mesma aplicação Web e a mesma base de dados SQL para cada implementação. No entanto, o Resource Manager fornece opções para que possa implementar modelos com uma flexibilidade muito maior. Este artigo mostra-lhe como adicionar parâmetros para o nome e palavra-passe de administrador da base de dados. Pode utilizar esta mesma abordagem para acrescentar mais flexibilidade a outros valores do modelo.

1. Para personalizar o modelo, selecione **Editar**.
   
     ![Mostrar modelo](./media/resource-manager-export-template/select-edit.png)
2. Selecione o modelo.
   
     ![Editar modelo](./media/resource-manager-export-template/select-added-template.png)
3. Para poder transmitir os valores que poderá pretender especificar durante a implementação, adicione os dois parâmetro seguintes à secção **parâmetros** do modelo:

   ```json
   "administratorLogin": {
       "type": "String"
   },
   "administratorLoginPassword": {
       "type": "SecureString"
   },
   ```

4. Para utilizar os parâmetros novos, substitua a definição do servidor SQL na secção **Recursos**. Repare que **administratorLogin** e **administratorLoginPassword** utilizam agora os valores dos parâmetros.

   ```json
   {
       "comments": "Generalized from resource: '/subscriptions/{subscription-id}/resourceGroups/exportsite/providers/Microsoft.Sql/servers/tfserverexport'.",
       "type": "Microsoft.Sql/servers",
       "kind": "v12.0",
       "name": "[parameters('servers_tfserverexport_name')]",
       "apiVersion": "2014-04-01-preview",
       "location": "South Central US",
       "scale": null,
       "properties": {
           "administratorLogin": "[parameters('administratorLogin')]",
           "administratorLoginPassword": "[parameters('administratorLoginPassword')]",
           "version": "12.0"
       },
       "dependsOn": []
   },
   ```

6. Selecione **OK** quando terminar de editar o modelo.
7. Selecione **Guardar** para guardar as alterações feitas ao modelo.
   
     ![Guardar o modelo](./media/resource-manager-export-template/save-template.png)
8. Para reimplementar o modelo atualizado, selecione **Implementar**.
   
     ![Implementar o modelo](./media/resource-manager-export-template/redeploy-template.png)
9. Forneça os valores dos parâmetros e selecione um grupo de recursos no qual implementar os recursos.


## <a name="fix-export-issues"></a>Corrigir problemas de exportação
Nem todos os tipos de recursos suportam a função de modelo de exportação. Para resolver este problema, adicione manualmente os recursos em falta no seu modelo. A mensagem de erro inclui os tipos de recursos que não podem ser exportados. Localize o tipo de recurso na [Referência a modelos](/azure/templates/). Por exemplo, para adicionar manualmente um gateway de rede virtual, veja a [referência ao modelo Microsoft.Network/virtualNetworkGateways](/azure/templates/microsoft.network/virtualnetworkgateways).

> [!NOTE]
> Só encontra problemas de exportação se exportar a partir de um grupo de recursos em vez do seu histórico de implementação. Se a última implementação representar com precisão o estado atual do grupo de recursos, deve exportar o modelo do histórico de implementação em vez do grupo de recursos. Só deve exportar de um grupo de recursos depois de efetuar alterações ao grupo de recursos que não esteja definido num único modelo.
> 
> 

## <a name="next-steps"></a>Passos Seguintes

* Pode implementar um modelo através do [PowerShell](resource-group-template-deploy.md), do [CLI do Azure](resource-group-template-deploy-cli.md) ou da [API REST](resource-group-template-deploy-rest.md).
* Para ver como exportar um modelo através do PowerShell, consulte [modelos de exportar o Azure Resource Manager com o PowerShell](resource-manager-export-template-powershell.md).
* Para ver como exportar um modelo através da CLI do Azure, consulte [modelos de exportar o Azure Resource Manager com a CLI do Azure](resource-manager-export-template-cli.md).

