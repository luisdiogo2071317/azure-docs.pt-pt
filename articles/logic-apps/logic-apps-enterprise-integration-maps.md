---
title: Transformar XML com mapas XSLT - Azure Logic Apps | Documentos da Microsoft
description: Adicionar mapas XSLT que transformam o XML no Azure Logic Apps com o Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: 90f5cfc4-46b2-4ef7-8ac4-486bb0e3f289
ms.date: 07/08/2016
ms.openlocfilehash: c5e5e0a0a3f8bd5feedc00d5bbfb76a1453ccc84
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/28/2018
ms.locfileid: "43123561"
---
# <a name="add-maps-for-xml-transformation-in-azure-logic-apps-with-enterprise-integration-pack"></a>Adicionar mapas para a transformação XML no Azure Logic Apps com o Enterprise Integration Pack

Integração da empresa utiliza o mapas para transformar dados XML entre formatos. Um mapa é um documento XML que define os dados num documento que deva ser transformado em outro formato. 

## <a name="why-use-maps"></a>Por que usar o maps?

Suponha que recebe regularmente pedidos de B2B ou notas fiscais de um cliente que utiliza o formato YYYMMDD para datas. No entanto, na sua organização, armazenar datas no formato MMDDYYY. Pode utilizar um mapa para *transformar* o formato de data YYYMMDD em MMDDYYY antes de armazenar os detalhes da ordem ou por fatura na sua base de dados de atividade do cliente.


## <a name="how-do-i-create-a-map"></a>Como posso criar um mapa?

Pode criar projetos de integração BizTalk com o [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Saiba mais sobre o enterprise integration pack") para Visual Studio 2015. Em seguida, pode criar um ficheiro de mapa de integração que permite que mapeie visualmente itens entre dois arquivos de esquema XML. Depois de criar este projeto, terá um documento XSLT.

Se o mapa tiver uma referência a um assembly externo, em seguida, ambos têm de ser carregadas para a conta de integração. Eles devem ser carregados numa ordem específica, primeiro o assembly e, em seguida, o mapa que faça referência ao assembly.


## <a name="how-do-i-add-a-map"></a>Como posso adicionar um mapa?

1. No portal do Azure, selecione **procurar**.

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. Na caixa de filtro de pesquisa, introduza **integração**, em seguida, selecione **contas de integração** na lista de resultados.

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. Selecione a conta de integração em que pretende adicionar o mapa.

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. Selecione o **Maps** mosaico.

    ![](./media/logic-apps-enterprise-integration-maps/map-1.png)

5. Depois dos mapas é aberta a página, escolha **adicionar**.

    ![](./media/logic-apps-enterprise-integration-maps/map-2.png)  

6. Introduza um **nome** para seu mapa. Para carregar o ficheiro de mapa, selecione o ícone de pasta no lado direito do **mapa** caixa de texto. Depois de concluir o processo de carregamento, escolha **OK**.

    ![](./media/logic-apps-enterprise-integration-maps/map-3.png)

7. Depois do Azure adiciona o mapa para sua conta de integração, receberá uma mensagem no ecrã que mostra se o ficheiro de mapa foi adicionado ou não. Depois de receber esta mensagem, escolha o **Maps** mosaico para que pode ver o mapa recentemente adicionado.

    ![](./media/logic-apps-enterprise-integration-maps/map-4.png)


## <a name="how-do-i-add-an-assembly"></a>Como posso adicionar um assembly?
Abra a conta de integração para carregar a assemblagem onde pretende.

1. Escolha o **Assemblies** mosaico.

    ![integrationaccount-assembly-tile](./media/logic-apps-enterprise-integration-maps/assemblytile.png)

2. Depois dos Assemblies é aberta a página, escolha **adicionar**. Introduza um **nome** para seu assembly. Para carregar o ficheiro de assemblagem, escolha o ícone de pasta no lado direito do **Assembly** caixa de texto. Depois de concluir o processo de carregamento, escolha **OK**.

    ![adicionar os Assemblies](./media/logic-apps-enterprise-integration-maps/assemblyfile.png)


## <a name="how-do-i-edit-a-map"></a>Como faço para editar um mapa?

Tem de carregar um novo ficheiro de mapa com as alterações que pretende. Em primeiro lugar, pode baixar o mapa para edição.

Para carregar um novo mapa, que substitui o mapa existente, siga estes passos.

1. Escolha o **Maps** mosaico.

2. Depois de abre a página de mapas, selecione o mapa de que pretende editar.

3. Sobre o **Maps** página, selecione **atualização**.

    ![](./media/logic-apps-enterprise-integration-maps/edit-1.png)

4. No Seletor de ficheiros, selecione o ficheiro de mapa que pretende carregar, em seguida, selecione **aberto**.

    ![](./media/logic-apps-enterprise-integration-maps/edit-2.png)

## <a name="how-to-delete-a-map"></a>Como eliminar um mapa?

1. Escolha o **Maps** mosaico.

2. Depois de abre a página de mapas, selecione o mapa de que pretende eliminar.

3. Escolher **eliminar**.

    ![](./media/logic-apps-enterprise-integration-maps/delete.png)

4. Confirme que pretende eliminar o mapa.

    ![](./media/logic-apps-enterprise-integration-maps/delete-confirmation-1.png)

## <a name="next-steps"></a>Próximos Passos
* [Saiba mais sobre o Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Saiba mais sobre o Enterprise Integration Pack")  
* [Saiba mais sobre os contratos](../logic-apps/logic-apps-enterprise-integration-agreements.md "Saiba mais sobre os contratos de integração do enterprise")  
* [Saiba mais sobre transformações](logic-apps-enterprise-integration-transform.md "Saiba mais sobre a integração empresarial de transformações")  

