---
title: A criação de Visual no Azure Data Factory | Microsoft Docs
description: Saiba como utilizar o visual criação no Azure Data Factory
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/01/2018
ms.author: shlo
ms.openlocfilehash: b588fd4b67dbed1e0dc3d4ad2cbd75b462ce311f
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34725147"
---
# <a name="visual-authoring-in-azure-data-factory"></a>A criação de Visual no Azure Data Factory
O Azure Data Factory utilizador interface experiência (UX) permite-lhe visualmente cria e implementar os recursos para a fábrica de dados sem ter de escrever qualquer código. Pode arraste as actividades à tela pipeline, efetuar execuções do teste, iteratively, depurar e implementar e monitorizar a execução de pipeline. Existem duas abordagens para utilizar o UX para efetuar a criação de visual:

- Autor diretamente com o serviço fábrica de dados.
- Autor com a integração de Git de serviços de equipa do Visual Studio (VSTS) para colaboração, controlo de origem ou controlo de versões.

## <a name="author-directly-with-the-data-factory-service"></a>Autor diretamente com o serviço fábrica de dados
A criação de Visual com o serviço Data Factory difere da criação visual com VSTS de duas formas:

- O serviço fábrica de dados não inclui um repositório para armazenar as entidades JSON para as suas alterações.
- O serviço fábrica de dados não está otimizado para colaboração ou controlo de versão.

![Configurar o serviço fábrica de dados ](media/author-visually/configure-data-factory.png)

Quando utiliza o UX **criação tela** para criar diretamente com o serviço fábrica de dados, apenas o **publicar todas as** modo está disponível. Quaisquer alterações efetuadas são publicadas diretamente para o serviço fábrica de dados.

![Modo de publicação](media/author-visually/data-factory-publish.png)

## <a name="author-with-vsts-git-integration"></a>Autor com a integração de VSTS Git
Criação visual com a integração de VSTS Git suporta o controlo de origem e de colaboração for work nos seus pipelines de fábrica de dados. Pode associar uma fábrica de dados com um repositório de VSTS Git conta para o controlo de origem, colaboração, controlo de versões e assim sucessivamente. Uma única conta de VSTS Git pode ter vários repositórios, mas um repositório de VSTS Git pode ser associado a fábrica de dados apenas um. Se não tiver uma conta VSTS ou repositório, siga [estas instruções](https://docs.microsoft.com/vsts/accounts/create-account-msa-or-work-student) para criar os seus recursos.

> [!NOTE]
> Pode armazenar ficheiros de dados e de script num repositório de VSTS GIT. No entanto, tem de carregar os ficheiros manualmente ao Storage do Azure. Um pipeline do Data Factory não carregar automaticamente ficheiros de script ou dados armazenados num repositório de GIT de VSTS ao Storage do Azure.

### <a name="configure-a-vsts-git-repository-with-azure-data-factory"></a>Configurar um repositório de VSTS Git com o Azure Data Factory
Pode configurar um repositório de VSTS GIT com uma fábrica de dados através de dois métodos.

#### <a name="method1"></a> O método de configuração 1: vamos iniciar a página de introdução

No Azure Data Factory, vá para o **comecemos** página. Selecione **configurar repositório de código**:

![Configurar um repositório de código VSTS](media/author-visually/configure-repo.png)

O **repositório definições** é apresentado o painel de configuração:

![Configurar as definições do repositório de código](media/author-visually/repo-settings.png)

O painel mostra o seguinte código VSTS repositório:

| Definição | Descrição | Valor |
|:--- |:--- |:--- |
| **Tipo de repositório** | O tipo do repositório de código VSTS.<br/>**Tenha em atenção**: GitHub não é atualmente suportado. | Visual Studio Team Services Git |
| **Azure Active Directory** | Nome do seu inquilino do Azure AD. | <your tenant name> |
| **Conta do Visual Studio Team Services** | Nome da sua conta VSTS. Pode localizar o nome da sua conta VSTS em `https://{account name}.visualstudio.com`. Pode [iniciar sessão na sua conta VSTS](https://www.visualstudio.com/team-services/git/) para aceder ao seu perfil do Visual Studio e ver as repositórios e projetos. | \<nome da sua conta > |
| **ProjectName** | O nome de projeto VSTS. Pode localizar o nome do projeto VSTS em `https://{account name}.visualstudio.com/{project name}`. | \<o nome do projeto VSTS > |
| **RepositoryName** | O nome de repositório de código VSTS. Projetos VSTS contêm repositórios de Git para gerir o seu código de origem à medida que cresce o projeto. Pode criar um novo repositório ou utilizar um repositório existente que já se encontra no seu projeto. | \<o nome de repositório de código VSTS > |
| **Ramo de colaboração** | O ramo de colaboração VSTS que será utilizado para publicação. Por predefinição, é `master`. Altere esta opção caso pretenda publicar recursos a partir de outra sucursal. | \<o nome da sua sucursal colaboração > |
| **Pasta raiz** | A pasta de raiz no seu ramo de colaboração VSTS. | \<nome da pasta de raiz > |
| **Importar os recursos de fábrica de dados existentes para o repositório** | Especifica se pretende importar recursos existentes de fábrica de dados a partir do UX **criação tela** para um repositório de VSTS Git. Selecione a caixa para importar os seus recursos de fábrica de dados para o repositório de Git associado no formato JSON. Esta ação exporta cada recurso individualmente (ou seja, os serviços ligados e conjuntos de dados são exportados para JSONs separados). Quando esta caixa não está selecionada, os recursos existentes não são importados. | Selecionado (predefinição) |

#### <a name="configuration-method-2-ux-authoring-canvas"></a>O método de configuração 2: UX criação tela
No UX de fábrica de dados do Azure **criação tela**, localize a fábrica de dados. Selecione o **Data Factory** menu pendente e, em seguida, selecione **configurar repositório de código**.

É apresentado um painel de configuração. Para obter detalhes sobre as definições de configuração, consulte as descrições no <a href="#method1">método de configuração 1</a>.

![Configurar as definições do repositório de código para a criação do UX](media/author-visually/configure-repo-2.png)

#### <a name="switch-to-a-different-git-repo"></a>Mudar para um repositório de Git diferentes

Para mudar para um repositório de Git diferentes, localize o ícone no canto superior direito da página de descrição geral do Data Factory, conforme mostrado na captura de ecrã seguinte. Se não vir o ícone, limpe a cache local do browser. Selecione o ícone para remover a associação com o repositório atual.

Depois de remover a associação a atual repositório, pode configurar as definições de Git para utilizar um repositório diferentes. Em seguida, pode importar recursos de fábrica de dados existentes para o repositório de novo.

![Remova a associação com o repositório de Git atual.](media/author-visually/remove-repo.png)

### <a name="use-version-control"></a>Utilizar o controlo de versão
Sistemas de controlo de versão (também conhecido como _controlo de origem_) permitem aos programadores colaborar em código e controlar as alterações efetuadas para o código base. Controlo de origem é uma ferramenta essencial para projetos de programação multi.

Cada repositório de VSTS Git que está associada a uma fábrica de dados tem uma um ramo de colaboração. (`master` é o ramo de colaboração predefinido). Os utilizadores também podem criar ramos de funcionalidade clicando **+ novo ramo** e desenvolvimento nos ramos de funcionalidade.

![Alterar o código de sincronização ou publicando](media/author-visually/sync-publish.png)

Quando estiver preparado com o desenvolvimento da funcionalidade no seu ramo funcionalidade, pode clicar em **criar pedido de solicitação**. Isto leva-o para GIT VSTS, onde pode aumentar solicitação pedidos, código de revisões e intercalação muda para o ramo de colaboração. (`master` é a predefinição). Só são permitidas para publicar para o serviço fábrica de dados a partir do ramo colaboração. 

![Criar um novo pedido de solicitação](media/author-visually/create-pull-request.png)

#### <a name="publish-code-changes"></a>Publicar as alterações de código
Depois de ter intercalar as alterações para o ramo de colaboração (`master` é a predefinição), selecione **publicar** publicar manualmente as alterações de código no ramo principal para o serviço fábrica de dados.

![Publicar as alterações no serviço Data Factory](media/author-visually/publish-changes.png)

> [!IMPORTANT]
> O ramo principal não é representativo do que é implementado no serviço Data Factory. O ramo principal *tem* publicados manualmente o serviço fábrica de dados.

## <a name="use-the-expression-language"></a>Utilizar a linguagem de expressão
Pode especificar as expressões para valores de propriedade utilizando a linguagem de expressão que é suportada pelo Azure Data Factory. 

Especificar expressões de valores de propriedade selecionando **adicionar conteúdo dinâmico**:

![Utilizar a linguagem de expressão](media/author-visually/dynamic-content-1.png)

## <a name="use-functions-and-parameters"></a>Utilize funções e parâmetros

Pode utilizar as funções ou especificar parâmetros para conjuntos de dados e pipelines na fábrica de dados **builder expressão**:

Para obter informações sobre as expressões suportadas, consulte [expressões e as funções no Azure Data Factory](control-flow-expression-language-functions.md).

![Adicionar conteúdo dinâmico](media/author-visually/dynamic-content-2.png)

## <a name="provide-feedback"></a>Enviar comentários
Selecione **comentários** comente sobre as funcionalidades ou notificar a Microsoft sobre problemas com a ferramenta:

![Comentários](media/author-visually/provide-feedback.png)

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre monitorizar e gerir pipelines, consulte [monitorizar e gerir pipelines programaticamente](monitor-programmatically.md).
