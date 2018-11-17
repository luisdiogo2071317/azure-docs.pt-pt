---
title: As equipes e subequipas na API Content Moderator | Documentos da Microsoft
description: Saiba como utilizar as equipes e subequipas na API Content Moderator para os serviços cognitivos.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/22/2017
ms.author: sajagtap
ms.openlocfilehash: 6e1fc08af1062ae8962ba33c6df980182175264b
ms.sourcegitcommit: 7804131dbe9599f7f7afa59cacc2babd19e1e4b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/17/2018
ms.locfileid: "51852151"
---
# <a name="team-and-subteams"></a>Equipa e subequipas #

Tem de criar uma equipa antes de utilizar o Content Moderator. Quando inicia a sessão a cópia de segurança e nomeie a sua equipa, esta equipa irá tornar-se sua equipe de predefinição. Só pode ter uma equipe na ferramenta de revisão. No entanto, pode criar vários subequipas. Subequipas são úteis para a criação de equipes de escalonamento ou equipas dedicadas para rever as categorias específicas de conteúdo. Por exemplo, poderá enviar conteúdo para adultos para uma equipe diferente para revisão mais.

Este tópico explica como criar subequipas e atribuir rapidamente revisões em tempo real. No entanto, pode usar [fluxos de trabalho](workflows.md) para atribuir as revisões com base em critérios específicos.

## <a name="go-to-the-teams-setting"></a>Vá para as Equipes de definição ##

Para começar a utilizar na criação de um subteam, selecione o **Equipes** opção em definições.

![Configurações de equipe](images/0-teams-1.png)

## <a name="create-subteams"></a>Criar subequipas ##

A equipe de predefinido contém todos os revisores potenciais; subequipas são subconjuntos da Equipe do padrão. Não é possível atribuir alguém para uma subteam se ainda não estiverem membros da Equipe do padrão, por isso terá de adicionar qualquer revisores para a equipe de predefinição agora. Clique no botão de convite na página da equipa.

![Convidar utilizadores](images/invite-users.png)

### <a name="1-create-a-subteam"></a>1. Crie um subteam.
Role para baixo a página da equipa para a secção de Subteam. Clique no botão Adicionar Subteam. 

![Adicionar Subteam](images/1-teams-1.png)

### <a name="2-name-your-subteam"></a>2. Nome do seu subteam.
Introduza o seu nome de subteam na caixa de diálogo e, em seguida, clique em Guardar.

![Nome de subteam](images/1-Teams-2.PNG)

### <a name="3-assign-members-from-your-default-team"></a>3. Atribua membros da sua equipa de predefinição.
Clique no botão Adicionar membro para atribuir membros da sua equipa de predefinição a uma ou mais subequipas. Apenas pode adicionar utilizadores existentes para um subteam. Para adicionar novos utilizadores que não estão a ser a ferramenta de revisão, convidá-los através do botão "Convidar" na página de configurações de equipe.

![Atribuir membros subteam](images/1-Teams-3.PNG)

## <a name="assign-reviews-to-your-subteams"></a>Atribuir a revisões para sua subequipas ##

Assim que tiver sua subequipas criadas e os membros da Equipe atribuídos, pode começar a atribuir a imagem e as revisões de texto para esses subequipas. Isso é feito a partir da janela de revisão.
Se pretender atribuir uma imagem individual para um subteam, clique nas reticências no canto superior direito da imagem, selecione a mudança para o e, selecione um subteam.

![Atribuir a revisão da imagem para subteam](images/3-review-image-subteam-1.png)

## <a name="switch-between-subteams-to-review-assigned-content"></a>Alternar entre subequipas para rever o conteúdo atribuído ##

Se for um membro de um ou mais subequipas, pode alternar entre esses subequipas partir do dashboard da ferramenta de revisão. Para ver todos atual pendentes revisões que pertencem a um subteam, selecione escolher Subteam do separador de imagem.

![Alternar entre subequipas](images/3-review-image-subteam-2.png)
