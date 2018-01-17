---
title: "Visualmente criar fábricas de dados do Azure | Microsoft Docs"
description: "Saiba como criar visualmente fábricas de dados do Azure"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/9/2018
ms.author: shlo
ms.openlocfilehash: 3e67665facba78c4ca8e2317f0323b4c5c02a49c
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/16/2018
---
# <a name="visually-author-data-factories"></a>Visualmente fábricas de dados do autor
Com a experiência de UX de fábrica de dados do Azure, os utilizadores visualmente podem criar e implementar recursos na sua fábrica de dados sem ter de escrever uma única linha de código. Esta interface sem código permite-lhe arrastar e largar atividades numa tela pipeline, efetuar execuções do teste, iteratively, depurar e implementar e monitorizar a execução de pipeline. Pode optar por utilizar a ferramenta de ADF UX de duas formas:

1. Trabalhar diretamente com o serviço fábrica de dados
2. Configurar a integração de Git VSTS para colaboração, controlo de origem ou controlo de versões

## <a name="authoring-with-data-factory"></a>Criação com o Data Factory
A primeira opção é criar diretamente com o modo de fábrica de dados. Esta abordagem é diferente da criação através do repositório de código VSTS em que não há nenhum repositório armazenar as entidades JSON das alterações, nem está otimizado para colaboração ou controlo de versão.

![Configurar fábrica de dados](media/author-visually/configure-data-factory.png)

No modo de fábrica de dados, há apenas o modo de "Publicar". Quaisquer alterações efetuadas são publicadas diretamente para o serviço fábrica de dados.

![Publicar a fábrica de dados](media/author-visually/data-factory-publish.png)

## <a name="authoring-with-vsts-git-integration"></a>Criação com a integração de Git VSTS
Permite a criação com a integração de VSTS Git para controlo de origem e de colaboração ao criar os seus pipelines de fábrica de dados. Os utilizadores têm a opção para associar uma fábrica de dados com um repositório de VSTS Git conta para o controlo de origem, a colaboração e controlo de versões etc. Uma única conta de VSTS GIT pode ter vários repositórios. No entanto, um repositório de VSTS Git só pode ser associado a uma fábrica de dados individual. Se ainda não tiver uma conta VSTS e repositório, crie um [aqui](https://docs.microsoft.com/en-us/vsts/accounts/create-account-msa-or-work-student).

### <a name="configure-vsts-git-repo-with-azure-data-factory"></a>Configurar VSTS repositório de Git do Azure Data Factory
Os utilizadores podem configurar um repositório de VSTS GIT com uma fábrica de dados através de dois métodos.

#### <a name="method-1-lets-get-started-page"></a>Método 1: 'comecemos' página

Ir para a página 'Comecemos' e clique em 'Configurar o repositório de código'

![Configurar o repositório de código](media/author-visually/configure-repo.png)

A partir daí, é apresentado um painel do lado para configurar as definições do repositório.

![Configurar as definições do repositório](media/author-visually/repo-settings.png)
* **Tipo de repositório**: Git de serviços de equipa do Visual Studio (atualmente, Github não é suportada.)
* **Conta de serviços de equipa do Visual Studio**: O nome da conta pode ser encontrado do nome de https://{account}. visualstudio.com. Inicie sessão na sua conta VSTS [aqui](https://www.visualstudio.com/team-services/git/) e aceder ao seu perfil do Visual Studio para ver os repositórios e projetos
* **ProjectName:** pode encontrar o nome do projeto https://{account name}.visualstudio.com/{project nome}
* **RepositoryName:** o nome do repositório. Projetos VSTS contêm repositórios de Git para gerir o seu código de origem à medida que cresce o projeto. Crie um novo repositório ou utilizar um repositório existente já no projeto.
* **Importar os recursos de fábrica de dados existentes para o repositório**: ao selecionar esta caixa, pode importar os recursos de fábrica de dados atual criados na tela UX para o repositório de VSTS GIT associado no formato JSON. Esta ação exporta cada recurso individualmente (ou seja, serviços ligados e conjuntos de dados são exportados para JSONs separados).    Se desmarcar esta caixa de verificação, os recursos existentes não são importados para o repositório de Git.

#### <a name="method-2-from-authoring-canvas"></a>Método 2: De criação tela

No 'tela criação', clique no menu de lista pendente de fábrica de dados em seu nome de factory de dados. Em seguida, clique em 'Configurar o repositório de código.' Semelhante ao **1 método**, é apresentado um painel do lado para configurar as definições do repositório. Consulte as secções anteriores para obter informações sobre as definições.

![Configurar o repositório de código 2](media/author-visually/configure-repo-2.png)

### <a name="version-control"></a>Controlo de versão
Controlo de versão, também referido para como controlo de origem, sistemas de permitir que os programadores de colaborar em código e controlar as alterações efetuadas para o código base. Controlo de origem é uma ferramenta essencial para projetos de programação multi.

Cada repositório de VSTS Git depois de associados uma fábrica de dados tem um ramo principal. A partir daí, cada utilizador que tenha acesso ao repositório de VSTS Git tem duas opções quando efetuar alterações: sincronizar e publicar.

![Publicação de sincronização](media/author-visually/sync-publish.png)

#### <a name="sync"></a>Sync

Assim que clicar em 'sync', pode solicitar as alterações do ramo principal para o ramo local ou emitir alterações a partir do ramo local para o ramo principal.

![A sincronizar as alterações](media/author-visually/sync-change.png)

#### <a name="publish"></a>Publicar
 Publica as alterações no ramo principal para o serviço fábrica de dados.

> [!NOTE]
> O **ramo principal não é representativo do que é implementado no serviço Data Factory.** O ramo principal *tem* publicados manualmente o serviço fábrica de dados.




## <a name="expression-language"></a>Expressão de Idioma

Os utilizadores podem especificar expressões na definir valores de propriedade utilizando a linguagem de expressão suportada pelo Azure Data Factory. Consulte [expressões e as funções no Azure Data Factory](control-flow-expression-language-functions.md) para mais sobre os quais as expressões são suportadas.

Especificar expressões de valores de propriedade no UX desta forma.

![Expressão de Idioma](media/author-visually/expression-language.png)

## <a name="parameters"></a>Parâmetros
Os utilizadores podem especificar parâmetros para Pipelines e conjuntos de dados, no separador 'Parameters'. Além disso, utilizar parâmetros nas propriedades facilmente premindo em "Adicionar conteúdo dinâmico".

![Conteúdo dinâmico](media/author-visually/dynamic-content.png)

A partir daí, pode utilizar um parâmetro existente ou especifique um novo parâmetro num valor de propriedade.

![Parâmetros](media/author-visually/parameters.png)

## <a name="feedback"></a>Comentários
Clique no ícone 'Comentários' para fornecer comentários (Microsoft) em várias funcionalidades ou quaisquer problemas que poderá funcionar.

![Comentários](media/monitor-visually/feedback.png)

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre como monitorizar e gerir pipelines, consulte [monitorizar e gerir pipelines programaticamente](monitor-programmatically.md) artigo
