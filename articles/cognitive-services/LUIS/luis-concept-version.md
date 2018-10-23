---
title: Compreender o controlo de versões no LUIS
titleSuffix: Azure Cognitive Services
description: Saiba como utilizar as versões para gerir as alterações na compreensão de idiomas (LUIS)
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 61be39fc870eb73a5836f8e60a3495aff877d7ed
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/22/2018
ms.locfileid: "49637853"
---
# <a name="versions"></a>Versões
Criar modelos diferentes da mesma aplicação com [versões](luis-how-to-manage-versions.md). 

## <a name="version-id"></a>ID da versão
O ID de versão é composto por carateres, dígitos ou "." e não pode ter mais de 10 caracteres.

## <a name="initial-version"></a>Versão inicial
A versão inicial (0,1) é a versão de Active Directory predefinido. 

## <a name="active-version"></a>Versão do Active Directory
Para [definir uma versão](luis-how-to-manage-versions.md#set-active-version) como o Active Directory significa que é atualmente editada e testado no [LUIS](luis-reference-regions.md) Web site. Defina uma versão como o Active Directory para acessar seus dados, a realização de atualizações, bem como para testar e publicá-lo.

O nome da versão atualmente ativa, é apresentado no painel superior, esquerdo após o nome da aplicação. 

[ ![Alterar versão do Active Directory](./media/luis-concept-version/version-in-nav-bar-inline.png) ](./media/luis-concept-version/version-in-nav-bar-expanded.png#lightbox)

## <a name="versions-and-publishing-slots"></a>As versões e ranhuras de publicação
Publicar para os blocos estágio e produto. Cada bloco pode ter uma versão diferente ou a mesma versão. Isso é útil para verificar as alterações entre versões do modelo através do ponto de extremidade, o que está disponível para bots ou outros LUIS chamar as aplicações. 

## <a name="clone-a-version"></a>Clonar uma versão
Clone uma versão para criar uma cópia de uma versão existente e guarde-o como uma nova versão. Clone uma versão para utilizar o mesmo conteúdo da versão existente como ponto de partida para a nova versão. Depois de clonar uma versão, se torna a nova versão da **Active Directory** versão. 

## <a name="import-and-export-a-version"></a>Importar e exportar uma versão
Pode importar uma versão ao nível da aplicação. Essa versão torna-se a versão do Active Directory e usado o ID de versão na propriedade "versionId" do ficheiro de aplicação. Também pode importar o nível de versão para uma aplicação existente. A nova versão torna-se a versão do Active Directory. 

Pode exportar uma versão ao nível da aplicação ou pode exportar uma versão no nível de versão. A única diferença é que a versão de exportado ao nível da aplicação é a versão atualmente ativa e ao nível da versão, pode escolher qualquer versão para exportar no **[configurações](luis-how-to-manage-versions.md)** página. 

O ficheiro exportado não contém informações aprendidas por máquina porque a aplicação é reestruturar os depois de serem importado. O ficheiro exportado contém colaboradores--precisa adicionar estas back assim que a versão é importada para a nova aplicação.

## <a name="export-each-version-as-app-backup"></a>Exportar cada versão como cópia de segurança de aplicação
Para fazer cópias de segurança a sua aplicação LUIS, exportá-cada versão sobre o **[definições](luis-how-to-manage-versions.md)** página.

## <a name="delete-a-version"></a>Eliminar uma versão
Pode eliminar todas as versões, exceto a versão do Active Directory na lista de versões na página de definições. 

## <a name="version-availability-at-the-endpoint"></a>Disponibilidade de versão no ponto final
Não são automaticamente disponíveis na sua aplicação versões treinadas [ponto final](luis-glossary.md#endpoint). Deve [publicar](luis-how-to-publish-app.md) ou voltar a publicar uma versão para que ele esteja disponível no seu ponto final de aplicação. Pode publicar **transição** e **produção**, dando-lhe até duas versões da aplicação disponível no ponto final. Se precisar de mais versões da aplicação disponível num ponto de extremidade, deve exportar a versão e importe novamente para uma nova aplicação. A nova aplicação tem um ID de aplicação diferente.

## <a name="collaborators"></a>Funcionários
O proprietário e todos os [colaboradores](luis-how-to-collaborate.md) têm acesso total a todas as versões da aplicação.

## <a name="next-steps"></a>Passos Seguintes

Consulte how to add [controle de versão](luis-how-to-manage-versions.md) na página de definições da aplicação. 

Aprenda a projetar [intenções](luis-concept-intent.md) para o modelo.