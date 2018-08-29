---
title: Adicionar esquemas para a validação de XML - Azure Logic Apps | Documentos da Microsoft
description: Crie esquemas que validam os documentos XML no Azure Logic Apps com o Enterprise Integration Pack
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: 56c5846c-5d8c-4ad4-9652-60b07aa8fc3b
ms.date: 07/29/2016
ms.openlocfilehash: e03346da1c2b77f885c39d5329f990684979c56e
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/28/2018
ms.locfileid: "43123078"
---
# <a name="validate-xml-with-schemas-in-azure-logic-apps-with-enterprise-integration-pack"></a>Validar XML com esquemas no Azure Logic Apps com o Enterprise Integration Pack

Esquemas de confirmar que os documentos XML que receber são válidos e tem os dados esperados num formato predefinido. Esquemas também ajudar a validar as mensagens que são trocadas num cenário B2B.

## <a name="add-a-schema"></a>Adicionar um esquema

1. No portal do Azure, selecione **todos os serviços**.

    ![Portal do Azure, "Todos os serviços"](media/logic-apps-enterprise-integration-schemas/overview-11.png)

2. Na caixa de filtro de pesquisa, introduza **integração**e selecione **contas de integração** na lista de resultados.

    ![Caixa de pesquisa de filtro](media/logic-apps-enterprise-integration-schemas/overview-21.png)

3. Selecione o **conta de integração** onde pretende adicionar o esquema.

    ![Lista de contas de integração](media/logic-apps-enterprise-integration-schemas/overview-31.png)

4. Escolha o **esquemas** mosaico.

    ![Conta de integração de exemplo, "Esquemas"](media/logic-apps-enterprise-integration-schemas/schema-11.png)

### <a name="add-a-schema-file-smaller-than-2-mb"></a>Adicione um ficheiro de esquema menor do que 2 MB

1. Na **esquemas** painel abrir (a partir de passos anteriores), selecione **Add**.

    ![Painel de esquemas, "Adicionar"](media/logic-apps-enterprise-integration-schemas/schema-21.png)

2. Introduza um nome para o seu esquema. Carregue o ficheiro de esquema ao selecionar o ícone de pasta junto a **esquema** caixa. Depois do processo de carregamento for concluído, selecione **OK**.

    ![Captura de ecrã de "Adicionar Schema", com "Ficheiro pequeno" realçado](media/logic-apps-enterprise-integration-schemas/schema-31.png)

### <a name="add-a-schema-file-larger-than-2-mb-up-to-8-mb-maximum"></a>Adicione um ficheiro de esquema mais de 2 MB (até 8 MB máximo)

Estes passos são diferentes com base no nível de acesso do contentor do blob: **pública** ou **sem acesso anónimo**.

**Para determinar este nível de acesso**

1.  Open **Explorador de armazenamento do Azure**. 

2.  Sob **contentores de BLOBs**, selecione o contentor de BLOBs que quiser. 

3.  Selecione **Security**, **nível de acesso**.

Se for o nível de acesso de segurança de blob **público**, siga estes passos.

![Explorador de armazenamento do Azure, com "Contentores de BLOBs", "Segurança" e "Public" realçado](media/logic-apps-enterprise-integration-schemas/blob-public.png)

1. Carregar o esquema para a conta de armazenamento e copie o URI.

    ![Conta de armazenamento, com o URI realçado](media/logic-apps-enterprise-integration-schemas/schema-blob.png)

2. Na **Adicionar esquema**, selecione **ficheiros grandes**e forneça o URI no **URI de conteúdo** caixa de texto.

    ![Esquemas, com o botão "Adicionar" e "Ficheiros grandes" realçado](media/logic-apps-enterprise-integration-schemas/schema-largefile.png)

Se for o nível de acesso de segurança de blob **sem acesso anónimo**, siga estes passos.

![Explorador de armazenamento do Azure, com "Contentores de BLOBs", "Segurança" e "Sem acesso anónimo" realçado](media/logic-apps-enterprise-integration-schemas/blob-1.png)

1. Carregar o esquema para a conta de armazenamento.

    ![Conta de armazenamento](media/logic-apps-enterprise-integration-schemas/blob-3.png)

2. Gere uma assinatura de acesso partilhado para o esquema.

    ![Conta de armazenamento, com o separador de assinaturas de acesso partilhado realçado](media/logic-apps-enterprise-integration-schemas/blob-2.png)

3. Na **Adicionar esquema**, selecione **ficheiros grandes**e forneça o URI de assinatura de acesso partilhado no **URI de conteúdo** caixa de texto.

    ![Esquemas, com o botão "Adicionar" e "Ficheiros grandes" realçado](media/logic-apps-enterprise-integration-schemas/schema-largefile.png)

4. Na **esquemas** painel da sua conta de integração, o seu esquema recém-adicionado deve aparecer.

    ![Sua conta de integração, com "Esquemas" e o novo esquema realçado](media/logic-apps-enterprise-integration-schemas/schema-41.png)

## <a name="edit-schemas"></a>Editar esquemas

1. Escolha o **esquemas** mosaico.

2. Depois do **esquemas** é apresentado o painel, selecione o esquema que pretende editar.

3. Sobre o **esquemas** painel, escolha **editar**.

    ![Painel de esquemas](media/logic-apps-enterprise-integration-schemas/edit-12.png)

4. Selecione o ficheiro de esquema que pretende editar, em seguida, selecione **aberto**.

    ![Ficheiro de esquema aberta para editar](media/logic-apps-enterprise-integration-schemas/edit-31.png)

O Azure mostra uma mensagem que o esquema carregado com êxito.

## <a name="delete-schemas"></a>Eliminar esquemas

1. Escolha o **esquemas** mosaico.

2. Depois do **esquemas** é apresentado o painel, selecione o esquema que pretende eliminar.

3. Sobre o **esquemas** painel, escolha **eliminar**.

    ![Painel de esquemas](media/logic-apps-enterprise-integration-schemas/delete-12.png)

4. Para confirmar que pretende eliminar o esquema selecionado, escolha **Sim**.

    ![Mensagem de confirmação "Excluir o esquema"](media/logic-apps-enterprise-integration-schemas/delete-21.png)

    Na **esquemas** painel, a lista de esquema é atualizada e já não inclui o esquema que eliminou.

    ![A integração de conta, com "Esquemas" realçado](media/logic-apps-enterprise-integration-schemas/delete-31.png)

## <a name="next-steps"></a>Passos Seguintes
* [Saiba mais sobre o Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Saiba mais sobre o enterprise integration pack").  

