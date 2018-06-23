---
title: API de reconhecimento de orador | Microsoft Docs
description: Utilize algoritmos avançados para verificação de orador e identificação de orador com a API de reconhecimento de orador nos serviços cognitivos.
services: cognitive-services
author: dwlin
manager: zhang
ms.service: cognitive-services
ms.component: speaker-recognition
ms.topic: article
ms.date: 03/20/2016
ms.author: dwlin
ms.openlocfilehash: 6d5e4e4bbe0cb5e57d2556f680ffcf8d16ee1818
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352166"
---
# <a name="speaker-recognition-api"></a>API de Reconhecimento de Orador

Bem-vindo às APIs de reconhecimento de orador da Microsoft. APIs de reconhecimento de orador são APIs baseado na nuvem que fornecem os algoritmos mais avançados para verificação de orador e identificação de orador. Reconhecimento de orador pode ser dividido em duas categorias: verificação de orador e identificação de orador.


## <a name="speaker-verification"></a>Verificação de Orador


Voz tem características exclusivas que podem ser utilizadas para identificar uma pessoa, tal como uma identificação digital.  Utilização de voz como um sinal para cenários de autenticação e controlo de acesso tem emerged como uma nova ferramenta inovadora – essencialmente de oferta um nível de em segurança que simplifica a experiência de autenticação para os clientes.

APIs de verificação de orador automaticamente possa verificar e autenticar utilizadores que utilizam os respetivos voz ou reconhecimento de voz.

### <a name="enrollment"></a>Inscrição

A inscrição para a verificação de orador é texto dependentes, que significa speakers tem de escolher uma frase de acesso específica a utilizar durante a inscrição e verificação fases. 

No registo, voz de orador é registada indicar uma expressão específica, em seguida, são extraídas de diversas funcionalidades e o frase de acesso que escolheu é reconhecido. Em conjunto, extraídas funcionalidades e a escolhida frase formulário uma assinatura de voz exclusivo.

### <a name="verification"></a>Verificação
###
Na verificação, uma entrada de voz e o frase são comparados com a inscrição assinatura de voz e frase – para verificar se são ou não são da mesma pessoa e se estes estão indicar o frase de acesso correto.

Para obter mais detalhes sobre a verificação de orador, consulte a API [orador - verificação](https://westus.dev.cognitive.microsoft.com/docs/services/563309b6778daf02acc0a508/operations/563309b7778daf06340c9652).

## <a name="speaker-identification"></a>Identificação de Orador

APIs de identificação de orador pode identificar automaticamente a pessoa falando num ficheiro de áudio, atribuído um grupo de speakers potenciais. O entrada de áudio se encontra emparelhado em relação ao grupo fornecido de speakers e, no caso de que existe uma correspondência localizada, é devolvida a identidade de orador.

Speakers todos os devem passar por um processo de inscrição primeiro para obter as respetivas voz registado para o sistema e tem uma impressão de voz criada.


### <a name="enrollment"></a>Inscrição

A inscrição para fins de identificação de orador é texto independentes, que significa que não existem sem restrições no que diz altifalante do áudio. Voz de orador é registada e várias funcionalidades são extraídas para formar uma assinatura de voz exclusivo. 


### <a name="recognition"></a>Reconhecimento

Áudio de altifalante desconhecido, juntamente com o grupo de potencial de speakers, é fornecido durante reconhecimento. A entrada de voz é comparada em relação a todos os speakers para determinar cujo voz é e, se existir uma correspondência encontrada, a identidade do altifalante é devolvida.


Para obter mais detalhes sobre a identificação de orador, consulte a API [orador - identificação](https://westus.dev.cognitive.microsoft.com/docs/services/563309b6778daf02acc0a508/operations/5645c068e597ed22ec38f42e).
