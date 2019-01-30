---
title: Configurar definições de ferramenta de revisão do Content Moderator - Content Moderator
titlesuffix: Azure Cognitive Services
description: Configurar ou obter a sua equipa, etiquetas, conectores, fluxos de trabalho e as credenciais.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 32563b429764a45efbef952310ab6615567d42f0
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55210935"
---
# <a name="configure-review-tool-settings"></a>Configurar as definições da ferramenta de revisão

Utilizar o separador de definições no dashboard da ferramenta de revisão, é fácil definir e alterar muitos componentes.

![Definições de revisão do moderador de conteúdo](images/settings-1.png)

## <a name="team-and-subteams"></a>Equipa e subequipas

Gerir a sua equipa e subequipas a partir deste separador. Só pode ter uma equipe, mas pode [criar vários subequipas](subteams.md) e enviar convites para membros futuros. Depois de ter enviado convites, pode monitorizá-las, alterar as permissões para os membros da Equipe e convidar utilizadores adicionais. Depois dos membros da Equipe aceitou o convite, pode atribuir esses membros para subequipas diferentes. Pode definir funções dos membros da Equipe para ser administradores ou revisores: os administradores podem convidar outros utilizadores, enquanto os revisores não é possível.

![As configurações de equipe do moderador de conteúdo](images/settings-2-team.png)

## <a name="tags"></a>Etiquetas

Aqui pode [definir etiquetas personalizadas](tags.md) ao introduzir o código curto, o nome e a descrição para as suas etiquetas. Depois de criá-lo, ele está disponível durante as revisões. Pode utilizar diferentes etiquetas para diferentes revisões, ao ativar a visibilidade de logoff.

![Definições de etiquetas do Content Moderator](images/settings-3-tags.png)

## <a name="connectors"></a>Conectores

Fluxos de trabalho adiciona funcionalidades ao utilizar conectores para comunicar com a ferramenta de revisão. A ferramenta de revisão chama as APIs de moderador de conteúdo com o fluxo de trabalho padrão moderadores de conteúdos. Quando se inscreve para a ferramenta de revisão, ele automaticamente-Aprovisiona as credenciais de API do moderador para. Também suporta a integração de outro APIs, de conector, desde que está disponível um conector. Fizemos alguns conectores disponíveis de imediato.

O [separador conectores](connectors.md) é onde pode gerir conectores. Pode adicionar ou eliminar conectores e localizar a chave de subscrição para um conector específico. Clique em ligar para adicioná-las para os fluxos de trabalho personalizados. 

![Definições de conectores do moderador de conteúdo](images/settings-4-connectors.png)

## <a name="workflows"></a>Fluxos de trabalho

Gerir fluxos de trabalho a partir do separador fluxos de trabalho. Pode testar os fluxos de trabalho através do carregamento de um ficheiro de exemplo. Também pode [definir fluxos de trabalho personalizados](workflows.md) para a imagem e texto ao utilizar os conectores de API disponíveis (que se encontra no separador de conectores). 

![Definições de fluxo de trabalho do moderador de conteúdo](images/settings-5-workflows.png)

## <a name="credentials"></a>Credenciais

Este separador fornece acesso rápido para a chave de subscrição do Content Moderator, que terá de utilizar as APIs incluídas com o Content Moderator (moderação de imagens, moderação de texto, a gestão da lista, fluxo de trabalho e APIs de revisão).
 
![O Content Moderator credenciais](images/settings-6-credentials.png)
