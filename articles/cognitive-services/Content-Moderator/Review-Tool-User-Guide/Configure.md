---
title: Configurar as definições de ferramenta de revisão conteúdo Moderator | Microsoft Docs
description: Configurar ou obtenha a sua equipa, etiquetas, conectores, fluxos de trabalho e as credenciais.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/25/2017
ms.author: sajagtap
ms.openlocfilehash: a3432a1d8f424fbe78570f47b774c6e7942e16b1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351506"
---
# <a name="about-review-tool-settings"></a>Sobre as definições da ferramenta de revisão #

Utilizar o separador de definições no Dashboard da ferramenta de revisão, é fácil definir e alterar diversos componentes.

![Definições de revisão Moderator de conteúdo](images/settings-1.png)

## <a name="team-and-subteams"></a>Equipa e Subteams ## 

Gerir a sua equipa e subteams a partir deste separador. Só pode ter um agrupamento, mas pode [criar vários subteams](subteams.md) e enviar convites aos membros futuros. Depois de ter enviado invites, pode monitorizá-las, alterar as permissões para os membros da equipa e convidar utilizadores adicionais. Depois dos membros da equipa tem aceite o convite, pode atribuir os membros a subteams diferentes. Pode definir funções os membros do agrupamento ser administradores ou revisores: os administradores podem convidar outros utilizadores, enquanto os revisores não é possível.

![Definições de equipa Moderator de conteúdo](images/settings-2-team.png)

## <a name="tags"></a>Etiquetas ##

Este é onde pode [etiquetas personalizadas de definir](tags.md) ao introduzir o código de curto, um nome e uma descrição para as suas etiquetas. Depois de ter criado-lo, está disponível durante revisões. Pode utilizar diferentes etiquetas para revisões diferentes, ao ativar a visibilidade terminar a sessão e sucessivamente.

![Definições de conteúdo Moderator etiquetas](images/settings-3-tags.png)

## <a name="connectors"></a>Conectores ##

Fluxos de trabalho adiciona funcionalidades ao utilizar conectores para comunicar com a ferramenta de revisão. A ferramenta de revisão chamadas das APIs do Moderator conteúdo com o fluxo de trabalho predefinido para moderating conteúdo. Quando se inscreve para a ferramenta de revisão,-auto-Aprovisiona as credenciais de Moderator API por si. Também suporta a integração de outro conector de APIs, desde que um conector está disponível. Efetuamos alguns conetores disponíveis a box.

O [separador conectores](connectors.md) é onde irá gerir os conectores. Pode adicionar ou eliminar os conectores e localizar a chave de subscrição de um conector específico. Clicar em ligar para adicioná-las para os fluxos de trabalho personalizados. 

![Definições de conectores Moderator de conteúdo](images/settings-4-connectors.png)

## <a name="workflows"></a>Fluxos de trabalho ##

Gerir fluxos de trabalho a partir do separador fluxos de trabalho. Pode testar os fluxos de trabalho através do carregamento de um ficheiro de exemplo. Também pode [definir fluxos de trabalho personalizados](workflows.md) para a imagem e texto ao utilizar os conectores de API disponíveis (que se encontra no separador de conectores). 

![Definições de fluxo de trabalho Moderator de conteúdo](images/settings-5-workflows.png)

## <a name="credentials"></a>Credenciais ##

Este separador permite aceder rapidamente a sua chave de subscrição Moderator conteúdo, que terá de utilizar as APIs incluídas com conteúdo Moderator (moderação de interrupção de imagem, moderação de interrupção de texto, a gestão da lista, fluxo de trabalho e APIs de revisão).
 
![Credenciais Moderator conteúdo](images/settings-6-credentials.png)
