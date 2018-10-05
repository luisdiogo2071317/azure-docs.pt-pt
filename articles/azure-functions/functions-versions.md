---
title: As funções runtime versões descrição geral do Azure
description: As funções do Azure suporta várias versões do tempo de execução. Aprenda as diferenças entre eles e como escolher aquele que é ideal para.
services: functions
documentationcenter: ''
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 10/03/2018
ms.author: glenga
ms.openlocfilehash: 372cf445e518ccdb287ce23ade6a3d92ddc5bc2b
ms.sourcegitcommit: 4edf9354a00bb63082c3b844b979165b64f46286
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48784904"
---
# <a name="azure-functions-runtime-versions-overview"></a>As funções runtime versões descrição geral do Azure

 Existem duas versões principais do runtime das funções do Azure: 1.x e 2.x. A versão atual, onde os projetos de novos recursos e melhorias estão sendo feitas é 2.x, embora ambos são suportadas para cenários de produção.  Os seguintes detalhes algumas das diferenças entre os dois, como pode criar cada versão e atualizar a partir do 1.x 2.x.

> [!NOTE] 
> Este artigo refere-se ao serviço cloud, as funções do Azure. Para obter informações sobre o produto de pré-visualização permite-lhe executar as funções do Azure no local, consulte a [descrição geral de tempo de execução de funções do Azure](functions-runtime-overview.md).

## <a name="creating-1x-apps"></a>Criar aplicações de 1.x

Novas aplicações criadas no Portal do Azure são definidas como 2.x por padrão, pois esta é a versão mais atual e em que estão sendo feitos novos investimentos de funcionalidade.  No entanto pode criar aplicações de v1.x efetuando o seguinte procedimento.

1. Criar uma função do Azure a partir do Portal do Azure
1. Abra a aplicação criada e, enquanto está em branco abrir o **definições de função**
1. Alterar a versão de ~ 2 para ~ 1.  *Esta alternância será desativada se tem funções na sua aplicação*.
1. Clique em salvar e reinicie a aplicação.  Todos os modelos agora devem criar e executar no 1.x.

## <a name="cross-platform-development"></a>Desenvolvimento entre plataformas

Tempo de execução 1.x suporta desenvolvimento e alojamento apenas no portal ou no Windows. Tempo de execução 2.x é executada no .NET Core 2, que significa que ele pode ser executado em todas as plataformas suportadas pelo .NET Core, incluindo o macOS e Linux. Isto permite o desenvolvimento entre plataformas e cenários de hospedagem.

## <a name="languages"></a>Linguagens

Tempo de execução 2.x usa um novo modelo de extensibilidade do idioma. Além disso, para melhorar o desempenho e as ferramentas, cada aplicação em 2.x está limitada a só ter funções num único idioma. Idiomas atualmente suportados no 2.x são c#, F #, JavaScript e Java. Linguagens experimentais do Azure funções 1.x não tenham sido atualizadas para usar o novo modelo, pelo que não são suportados no 2.x. A tabela seguinte indica quais linguagens de programação são suportadas em cada versão de runtime.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

Para obter mais informações, consulte [idiomas suportados](supported-languages.md).

## <a name="migrating-from-1x-to-2x"></a>Migrando do 1.x para o 2.x

Pode desejar mover uma aplicação existente, escrita em 1.x para 2.x.  A maioria das considerações de necessários na movimentação entre as versões está relacionadas com as alterações de tempo de execução de linguagem listadas acima (por exemplo c# mudando do .NET Framework 4.7 para .NET Core 2).  Terá de certificar-se de que seu código e as bibliotecas são compatíveis com os tempos de execução de linguagem a ser utilizados.  Certifique-se também de que tenha em atenção a quaisquer alterações no acionador, ligações e recursos, realçados abaixo.

### <a name="changes-in-triggers-and-bindings"></a>Alterações em acionadores e enlaces

Embora a maioria das propriedades de Acionador e vinculação e configurações permanece iguais nas versões, no 2.x terá de instalar qualquer acionador ou enlace para a aplicação. A única exceção para isso é acionadores HTTP e temporizador.  Ver [Registre-se e instalar as extensões de enlace](./functions-triggers-bindings.md#register-binding-extensions).  Tenha em atenção que também pode haver alterações na `function.json` ou atributos da função entre versões (por exemplo, o cosmos DB `connection` propriedade é agora `ConnectionStringSetting`).  Referência a [tabela de associação existente](#bindings) para obter ligações para documentação para cada ligação.

### <a name="changes-in-features-available"></a>Alterações em recursos disponíveis

Além das alterações em idiomas e enlaces, existem algumas funcionalidades que foram removidas, atualizadas ou substituídas entre versões.  Seguem-se algumas das principais considerações a tomar ao iniciar com 2.x depois de utilizar 1.x.  No v2.x foram efetuadas as seguintes alterações:

* Chaves para chamar uma função sempre serão armazenadas no armazenamento de BLOBs encriptados. No 1.x por predefinição estivessem no armazenamento de ficheiros e podem ser movidas para se a ativação de funcionalidades, como blocos de Blobs.  Atualizar uma aplicação de 1.x para o 2.x e segredos está atualmente no armazenamento de ficheiros serão repostas.
* Para melhorar o desempenho, os acionadores de tipo "webhook" são removidos e substituídos por acionadores "HTTP".
* Configuração do anfitrião (`host.json`) deverá ou estar vazios ou conter `version` de `2.0` para uma das propriedades.
* Para melhorar a monitorização e observability, o Dashboard de WebJobs (`AzureWebJobsDashboard`) é substituído pelo [Azure Application Insights](functions-monitoring.md) (`APPINSIGHTS_INSTRUMENTATIONKEY`)
* Definições da aplicação (`local.settings.json`) requer um valor para a propriedade `FUNCTIONS_WORKER_RUNTIME` que mapeia para o idioma da aplicação `dotnet | node | java | python`.
    * Para melhorar o tempo de requisitos de espaço e arranque, as aplicações estão limitadas a um único idioma. Pode publicar várias aplicações para que as funções em idiomas diferentes para a mesma solução.
* Tempo limite predefinido para funções num plano do serviço de aplicações é de 30 minutos.  Ele pode ainda ser definido manualmente ilimitado.
* Limitações de simultaneidade HTTP são implementadas por predefinição para as funções do plano de consumo (100 solicitações simultâneas por instância).  Estas definições podem ser modificadas por meio do `host.json` ficheiro.
* [Devido ao .NET core limitações](https://github.com/Azure/azure-functions-host/issues/3414), `.fsx` scripts para funções do F # foram removidas. As funções do F # compiladas ainda são suportadas.
* O formato de acionadores baseados em webhooks (por exemplo, o Event Grid) foi alterado para `https://{app}/runtime/webhooks/{triggerName}`

### <a name="upgrading-a-locally-developed-application"></a>Atualizar um aplicativo desenvolvido localmente

Se a sua aplicação v1.x foi desenvolvida localmente, pode efetuar alterações na aplicação ou projeto para que seja compatível com v2.  Recomenda-se para criar uma nova aplicação e a porta sobre o código para a nova aplicação.  Apesar de existirem alterações que poderiam ser feitas a uma aplicação existente para executar um ponto de partida em atualização, existem vários outros aprimoramentos entre v1 e v2 probabilidade de código legado não está a tirar partido de (por exemplo em c# a alteração da `TraceWriter` para `ILogger`) .  

O caminho recomendado é começar com um dos modelos v2 e mova sobre código num novo projeto ou a aplicação.

#### <a name="visual-studio-runtime-versions"></a>Versões de tempo de execução do Visual Studio

No Visual Studio selecionar a versão de runtime quando cria um projeto.  Visual Studio tem os bits para ambas as versões principais e dinamicamente pode utilizar a uma certa para o projeto.  Estas definições são derivadas do `.csproj` ficheiro.  Para aplicações de 1.x o projeto tem as propriedades

```xml
<TargetFramework>net461</TargetFramework>
<AzureFunctionsVersion>v1</AzureFunctionsVersion>
```

No v2 são as propriedades do projeto

```xml
<TargetFramework>netstandard2.0</TargetFramework>
<AzureFunctionsVersion>v2</AzureFunctionsVersion>
```

Clicar em debug ou publicar será definido corretamente a versão correta para as definições do projeto.

#### <a name="vs-code-and-azure-functions-core-tools"></a>O VS Code e ferramentas de núcleo das funções do Azure

Outras ferramentas local conta com as ferramentas de núcleo de funções do Azure.  Essas ferramentas são instaladas na máquina e apenas uma versão é instalada geralmente numa máquina de desenvolvimento de uma só vez.  Ver [instruções de como instalar versões específicas das ferramentas de núcleo](./functions-run-local.md).

Para o VS Code também poderá ter de atualizar a definição de utilizador para o `azureFunctions.projectRuntime` para corresponder à versão das ferramentas instalado.  Isto também irá atualizar os modelos e linguagens apresentadas durante a criação de novas aplicações.

### <a name="changing-version-of-apps-in-azure"></a>Alterar versão das aplicações no Azure

Versões da aplicação publicada são definidas através da definição de aplicação `FUNCTIONS_EXTENSION_VERSION`.  Ele é definido como `~2` para aplicações v2, e `~1` para aplicações v1.  É recomendável usar para alterar a versão de tempo de execução de uma aplicação que tenha publicadas sem também alterar o código de uma dessas funções de funções existentes.  O caminho recomendado é criar uma nova aplicação de função e definido para a versão adequada, testar as alterações e, em seguida, desativar ou eliminar a aplicação anterior.

## <a name="bindings"></a>Enlaces 

Tempo de execução 2.x usa um novo [modelo de extensibilidade de ligação](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) que oferece essas vantagens:

* Suporte para extensões de vinculação de terceiros.
* Desacoplamento do tempo de execução e enlaces. Isso permite que as extensões de vinculação ser uma versão e lançado de forma independente. Pode, por exemplo, optar por atualizar para uma versão de uma extensão que se baseia numa versão mais recente de um SDK subjacente.
* Um mais leve ambiente de execução, em que apenas as associações em utilização são conhecidas e carregadas pelo tempo de execução.

Todos os enlaces de funções do Azure incorporados ADOTARAM esse modelo e já não estão incluídos por predefinição, exceto para o acionador de temporizador e o acionador HTTP. Essas extensões são instaladas automaticamente quando cria funções através de ferramentas como o Visual Studio ou através do portal.

A tabela seguinte indica as ligações são suportadas em cada versão de runtime.

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações, consulte os seguintes recursos:

* [Criar código e testar as Funções do Azure localmente](functions-run-local.md)
* [Como as versões de tempo de execução de funções do Azure de destino](set-runtime-version.md)
* [Notas de versão](https://github.com/Azure/azure-functions-host/releases)
