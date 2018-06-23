---
title: Equipas e subteams na API Moderator conteúdo | Microsoft Docs
description: Saiba como utilizar equipas e subteams na API Moderator conteúdo para os serviços cognitivos.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/22/2017
ms.author: sajagtap
ms.openlocfilehash: 161c7cd8bac07d5ffc138297d98a40317a8d88fc
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351541"
---
# <a name="team-and-subteams"></a>Equipa e Subteams #

Tem de criar um agrupamento antes de utilizar Moderator conteúdo. Quando inicia a sessão de segurança e sua equipa de nome, esta equipa irá tornar-se a equipa de predefinição. Só pode ter um agrupamento na ferramenta de revisão. No entanto, pode criar várias subteams. Subteams são úteis para criar agrupamentos de escalamento ou equipas dedicadas para rever as categorias específicas do conteúdo. Por exemplo, poderá pretender enviar conteúdo para adultos a um agrupamento diferente para revisão mais.

Este tópico explica como criar subteams e atribuir rapidamente revisões no momento. No entanto, pode utilizar [fluxos de trabalho](workflows.md) atribuir revisões com base em critérios específicos.

## <a name="go-to-the-teams-setting"></a>Vá para as equipas de definição ##

Para começar a utilizar na criação de um subteam, selecione o **equipas** opção em definições.

![Definições de equipa](images/0-teams-1.png)

## <a name="create-subteams"></a>Criar subteams ##

O agrupamento predefinido contém todos os revisores de potenciais; subteams são subconjuntos do agrupamento predefinido. Não é possível atribuir alguém para um subteam se não estiverem já membros da equipa de predefinido, por isso terá de adicionar quaisquer revisores à equipa de predefinição agora. Clique no botão de convite na página de equipa.

![Convidar utilizadores](images/invite-users.png)

### <a name="1-create-a-subteam"></a>1. Crie um subteam.
Desloque para baixo a página de agrupamento para a secção de Subteam. Clique no botão Adicionar Subteam. 

![Adicionar Subteam](images/1-teams-1.png)

### <a name="2-name-your-subteam"></a>2. Nome do seu subteam.
Introduza o nome da sua subteam na caixa de diálogo, em seguida, clique em Guardar.

![Nome de subteam](images/1-Teams-2.PNG)

### <a name="3-assign-members-from-your-default-team"></a>3. Atribua membros da sua equipa de predefinição.
Clique no botão Adicionar Membro atribuir membros da sua equipa predefinido para um ou mais subteams. Só é possível adicionar os utilizadores existentes para um subteam. Para adicionar novos utilizadores que não estão na ferramenta de revisão, convidá-los utilizando o botão "Convite" na página Definições de agrupamento.

![Atribuir membros subteam](images/1-Teams-3.PNG)

## <a name="assign-reviews-to-your-subteams"></a>Atribuir revisões à sua subteams ##

Assim que tiver os subteams criados e os membros da equipa atribuídos, pode começar a atribuição de imagem e texto revê para esses subteams. Isto é feito a partir da janela de revisão.
Se pretender atribuir uma imagem individuais para um subteam, clique no botão de reticências no canto superior direito da imagem, selecione para avançar para a e, selecione um subteam.

![Atribuir revisão de imagem para subteam](images/3-review-image-subteam-1.png)

## <a name="switch-between-subteams-to-review-assigned-content"></a>Alternar entre subteams para rever o conteúdo atribuído ##

Se for um membro de um ou mais subteams, pode alternar entre essas subteams a partir do Dashboard de ferramentas de revisão. Para ver todos os atual pendentes revisões que pertençam a um subteam, selecione Subteam escolha o separador imagem.

![Alternar entre subteams](images/3-review-image-subteam-2.png)
