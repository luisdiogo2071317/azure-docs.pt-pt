---
title: Introdução ao Modelos privados | Microsoft Docs
description: Adicionar, gerir e partilhar os seus modelos privados com o Portal do Azure, o Azure CLI ou PowerShell.
services: marketplace-customer
documentationcenter: ''
author: msmbaldwin
manager: asimm
editor: ''
tags: marketplace, azure-resource-manager
keywords: ''
ms.assetid: 6ec20778-b578-4885-acb5-104b0e51ea1a
ms.service: marketplace
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: mbaldwin
ms.openlocfilehash: e3a0bbe75177ac25a0aeff89d171dfe88bd0880f
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="get-started-with-private-templates-on-the-azure-portal"></a>Introdução ao Modelos privados no portal do Azure
Um modelo do [Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) é um modelo declarativo utilizado para definir a implementação. Pode definir os recursos a implementar para uma solução e especificar os parâmetros e as variáveis que permitem introduzir os valores para vários ambientes. O modelo é constituído por JSON e expressões que pode utilizar para construir valores para a implementação.

Pode utilizar a nova capacidade **Modelos** no [portal do Azure](https://portal.azure.com), juntamente com o fornecedor de recursos **Microsoft.Gallery** como uma extensão do [Azure Marketplace](https://azure.microsoft.com/marketplace/), para permitir que os utilizadores criem, giram e implementem modelos privados a partir de uma biblioteca pessoal.

> [!NOTE]
> Em vez de utilizar modelos privados no portal, a Microsoft recomenda a criação de uma aplicação de um catálogo de serviço através de [Aplicações Geridas](../managed-applications/overview.md). Pode disponibilizar a aplicação do catálogo de serviço aos utilizadores na sua organização.

Este documento explica como utilizar o portal do Azure para adicionar, gerir e partilhar um **Modelo** privado.

## <a name="guidance"></a>Orientação
As seguintes sugestões ajudam-no a tirar o máximo partido dos **Modelos** ao trabalhar com as suas soluções:

* Um **Modelo** é um recurso encapsulado que contém um modelo do Resource Manager e metadados adicionais. Este comporta-se de uma forma semelhante a um item no Marketplace. A principal diferença é que é um item privado em vez de um item público do Marketplace.
* A biblioteca de **Modelos** funciona bem para utilizadores que necessitam de personalizar as respetivas implementações.
* Os **Modelos** funcionam bem para utilizadores que necessitam de um repositório simples no Azure.
* Comece com um modelo do Resource Manager existente. Encontre modelos no [github](https://github.com/Azure/azure-quickstart-templates) ou escolha a opção [Exportar modelo](../azure-resource-manager/resource-manager-export-template.md) a partir de um grupo de recursos existente.
* Os **Modelos** estão ligados ao utilizador que os publica. O nome do publicador é visível para todas as pessoas que têm acesso de leitura ao mesmo.
* Os **Modelos** são recursos do Resource Manager e não é possível mudar o nome uma vez publicado.

## <a name="add-a-template-resource"></a>Adicionar um recurso de Modelo
Existem duas formas de criar um recurso de **Modelo** no Portal do Azure.

### <a name="method-1-create-a-new-template-resource-from-a-running-resource-group"></a>Método 1: Criar um novo recurso de Modelo a partir de um grupo de recursos em execução
1. Navegue para um grupo de recursos existente no portal do Azure. Selecione **Exportar modelo** em **Definições**.
2. Assim que o modelo do Resource Manager seja exportado, utilize o botão **Guardar Modelo** para guardá-lo no repositório de **Modelos**. Encontre detalhes completos sobre a Exportação do modelo [aqui](../azure-resource-manager/resource-manager-export-template.md).
   <br /><br />
   ![Exportação do grupo de recursos](media/rg-export-portal1.PNG)
3. Selecione o botão de comando **Guardar para modelo**.
   <br /><br />
4. Introduza as seguintes informações:
   
   * Nome – O nome do objeto de modelo (NOTA: Este campo é um nome baseado no Azure Resource Manager. Todas as restrições de nomenclatura aplicam-se e não pode ser alterada depois de criada).
   * Descrição – Resumo rápido sobre o modelo.
     
     ![Guardar Modelo](media/save-template-portal1.PNG)
5. Clique em **Guardar**.
   
   > [!NOTE]
   > O portal mostra notificações quando o modelo do Resource Manager exportado apresenta erros, mas continuará a poder guardar este modelo do Resource Manager nos Modelos. Certifique-se de que verifica e corrige quaisquer problemas do modelo do Resource Manager antes de voltar a implementar o modelo do Resource Manager exportado.
   > 
   > 

### <a name="method-2-add-a-new-template-resource-from-browse"></a>Método 2: Adicionar um novo recurso de Modelo por pesquisa
Também pode adicionar um novo **Modelo** em branco utilizando o botão de comando +Adicionar em **Procurar > Modelos**. Indique um Nome, Descrição e o modelo do Resource Manager JSON.

![Adicionar Modelo](media/add-template-portal1.PNG)

> [!NOTE]
> O Microsoft.Gallery é um Inquilino baseado no fornecedor de recursos Azure. O recurso de Modelo está associado ao utilizador que o criou. Este não está associado a qualquer subscrição específica. Escolha uma subscrição ao implementar um modelo.
> 
> 

## <a name="view-template-resources"></a>Ver os recursos do Modelo
Todos os **Modelos** disponíveis podem ser vistos em **Procurar > Modelos**. Os modelos disponíveis incluem os que foram criados, bem como aqueles que foram partilhados consigo com diferentes níveis de permissões. Para obter mais informações, veja [controlo de acesso](#access-control-for-a-tenant-resource-provider).

![Ver Modelo](media/view-template-portal1.PNG)

Pode ver os detalhes de um **Modelo** ao clicar num item na lista.

![Ver Modelo](media/view-template-portal2c.png)

## <a name="edit-a-template-resource"></a>Editar um recurso de Modelo
Pode iniciar o fluxo de edição de um **Modelo** ao clicar com o botão direito no item da lista Procurar ou escolhendo o botão de comando Editar.

![Editar Modelo](media/edit-template-portal1a.PNG)

Pode editar a descrição ou o texto do modelo do Resource Manager. Não pode editar o nome, uma vez que é um nome de recurso do Resource Manager. Ao editar o modelo do Resource Manager JSON, iremos validar para garantir de que é JSON válido. Escolha **OK** e, em seguida, **Guardar** para guardar o modelo atualizado.

![Editar Modelo](media/edit-template-portal2a.PNG)

Uma vez que o Modelo é guardado, verá uma notificação de confirmação.

![Editar Modelo](media/edit-template-portal3b.png)

## <a name="deploy-a-template-resource"></a>Implementar um recurso de Modelo
Pode implementar qualquer **Modelo** no qual possua permissão de **Leitura**. Preencha os valores para os parâmetros do modelo do Resource Manager para continuar com a implementação.

![Implementar Modelo](media/deploy-template-portal1b.png)

## <a name="share-a-template-resource"></a>Partilhar um recurso de Modelo
Um recurso de **Modelo** pode ser partilhado com os seus parceiros. A partilha comporta-se de forma semelhante à [atribuição de funções para qualquer recurso no Azure](../active-directory/role-based-access-control-configure.md). O proprietário do **Modelo** concede permissões a outros utilizadores para que possam interagir com um recurso de Modelo. A pessoa ou grupo de pessoas com quem partilha o **Modelo** podem ver o modelo do Resource Manager e as propriedades da sua galeria.

### <a name="access-control-for-the-microsoftgallery-resources"></a>Controlo de acesso para os recursos do Microsoft.Gallery
| Função | Permissões |
| --- | --- |
| Proprietário |Permite controlo total sobre o recurso Modelo, incluindo Partilhar |
| Leitor |Permite a Leitura e Execução (Implementação) no recurso do Modelo |
| Contribuinte |Concede permissão para Editar e Eliminar o recurso de Modelo. O utilizador não pode Partilhar o Modelo com outras pessoas |

Selecione **Partilhar** no item de procura ao clicar com o botão direito do rato ou ao ver um item específico.

![Partilhar Modelo](media/share-template-portal1a.png)

 Agora, pode escolher uma função e um utilizador ou grupo para fornecer acesso a um determinado **Modelo**. As funções disponíveis são Proprietário, Leitor e Contribuinte.

![Partilhar Modelo](media/share-template-portal2b.png)

![Partilhar Modelo](media/share-template-portal3b.png)

Clique em **Selecionar** e **Ok**. Agora, pode ver os utilizadores ou grupos que adicionou ao recurso.

![Partilhar Modelo](media/share-template-portal4b.png)

> [!NOTE]
> Um Modelo pode ser partilhado apenas com utilizadores e grupos no mesmo inquilino do Azure Active Directory. Se partilhar um Modelo com um endereço de e-mail que não se encontra no seu inquilino, é enviado um convite a pedir ao utilizador que se junte ao inquilino como convidado.
> 
> 

## <a name="next-steps"></a>Passos seguintes
* Para saber mais sobre como criar modelo do Resource Manager, consulte [Autorizar modelos](../azure-resource-manager/resource-group-authoring-templates.md).
* Para compreender as funções que pode utilizar num modelo do Resource Manager, veja [Funções de modelo](../azure-resource-manager/resource-group-template-functions.md)

