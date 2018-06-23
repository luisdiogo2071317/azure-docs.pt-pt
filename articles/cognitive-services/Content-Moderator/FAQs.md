---
title: Perguntas mais frequentes para Moderator conteúdo do Azure | Microsoft Docs
description: Obtenha respostas às perguntas mais frequentes sobre Moderator conteúdo.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 11/21/2016
ms.author: sajagtap
ms.openlocfilehash: 7bf6c67bd0a83d9455d14253f4f4b2becafd29ba
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351595"
---
# <a name="frequently-asked-questions-faq"></a>Perguntas mais frequentes (FAQ)

#### <a name="what-does-my-content-moderator-subscription-include"></a>O que faz o meu Moderator conteúdo incluir subscrição?
A subscrição de conteúdo Moderator inclui acesso para a ferramenta de revisão e as APIs. Pode decidir se pretende utilizar um ou outro ou ambos, consoante as suas necessidades de negócio.

#### <a name="what-are-the-limitsrestrictions-of-the-content-that-can-be-moderated-by-using-the-api"></a>Quais os limites/restrições do conteúdo que pode ser moderado através da API?
Ao utilizar a API, imagens tem de ter um mínimo de 128 pixels e um tamanho máximo do ficheiro de 4 MB. Texto pode ser, no máximo 1024 carateres. Não há nenhum limite a duração de vídeo.

#### <a name="what-happens-if-the-content-passed-to-the-text-api-or-the-image-api-exceeds-the-size-limits"></a>O que acontece se o conteúdo passado para a API de texto ou para a API de imagem exceder os limites de tamanho?
O texto API devolve um código de erro que informa que o texto é maior do que o permitido. A imagem de API também devolve um código de erro que informa que a imagem não cumpre os requisitos de tamanho.

#### <a name="do-you-keep-the-images-text-or-videos-that-are-submitted-for-moderation"></a>Mantenha as imagens, texto ou vídeos são submetidos para moderação interrupção?
O conteúdo é o seu próprio e não pode ser mantido pela Microsoft sem o seu consentimento. A Microsoft utiliza líder da indústria medidas de segurança para ajudar a proteger os seus conteúdos.

#### <a name="can-i-use-content-moderator-to-screen-for-illegal-child-exploitation-images"></a>Posso utilizar Moderator conteúdo para o ecrã para imagens de exploração de subordinados ilegal?
Não. No entanto, as organizações qualificadas podem utilizar o [PhotoDNA o serviço em nuvem](https://www.microsoft.com/photodna "serviço em nuvem do Microsoft PhotoDNA") ao ecrã para este tipo de conteúdo.

#### <a name="up-to-how-many-review-teams-can-a-user-join-can-the-user-switch-between-teams"></a>Até quantos rever equipas de um utilizador associar? O utilizador pode alternar entre equipas?
Um utilizador pode associar uma equipa cada vez.

#### <a name="what-kind-of-team-member-roles-are-supported-by-the-review-tool-how-are-they-different"></a>Que tipo de membro de equipa funções são suportadas pela ferramenta de revisão? Em que diferem?
O Studio atualmente permite atribuir funções de administrador e revisor. Os administradores podem convidar outros utilizadores e têm acesso para as definições de configuração ao revisores só podem rever os resultados de moderação interrupção e etiqueta ou untag conteúdo.

#### <a name="what-is-a-tag-does-the-review-tool-support-custom-tags"></a>O que é uma etiqueta? A ferramenta de revisão suporta etiquetas personalizadas?
Uma tag é uma ou duas letras código curto que indica um sinalizador de moderação interrupção, tais como 'a' para adultos, 'r' para racy e etc. Os administradores podem definir novas etiquetas para a sua empresa, conforme necessário.

#### <a name="how-many-team-members-can-i-have-in-my-review-team"></a>Os membros da equipa quantos pode ter na minha equipa de revisão
Pode ter um máximo de cinco membros de equipa, incluindo o administrador numa equipa.
