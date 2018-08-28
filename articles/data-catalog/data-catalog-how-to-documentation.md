---
title: Como documentar origens de dados no catálogo de dados do Azure
description: Artigo que mostra como realce de como documentar recursos de dados no catálogo de dados do Azure.
services: data-catalog
author: spelluru
ms.author: spelluru
ms.assetid: 053b1701-b848-4ada-b726-6f485caa9961
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: 4d3dfac425caf36e9ca9a4d3593bba336b18b7a3
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43053709"
---
# <a name="document-data-sources"></a>Documentar origens de dados
## <a name="introduction"></a>Introdução
**Catálogo de dados do Microsoft Azure** é um serviço cloud totalmente gerido que funciona como um sistema de registo e sistema de deteção de origens de dados empresariais. Em outras palavras, **catálogo de dados do Azure** é uma questão de ajudar as pessoas a descobrir, *compreender*e utilizar origens de dados e ajuda as organizações a obter mais valor dos seus dados existentes.

Quando uma origem de dados estiver registrada no **catálogo de dados do Azure**, seus metadados é copiado e indexados pelo serviço, mas a história não acaba nisso. **O catálogo de dados do Azure** também permite que os utilizadores forneçam a sua própria documentação completa que pode descrever a utilização e cenários comuns para a origem de dados.

Na [como anotar origens de dados](data-catalog-how-to-annotate.md), ficará a saber que os especialistas que conhecem a origem de dados podem anotá-lo com etiquetas e uma descrição. O **catálogo de dados do Azure** portal inclui um editor de texto formatado, para que os utilizadores totalmente podem documentar recursos de dados e contentores. O editor inclui a formatação de parágrafo, tais como cabeçalhos, formatação de texto, listas com marcas, listas numeradas e tabelas.

As etiquetas e descrições são ótimas para anotações simples. No entanto, para ajudar a compreender melhor a utilização de uma origem de dados e cenários comerciais para uma origem de dados de consumidores de dados, um especialista pode fornecer documentação detalhada e completa. É fácil de uma origem de dados de documentos. Selecione um recurso de dados ou o contentor e escolha **documentação**.

![](media/data-catalog-documentation/data-catalog-documentation.png)

## <a name="documenting-data-assets"></a>Como documentar recursos de dados
A vantagem **catálogo de dados do Azure** documentação permite-lhe utilizar o seu catálogo de dados como um repositório de conteúdo para criar uma narrativa completa dos seus recursos de dados. Pode explorar o conteúdo detalhado que descreve a contentores e tabelas. Se já tiver conteúdo de outro repositório de conteúdo, como o SharePoint ou uma partilha de ficheiros, pode adicionar as ligações de documentação do recurso para fazer referência a este conteúdo existente. Esta funcionalidade torna mais detectáveis seus documentos existentes.

> [!NOTE]
> Documentação não está incluída no índice de pesquisa.
>
>

![](media/data-catalog-documentation/data-catalog-documentation2.png)

O nível de documentação pode variar entre descrevendo as características e o valor de um contentor de elemento de dados para uma descrição detalhada do esquema da tabela dentro de um contêiner. O nível de documentação fornecida deve ser conduzido por suas necessidades empresariais. Mas em geral, aqui estão alguns prós e contras de como documentar recursos de dados:

* Apenas um contêiner de documentos: todo o conteúdo é num único local, mas podem não ter os detalhes necessários para os utilizadores tomar uma decisão informada.
* Apenas as tabelas de documentos: conteúdo é específico para esse objeto, mas os utilizadores têm vários locais para documentos.
* Documente a contentores e tabelas: uma abordagem mais abrangente, mas pode introduzir mais manutenção dos documentos.

## <a name="summary"></a>Resumo
Documentar origens de dados com **catálogo de dados do Azure** pode criar uma narrativa sobre seus recursos de dados da forma mais detalhada, conforme necessário.  Ao utilizar ligações, pode ligar ao conteúdo armazenado num repositório de conteúdo existente, que reúne os documentos existentes e recursos de dados. Assim que os utilizadores detetam recursos de dados apropriado, eles podem ter um conjunto completo de documentação.
