---
title: Perguntas mais frequentes para a API de visão do computador | Microsoft Docs
description: Obtenha respostas às perguntas mais frequentes sobre a API de visão de computador nos serviços cognitivos da Microsoft.
services: cognitive-services
author: KellyDF
manager: corncar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 01/26/2017
ms.author: kefre
ms.openlocfilehash: 5c862dd2fb26a005f4e785673a4e9358ecf9286f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351542"
---
# <a name="computer-vision-api-frequently-asked-questions"></a>Computador visão API perguntas mais frequentes
### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-computer-vision-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Se não conseguir encontrar respostas às suas perguntas neste FAQ, tente pedir a Comunidade de API de visão do computador no [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) ou contacte [ajuda e suporte no UserVoice](https://cognitive.uservoice.com/)

-----

**Pergunta**: *posso preparar computador visão API a utilização de etiquetas personalizadas?  Por exemplo, gostaria de feed de imagens de breeds cat para 'Preparar' o AI, em seguida, receber o valor de breed um pedido de AI.*

**Resposta**: esta função não está atualmente disponível. No entanto, a nossa engenheiros estiver a trabalhar para ativar esta funcionalidade para problemas de visão do computador.

-----

**Pergunta**: *visão de computador pode ser utilizado localmente sem uma ligação à internet?*

**Resposta**: atualmente não oferecemos no local ou solução local.

-----

**Pergunta**: *que idiomas são suportados com problemas de visão do computador?*

**Resposta**: os idiomas suportados incluem:

| | | Idiomas Suportados | | |
|---------------- |------------------ |------------------ |--------------------------- |--------------------
| Dinamarquês (da DK)  | Neerlandês (nl-NL)     | Português           | Finlandês (fi-FI)            |Francês (fr-FR)
| Alemão (Alemanha)  | Grego (el GR)     | Húngaro (hu-HU) | Italiano (it-IT)            | Japonês (ja-JP)
| Coreano (ko-KR)  | Norueguês (não-nb) | Polaco (pl-PL)    | Português (pt-BR) (pt-PT) | Russo (ru-RU)
| Espanhol (es-ES)   | Sueco (sv-SV)     | Turco (tr-TU)   |                            |

-----

**Pergunta**: *visão de computador pode ser utilizado para ler plates de licença?*

**Resposta**: A API de visão oferece boa deteção texto com OCR, mas não está atualmente otimizado para plates de licença. Estamos constantemente a tentar melhorar os nossos serviços e adicionou OCR para reconhecimento de licença plate automática a nossa lista de pedidos de funcionalidades.

-----

**Pergunta:** *que idiomas são suportados para reconhecimento de escrita manual?*

**Resposta**: atualmente, apenas inglês é suportado.

-----

**Pergunta**: *que tipos de escrita analisa são suportados para reconhecimento de escrita manual?*

**Resposta**: A tecnologia funciona com diferentes tipos de analisa, incluindo whiteboards, documento técnico e notas temporária amarela.

-----

**Pergunta**: *quanto does a operação de reconhecimento de escrita necessário?*

**Resposta**: A quantidade de tempo que demora depende o comprimento do texto. Para mais textos, pode demorar até a vários segundos. Por conseguinte, depois de concluída a operação de texto de Handwritten reconhecer, poderá ter de aguardar antes de pode obter os resultados utilizando a operação de obter o resultado de operação de texto Handwritten.

-----

**Pergunta**: *como funciona o texto de identificador de tecnologia de reconhecimento de escrita que foi inserido um acento circunflexo meio de uma linha a utilizar?*

**Resposta**: essa texto é devolvido como uma linha separada pela operação de reconhecimento de escrita.

-----

**Pergunta**: *como a tecnologia de reconhecimento de escrita processa palavras Escalamento ultrapassados ou linhas?*

**Resposta**: se as palavras são ultrapassadas com várias linhas para compor-los irreconhecível, a operação de reconhecimento de escrita não processará-los. No entanto, se as palavras são ultrapassadas através de uma única linha, esse cruzamento é tratado como ruído e as palavras ainda obterem captadas pela operação de reconhecimento de escrita.

-----

**Pergunta**: *que orientations de texto são suportados para a tecnologia de reconhecimento de escrita?*

**Resposta**: texto orientado para nos ângulos de até cerca de 30 graus graus 40 poderá obter captado pela operação de reconhecimento de escrita.

-----
