---
title: FAQs sobre a API de imagem digitalizada
titlesuffix: Azure Cognitive Services
description: Obtenha respostas às perguntas mais frequentes sobre a API de imagem digitalizada nos serviços cognitivos do Azure.
services: cognitive-services
author: KellyDF
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 01/26/2017
ms.author: kefre
ms.openlocfilehash: 5c0a4e981a290b9a758c8401a75e546c61618b45
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "45983906"
---
# <a name="computer-vision-api-frequently-asked-questions"></a>API de imagem digitalizada perguntas mais frequentes

### <a name="if-you-cant-find-answers-to-your-questions-in-this-faq-try-asking-the-computer-vision-api-community-on-stackoverflowhttpsstackoverflowcomquestionstaggedproject-oxfordormicrosoft-cognitive-or-contact-help-and-support-on-uservoicehttpscognitiveuservoicecom"></a>Se não conseguir encontrar respostas para suas perguntas neste FAQ, experimente perguntar à Comunidade de API de imagem digitalizada no [Stack Overflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) ou contacte [ajuda e suporte no UserVoice](https://cognitive.uservoice.com/)

-----

**Pergunta**: *posso treinar API de imagem digitalizada para utilizar etiquetas personalizadas?  Por exemplo, eu gostaria de feed em fotografias de breeds gato "Preparar" a IA, em seguida, receber o valor de linhagem numa solicitação de IA.*

**Resposta**: esta função não está atualmente disponível. No entanto, os nossos engenheiros a trabalhar para colocar essa funcionalidade para imagem digitalizada.

-----

**Pergunta**: *visão do computador podem ser usados localmente, sem uma ligação à internet?*

**Resposta**: atualmente não oferecemos no local ou solução local.

-----

**Pergunta**: *que idiomas são suportados com imagem digitalizada?*

**Resposta**: os idiomas suportados incluem:

| | | Idiomas Suportados | | |
|---------------- |------------------ |------------------ |--------------------------- |--------------------
| Dinamarquês (da-DK)  | Holandês (nl-NL)     | Português           | Finlandês (fi-FI)            |Francês (fr-FR)
| Alemão (Alemanha-DE)  | Grego (el-GR)     | Húngaro (hu-HU) | Italiano (it-IT)            | Japonês (ja-JP)
| Coreano (ko-KR)  | Norueguês (não-nb) | Polonês (pl-PL)    | Português (pt-BR) (pt-PT) | Russo (ru-RU)
| Espanhol (es-ES)   | Sueco (sv-SV)     | Turco (tr-TU)   |                            |

-----

**Pergunta**: *visão do computador pode ser usado para ler os pratos de licença?*

**Resposta**: A API de imagem digitalizada oferece deteção de texto boa com o OCR, mas não está atualmente otimizado para pratos de licença. Estamos constantemente a tentar melhorar os nossos serviços e adicionou OCR para reconhecimento de placa de automática para nossa lista de pedidos de funcionalidades.

-----

**Pergunta:** *que idiomas são suportados para o reconhecimento de manuscrito?*

**Resposta**: atualmente, é suportada apenas em inglês.

-----

**Pergunta**: *que tipos de superfícies de escrita são suportados para o reconhecimento de manuscrito?*

**Resposta**: A tecnologia funciona com diferentes tipos de superfícies, incluindo quadros de comunicações, documentos técnicos e notas.

-----

**Pergunta**: *quanto a operação de reconhecimento de manuscrito demora?*

**Resposta**: A quantidade de tempo que demora depende o comprimento do texto. Para textos de mais tempo, ele pode demorar vários segundos. Portanto, depois de concluída a operação de reconhecer texto manuscrito, poderá ter de aguardar antes de pode recuperar os resultados usando a operação obter Resultado da operação texto manuscrito.

-----

**Pergunta**: *como é que o texto de identificador de tecnologia de reconhecimento de manuscrito que foi inserido usando um acento circunflexo no meio de uma linha?*

**Resposta**: tal texto é devolvido como uma linha separada pela operação de reconhecimento de manuscrito.

-----

**Pergunta**: *como a tecnologia de reconhecimento de manuscrito lidar com palavras ultrapassados-out ou linhas?*

**Resposta**: se as palavras são riscamos com várias linhas para capturá-los irreconhecível, a operação de reconhecimento de manuscrito não escolhê-los. No entanto, se as palavras são riscamos usando uma única linha, essa cruzamento é tratado como ruído e as palavras ainda obterem pegou pela operação de reconhecimento de manuscrito.

-----

**Pergunta**: *as orientações de texto são suportadas para a tecnologia de reconhecimento de manuscrito?*

**Resposta**: texto orientado em ângulos de até cerca de 30 graus para 40 graus pode obter pegou pela operação de reconhecimento de manuscrito.

-----
