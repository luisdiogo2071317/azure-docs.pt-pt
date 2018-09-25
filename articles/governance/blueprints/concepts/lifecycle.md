---
title: Compreender o ciclo de vida de um esquema do Azure
description: Saiba mais sobre o ciclo de vida que atravessa um plano gráfico e os detalhes sobre cada fase.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: e0790168a8b9590aaa440a04cd99f26c2ece2818
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991544"
---
# <a name="understand-the-life-cycle-of-an-azure-blueprint"></a>Compreender o ciclo de vida de um esquema do Azure

Como muitos recursos no Azure, um plano gráfico em esquemas do Azure tem um típico e natural ciclo de vida. Estão criados, implantados e, finalmente eliminados quando já não é necessário ou relevantes.
Suporta operações de ciclo de vida CRUD (criar/ler/atualizar/eliminar) tradicionais de esquemas, mas também se baseia nelas para fornecer níveis adicionais de estado que suportam comuns integração contínua / implementação contínua (CI/CD) pipelines para utilização pelos utilizadores Gerir a sua infraestrutura por meio de código – um elemento-chave em DevOps chamado de infraestrutura como código (IaC).

Para compreender totalmente um plano gráfico e as fases, iremos abranger um ciclo de vida padrão:

> [!div class="checklist"]
> - Criação e edição de um plano gráfico
> - O plano gráfico de publicação
> - Criação e edição de uma nova versão do esquema
> - Publicar uma nova versão do esquema
> - A eliminação de uma versão específica do esquema
> - A eliminar o plano gráfico

## <a name="creating-and-editing-a-blueprint"></a>Criação e edição de um plano gráfico

Ao criar um plano gráfico, adicione artefactos ao mesmo, salvar num grupo de gestão e forneceu um nome exclusivo e uma versão exclusiva. Neste momento, o plano gráfico está numa **rascunho** modo e ainda não é possível atribuir. No entanto, enquanto estiver no **rascunho** modo possa continuar a ser atualizada e foi alterado.

Um plano gráfico no **rascunho** modo que nunca tenha sido publicado irá apresentar um ícone diferente no **definições do esquema** página que aquelas que foram **publicada**. O **versão mais recente** também será apresentado como **rascunho** para estes nunca publicado esquemas.

Criar e editar um plano gráfico com o [portal do Azure](../create-blueprint-portal.md#create-a-blueprint) ou [REST API](../create-blueprint-rest-api.md#create-a-blueprint).

## <a name="publishing-a-blueprint"></a>Publicação de um plano gráfico

Assim que foram feitas todas as alterações pretendidas para um plano gráfico no **rascunho** modo, pode ser **publicada** e disponíveis para atribuição. O **publicado** não é possível alterar a versão do esquema.
Uma vez **publicada**, o plano gráfico é apresentada com um ícone diferente que **rascunho** esquemas e mostra o número de versão fornecida no **versão mais recente** coluna.

Publicar uma esquema com o [portal do Azure](../create-blueprint-portal.md#publish-a-blueprint) ou [REST API](../create-blueprint-rest-api.md#publish-a-blueprint).

## <a name="creating-and-editing-a-new-version-of-the-blueprint"></a>Criação e edição de uma nova versão do esquema

Embora uma **publicado** não é possível alterar a versão de um plano gráfico, uma nova versão do plano gráfico pode ser adicionada ao esquema existente e modificada conforme necessário. Isso é feito ao fazer alterações numa esquema existente. Se o plano gráfico já estava **publicada**, quando estas alterações serão guardadas, mostram como **alterações não publicadas** na lista de definições do esquema. A guardar a guarda as alterações uma **rascunho** versão do esquema.

Editar uma esquema com o [portal do Azure](../create-blueprint-portal.md#edit-a-blueprint).

## <a name="publishing-a-new-version-of-the-blueprint"></a>Publicar uma nova versão do esquema

Tal como foi a primeira versão de um plano gráfico **publicada** atribuí-la, cada versão subseqüente desse mesmo esquema tem de ser **publicada** antes que possa ser atribuído. Quando **alterações não publicadas** foram feitos para um plano gráfico, mas eles ainda não tiverem sido **publicado**, o **publicar esquema** botão está disponível na página de esquema de edição. Se o botão não estiver visível, o plano gráfico já foi **publicada**, mas não tem nenhum **alterações não publicadas**.

> [!NOTE]
> Uma única esquema pode ter vários **publicado** versões que podem cada uma ser atribuídas a subscrições.

Os passos para publicar um plano gráfico com **alterações não publicadas** como uma nova versão de um plano gráfico existente são os mesmos que os passos para publicar um novo plano de gráfico.

## <a name="deleting-a-specific-version-of-the-blueprint"></a>A eliminação de uma versão específica do esquema

Cada versão de um plano gráfico é um objeto exclusivo e podem ser individualmente **publicado**. Isso também significa que cada versão de um plano gráfico pode ser eliminado. A eliminação de uma versão de um plano gráfico não tem qualquer impacto em outras versões desse esquema.

> [!NOTE]
> Não é possível eliminar um plano gráfico com atribuições ativas. Elimine as atribuições de primeiro e, em seguida, elimine a versão que pretende remover.

1. Inicie o serviço de esquemas do Azure no portal do Azure ao clicar no **todos os serviços** e a procurar e selecionando **política** no painel esquerdo. Sobre o **política** página, clique em **esquemas**.

1. Selecione **definição de Blueprint** partir da página à esquerda e utilize o filtro de opções para localizar o plano gráfico que pretende eliminar uma versão do. Clique no mesmo para abrir a página de edição.

1. Clique nas **versões publicadas** separador e localize a versão que pretende eliminar.

1. Com o botão direito na versão a eliminar e selecione **eliminar esta versão**.

## <a name="deleting-the-blueprint"></a>A eliminar o plano gráfico

O esquema de núcleo também pode ser eliminado. Eliminar o esquema de núcleo também irá eliminar quaisquer versões de esquema desse esquema, independentemente de **rascunho** ou **publicada** estado. Como com a eliminação de uma versão de um plano gráfico, a eliminar o esquema de principal não remove as atribuições de existentes de qualquer uma das versões de esquema.

> [!NOTE]
> Não é possível eliminar um plano gráfico com atribuições ativas. Elimine as atribuições de primeiro e, em seguida, elimine a versão que pretende remover.

Eliminar um plano gráfico com o [portal do Azure](../create-blueprint-portal.md#delete-a-blueprint) ou [REST API](../create-blueprint-rest-api.md#delete-a-blueprint).

## <a name="assignments"></a>Atribuições

Existem vários pontos durante o ciclo de vida que pode ser atribuído um plano gráfico para uma subscrição.
Sempre que o modo de uma versão do esquema é **publicado**, em seguida, essa versão pode ser atribuído a uma subscrição. Mesmo quando há um **rascunho** versão do esquema, se houver um ou mais versões de esquema num **publicado** modo, em seguida, cada um deles **publicada** versões é disponível para atribuir. Isto permite que as versões de um plano gráfico para ser utilizado e ativamente atribuído enquanto está a ser desenvolvida uma versão mais recente.

Como são atribuídas as versões de esquemas, é importante compreender onde que lhes foram atribuídas e com os quais os parâmetros tem sido atribuídos com. Os parâmetros podem ser estático ou dinâmico. Para obter mais informações, consulte [parâmetros estáticos e dinâmicos](parameters.md).

### <a name="updating-assignments"></a>A atualizar as atribuições

Quando lhe for atribuído um plano gráfico, a atribuição pode ser atualizada. Há vários motivos para atualizar uma atribuição existente, incluindo:

- Adicionar ou remover [bloqueio do recurso](resource-locking.md)
- Alterar o valor de [parâmetros dinâmicos](parameters.md#dynamic-parameters)
- Atualizar a atribuição para uma mais recente **publicado** versão do esquema

Para saber como, veja [atualizar atribuições existentes](../how-to/update-existing-assignments.md).

## <a name="next-steps"></a>Passos Seguintes

- Compreender como utilizar [parâmetros estáticos e dinâmicos](parameters.md)
- Aprenda a personalizar o [plano gráfico de ordem de sequenciamento](sequencing-order.md)
- Descubra como tornar a utilização de [plano gráfico de bloqueio do recurso](resource-locking.md)
- Saiba como [atualizar atribuições existentes](../how-to/update-existing-assignments.md)
- Resolver problemas durante a atribuição de um plano gráfico com [resolução de problemas gerais](../troubleshoot/general.md)