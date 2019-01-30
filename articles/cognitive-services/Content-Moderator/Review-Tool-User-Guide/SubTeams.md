---
title: Gerenciar equipes e subequipas na API Content Moderator - Content Moderator
titlesuffix: Azure Cognitive Services
description: Saiba como utilizar as equipes e subequipas na API Content Moderator para os serviços cognitivos.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: a36da1787d5abd5f7dc1455823be55f1880c7ba5
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55227136"
---
# <a name="manage-review-teams-and-subteams"></a>Gerenciar equipes de revisão e subequipas

Tem de criar uma equipa antes de utilizar o Content Moderator. Quando inicia a sessão a cópia de segurança e nomeie a sua equipa, esta equipa irá tornar-se sua equipe de predefinição. Só pode ter uma equipe na ferramenta de revisão. No entanto, pode criar vários subequipas. Subequipas são úteis para a criação de equipes de escalonamento ou equipas dedicadas para rever as categorias específicas de conteúdo. Por exemplo, poderá enviar conteúdo para adultos para uma equipe diferente para revisão mais.

Este tópico explica como criar subequipas e atribuir rapidamente revisões em tempo real. No entanto, pode usar [fluxos de trabalho](workflows.md) para atribuir as revisões com base em critérios específicos.

## <a name="go-to-the-teams-setting"></a>Vá para as Equipes de definição

Para começar a utilizar na criação de um subteam, selecione o **Equipes** opção em definições.

![Configurações de equipe](images/0-teams-1.png)

## <a name="create-subteams"></a>Criar subequipas

A equipe de predefinido contém todos os revisores potenciais; subequipas são subconjuntos da Equipe do padrão. Não é possível atribuir alguém para uma subteam se ainda não estiverem membros da Equipe do padrão, por isso terá de adicionar qualquer revisores para a equipe de predefinição agora. Clique no botão de convite na página da equipa.

![Convidar utilizadores](images/invite-users.png)

### <a name="create-a-subteam"></a>Criar um subteam
Role para baixo a página da equipa para a secção de Subteam. Clique no botão Adicionar Subteam. 

![Adicionar Subteam](images/1-teams-1.png)

### <a name="name-your-subteam"></a>Nomeie seu subteam
Introduza o seu nome de subteam na caixa de diálogo e, em seguida, clique em Guardar.

![Nome de subteam](images/1-Teams-2.PNG)

### <a name="assign-members-from-your-default-team"></a>Atribua membros da sua equipa de predefinição.
Clique no botão Adicionar membro para atribuir membros da sua equipa de predefinição a uma ou mais subequipas. Apenas pode adicionar utilizadores existentes para um subteam. Para adicionar novos utilizadores que não estão a ser a ferramenta de revisão, convidá-los através do botão "Convidar" na página de configurações de equipe.

![Atribuir membros subteam](images/1-Teams-3.PNG)

## <a name="assign-reviews-to-your-subteams"></a>Atribuir a revisões para sua subequipas

Assim que tiver sua subequipas criadas e os membros da Equipe atribuídos, pode começar a atribuir a imagem e as revisões de texto para esses subequipas. Isso é feito a partir da janela de revisão.
Se pretender atribuir uma imagem individual para um subteam, clique nas reticências no canto superior direito da imagem, selecione a mudança para o e, selecione um subteam.

![Atribuir a revisão da imagem para subteam](images/3-review-image-subteam-1.png)

## <a name="switch-between-subteams-to-review-assigned-content"></a>Alternar entre subequipas para rever o conteúdo atribuído

Se for um membro de um ou mais subequipas, pode alternar entre esses subequipas partir do dashboard da ferramenta de revisão. Para ver todos atual pendentes revisões que pertencem a um subteam, selecione escolher Subteam do separador de imagem.

![Alternar entre subequipas](images/3-review-image-subteam-2.png)
