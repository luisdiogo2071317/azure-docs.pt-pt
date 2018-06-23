---
title: Personalização de conversão de texto API Microsoft tradutor | Microsoft Docs
description: Utilize o Microsoft tradutor Hub para criar o seu próprio sistema de tradução automática utilizando o estilo e terminologia-preferencial.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.topic: article
ms.date: 05/10/2018
ms.author: v-jansko
ms.openlocfilehash: 1db22a414c41f338c4e7fd6ce9dc7ac739fa9237
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354967"
---
# <a name="customize-your-text-translations"></a>Personalizar as traduções de texto

A pré-visualização do Microsoft personalizada tradutor é a funcionalidade do serviço Microsoft Translator, que permite aos utilizadores personalizar avançada neuronal tradução da Microsoft Translator ao traduzir texto utilizando a API de texto do Microsoft tradutor (apenas a versão 3). 

A funcionalidade também pode ser utilizada para personalizar a tradução de reconhecimento de voz quando utilizado com [pré-visualização de reconhecimento de voz de serviços cognitivos](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/).

## <a name="custom-translator"></a>Tradutor personalizado
Com o conversor de personalizado, pode criar sistemas de tradução neuronal perceber a terminologia utilizada na sua própria empresariais e da indústria. O sistema de tradução personalizado será, em seguida, integrar aplicações existentes, os fluxos de trabalho e Web sites. 

### <a name="how-does-it-work"></a>Como funciona?
Utilize os documentos anteriormente traduzidos (leaflets, páginas Web, documentação, etc.) para criar um sistema de tradução reflete a terminologia específicas do domínio e o estilo, melhor do que um sistema de tradução genérico. Os utilizadores podem carregar documentos TMX, XLIFF, TXT, DOCX e XLSX.  

O sistema também aceita dados que é paralela ao nível do documento, mas ainda não estão alinhados a nível de frases. Se os utilizadores têm acesso a versões do mesmo conteúdo em vários idiomas mas separado documentos personalizada tradutor conseguirá corresponder automaticamente frases entre documentos.  O sistema também pode utilizar dados monolingual num ou ambos os idiomas para complementar os dados de formação paralelas para melhorar as traduções. 

O sistema personalizado, em seguida, está disponível através de uma chamada regular para a Microsoft tradutor texto API utilizando o parâmetro de categoria.

O especificado do tipo adequado e a quantidade de dados de formação não é invulgar esperar ganhos entre 5 e 10 ou BLEU ainda mais pontos na qualidade de tradução utilizando tradutor personalizada.

Podem encontrar mais detalhes sobre os vários níveis de personalização com base nos dados disponíveis no [Guia do utilizador personalizada tradutor](http://aka.ms/CustomTranslatorDocs).


## <a name="microsoft-translator-hub"></a>Microsoft tradutor Hub

O Microsoft tradutor Hub legado pode ser utilizado para traduzir análises tradução. [Saiba mais](https://www.microsoft.com/en-us/translator/hub.aspx) 

## <a name="custom-translator-versus-hub"></a>Tradutor personalizado versus Hub

|   | **Hub** | **Tradutor personalizado**|
|:-----|:----:|:----:|
|Estado da funcionalidade de personalização   | Disponibilidade Geral  | Pré-visualização |
| Versão de API de texto  | Apenas v2   | Apenas v3 |
| Personalização de SMT | Sim   | Não | 
| Personalização de NMT | Não    | Sim |
| Novo unificada personalização de serviços de reconhecimento de voz | Não    | Sim | 
| [Nenhum rastreio](http://www.aka.ms/notrace) | Sim   | Sim | 

## <a name="collaborative-translations-framework"></a>Colaboração traduções Framework

> [!NOTE]
> A partir de 1 de Fevereiro de 2018, AddTranslation() e AddTranslationArray() já não estão disponíveis para utilização com a API de texto de tradutor v 2.0. Estes métodos falhará e nada será escrito. O texto de tradutor API v 3.0 não suporta estes métodos.

>Uma funcionalidade semelhante está disponível na API tradutor Hub. Consulte [ https://hub.microsofttranslator.com/swagger ](https://hub.microsofttranslator.com/swagger). 

## <a name="next-steps"></a>Passos Seguintes
> [!div class="nextstepaction"]
> [Configurar um sistema de idioma personalizado utilizando tradutor personalizada](http://aka.ms/CustomTranslatorDocs)
