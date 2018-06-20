---
title: Compreender LUIS - Azure de controlo de versões | Microsoft Docs
description: Saiba como utilizar versões para gerir as alterações na compreensão de idiomas (LUIS)
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/13/2018
ms.author: v-geberr
ms.openlocfilehash: dabe7def2766770b686be3c43d4af4f331dd9577
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266078"
---
# <a name="versions"></a>Versões
Criar modelos diferentes da mesma aplicação com [versões](luis-how-to-manage-versions.md). 

## <a name="version-id"></a>ID da versão
O ID de versão é composto por carateres, dígitos ou '.' e não pode ter mais de 10 carateres de comprimento.

## <a name="initial-version"></a>Versão inicial
A versão inicial (0,1) é a versão de Active Directory predefinida. 

## <a name="active-version"></a>Versão do Active Directory
Para [definir uma versão](luis-how-to-manage-versions.md#set-active-version) como significa que o Active Directory está atualmente editado e testado no [LUIS] [ LUIS] Web site. Defina uma versão como o Active Directory para aceder aos respetivos dados, efetuar atualizações, bem como testar e publicá-lo.

O nome da versão atualmente ativo é apresentado no painel esquerdo, superior após o nome da aplicação. 

[ ![Alterar versão do Active Directory](./media/luis-concept-version/version-in-nav-bar-inline.png) ](./media/luis-concept-version/version-in-nav-bar-expanded.png#lightbox)

## <a name="versions-and-publishing-slots"></a>As versões e de ranhuras de publicação
Publicar os blocos fase e produto. Cada bloco pode ter uma versão diferente ou a mesma versão. Isto é útil para verificar as alterações entre versões do modelo através do ponto final, que está disponível para bots ou outros LUIS chamar aplicações. 

## <a name="clone-a-version"></a>Clonar uma versão
Clone de uma versão para criar uma cópia de uma versão existente e guarde-o como uma nova versão. Clone de uma versão a utilizar os mesmos conteúdos da versão existente como ponto de partida para a nova versão. Depois de clonar uma versão, a nova versão torna-se a **Active Directory** versão. 

## <a name="import-and-export-a-version"></a>Importar e exportar uma versão
Pode importar uma versão ao nível da aplicação. Essa versão torna-se a versão do Active Directory e utilizado o ID de versão na propriedade "versionId" do ficheiro app. Também pode importar a nível de versão para uma aplicação existente. A nova versão torna-se a versão do Active Directory. 

Pode exportar uma versão ao nível da aplicação ou pode exportar uma versão de nível de versão. A única diferença é que a versão exportada ao nível da aplicação é a versão atualmente ativa ao nível de versão, pode escolher qualquer versão para exportar no **[definições](luis-how-to-manage-versions.md)** página. 

O ficheiro exportado não contêm informações adquiridos por máquina porque a aplicação é retrained após ser importada. O ficheiro exportado contém colaboradores – tem de adicionar estas back assim que a versão é importada para a nova aplicação.

## <a name="export-each-version-as-app-backup"></a>Exportar cada versão como cópia de segurança da aplicação
Para fazer cópias de segurança a sua aplicação LUIS, exporte cada versão no **[definições](luis-how-to-manage-versions.md)** página.

## <a name="delete-a-version"></a>Eliminar uma versão
Pode eliminar todas as versões, exceto o Active Directory versão na lista de versões na página de definições. 

## <a name="version-availability-at-the-endpoint"></a>Disponibilidade de versão do ponto final
Não estão automaticamente disponíveis na sua aplicação versões treinadas [endpoint](luis-glossary.md#endpoint). Tem [publicar](PublishApp.md) ou voltar a publicar uma versão para que este esteja disponível no seu ponto final de aplicação. Pode publicar para **transição** e **produção**, dando-lhe até duas versões da aplicação disponível no ponto final. Se precisar de mais versões da aplicação disponível um ponto final, deve exportar a versão e reimportar para uma nova aplicação. A nova aplicação tem um ID de aplicação diferente.

## <a name="collaborators"></a>Funcionários
O proprietário e todos os [colaboradores](luis-how-to-collaborate.md) têm acesso total a todas as versões da aplicação.

## <a name="next-steps"></a>Passos Seguintes

Consulte how to add [versioning](luis-how-to-manage-versions.md) na página de definições de aplicação. 

Saber como estruturar [pendentes](luis-concept-intent.md) para o modelo.

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions