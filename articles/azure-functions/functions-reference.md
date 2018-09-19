---
title: Orientações para desenvolver as funções do Azure | Documentos da Microsoft
description: Aprenda os conceitos de funções do Azure e técnicas que precisa para desenvolver as funções no Azure, em todos os enlaces e linguagens de programação.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: funções de guia, do azure programador, funções, processamento de eventos, webhooks, computação dinâmica, arquitetura sem servidor
ms.assetid: d8efe41a-bef8-4167-ba97-f3e016fcd39e
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 10/12/2017
ms.author: glenga
ms.openlocfilehash: d97766b0a8c0df3b414d78f563406530f67c313b
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46125379"
---
# <a name="azure-functions-developers-guide"></a>Guia para programadores do funções do Azure
Nas funções do Azure, funções específicas partilham alguns conceitos técnicos de núcleos e componentes, independentemente do idioma ou associação usada. Antes de conhecer em detalhes específicos para um determinado idioma ou enlace de aprendizagem, certifique-se de que leia esta visão geral que se aplica a todos eles.

Este artigo pressupõe que já leu a [descrição geral das funções do Azure](functions-overview.md) e estão familiarizados com [conceitos de SDK do WebJobs como acionadores e enlaces e o tempo de execução JobHost](https://github.com/Azure/azure-webjobs-sdk/wiki). As funções do Azure baseia-se no SDK do WebJobs. 

## <a name="function-code"></a>Código de função
R *função* é o principal conceito nas funções do Azure. Escrever código para uma função numa linguagem à sua escolha e guarde os ficheiros de código e a configuração na mesma pasta. A configuração com o nome `function.json`, que contém dados de configuração do JSON. Há suporte para vários idiomas, e cada um deles tem uma experiência de um pouco diferente, otimizada para funcionar melhor para esse idioma. 

O ficheiro de Function define os enlaces de função e outras definições de configuração. O tempo de execução utiliza este ficheiro para determinar os eventos a monitorizar e como passar dados para e devolver dados de execução de função. Segue-se um exemplo de arquivo Function.

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

Definir o `disabled` propriedade `true` para impedir que a função a ser executada.

O `bindings` propriedade é onde configura acionadores e enlaces. Cada ligação compartilha algumas configurações comuns e algumas definições que são específicas para um determinado tipo de enlace. Cada ligação requer as seguintes definições:

| Propriedade | Tipos de valores / | Comentários |
| --- | --- | --- |
| `type` |cadeia |Tipo de enlace. Por exemplo, `queueTrigger`. |
| `direction` |'em', 'out' |Indica se a ligação é para receber dados para a função ou enviar dados a partir da função. |
| `name` |cadeia |O nome que é utilizado para dados vinculados na função. No c#, isso é um nome de argumento; para JavaScript, é a chave numa lista de chave/valor. |

## <a name="function-app"></a>Function app
Uma aplicação de funções é composta por um ou mais funções individuais que são geridas em conjunto pelo App Service do Azure. Todas as funções numa aplicação de funções de partilham o mesmo plano de preços, a implementação contínua e a versão de runtime. As funções escritas em várias linguagens podem partilhar a mesma aplicação de função. Considere uma aplicação de funções como uma forma de organizar e gerir coletivamente as suas funções. 

## <a name="runtime-script-host-and-web-host"></a>Tempo de execução (anfitrião de script e web host)
O tempo de execução, ou o anfitrião de script, é o anfitrião subjacente do SDK do WebJobs que está à escuta de eventos, coleta e envia os dados e, por fim, executa o seu código. 

Para facilitar a acionadores HTTP, há também um host da web que foi concebido para na frente o host de script em cenários de produção. Ter dois anfitriões ajuda a isolar o host do script da frente termina gerido pelo web host de tráfego.

## <a name="folder-structure"></a>estrutura de pastas
[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Quando configurando um projeto para a implementação de funções para uma aplicação de funções no Azure, pode lidar com essa estrutura de pasta como seu código de site. Recomendamos que utilize [implementação do pacote](deployment-zip-push.md) para implementar o projeto para a aplicação de funções no Azure. Também pode utilizar as ferramentas existentes, como [integração e implementação contínuas](functions-continuous-deployment.md) e DevOps do Azure.

> [!NOTE]
> Certifique-se implementar o seu `host.json` de ficheiros e pastas de função diretamente para o `wwwroot` pasta. Não inclua o `wwwroot` pasta das implementações. Caso contrário, acaba tendo `wwwroot\wwwroot` pastas.

## <a id="fileupdate"></a> Como atualizar os ficheiros de aplicação de função
O editor de função incorporado no portal do Azure permite-lhe atualizar o *Function* arquivo e o arquivo de código para uma função. Para carregar ou atualizar outros ficheiros, tal como *Package. JSON* ou *Project* ou dependências, precisa usar outros métodos de implantação.

As aplicações de função são criadas no serviço de aplicações, por isso, todos os [opções de implementação disponíveis para aplicações web padrão](../app-service/app-service-deploy-local-git.md) também estão disponíveis para aplicações de funções. Aqui estão alguns métodos que pode utilizar para carregar ou atualizar os ficheiros de aplicação de função. 

#### <a name="to-use-app-service-editor"></a>Para utilizar o Editor do serviço de aplicações
1. No portal das funções do Azure, clique em **funcionalidades de plataforma**.
2. Na **ferramentas de desenvolvimento** secção, clique em **Editor do serviço de aplicações**.   
   Após carregar o Editor do serviço de aplicações, verá os *Host. JSON* função de ficheiros e pastas sob *wwwroot*. 
5. Abrir ficheiros editá-los, ou arrastar e soltar do seu computador de desenvolvimento para carregar ficheiros.

#### <a name="to-use-the-function-apps-scm-kudu-endpoint"></a>Para utilizar o ponto final do SCM (Kudu) da aplicação de funções
1. Navegue para: `https://<function_app_name>.scm.azurewebsites.net`.
2. Clique em **consola de depuração > CMD**.
3. Navegue para `D:\home\site\wwwroot\` para atualizar *Host. JSON* ou `D:\home\site\wwwroot\<function_name>` para ficheiros de uma função de atualização.
4. Arrastar e largar um ficheiro que pretende carregar para a pasta apropriada na grelha de ficheiro. Existem duas áreas na grelha de arquivo onde pode soltar um arquivo. Para *. zip* arquivos, aparece uma caixa de com a etiqueta "Arraste aqui para carregar e deszipe." Para outros tipos de ficheiro, remova na grelha de ficheiro, mas os padrões de "deszipe".

<!--NOTE: I've removed documentation on FTP, because it does not sync triggers on the consumption plan --glenga -->

#### <a name="to-use-continuous-deployment"></a>Para utilizar a implementação contínua
Siga as instruções no tópico [implementação contínua para funções do Azure](functions-continuous-deployment.md).

## <a name="parallel-execution"></a>Execução paralela
Quando vários eventos acionadora ocorrem mais rapidamente do que um tempo de execução de função de thread único pode processá-los, o tempo de execução poderá invocar a função várias vezes em paralelo.  Se estiver a utilizar uma aplicação de funções a [plano de alojamento de consumo](functions-scale.md#how-the-consumption-plan-works), a aplicação de função pode aumentar horizontalmente a automaticamente.  Cada instância da aplicação de função, se a aplicação for executada no consumo ou um regulares do plano de alojamento [plano de alojamento do App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), pode processar chamadas simultâneas de função em paralelo com vários threads.  O número máximo de chamadas de função em simultâneo em cada instância de aplicação de função varia consoante o tipo de Acionador a ser utilizado, bem como os recursos utilizados por outras funções na aplicação de função.

## <a name="functions-runtime-versioning"></a>Controlo de versões de tempo de execução de funções

Pode configurar a versão do uso de tempo de execução de funções a `FUNCTIONS_EXTENSION_VERSION` definição de aplicação. Por exemplo, o valor "~ 1" indica que a aplicação de funções irá utilizar 1 como sua versão principal. Aplicações de função são atualizadas para cada nova versão secundária, à medida que são lançadas. Para obter mais informações, incluindo como ver a versão exata da sua aplicação de função, veja [como versões de tempo de execução de funções do Azure de destino](set-runtime-version.md).

## <a name="repositories"></a>Repositórios
O código das funções do Azure é open source e armazenados nos repositórios do GitHub:

* [Runtime das funções do Azure](https://github.com/Azure/azure-webjobs-sdk-script/)
* [Portal de funções do Azure](https://github.com/projectkudu/AzureFunctionsPortal)
* [Modelos do funções do Azure](https://github.com/Azure/azure-webjobs-sdk-templates/)
* [SDK de WebJobs do Azure](https://github.com/Azure/azure-webjobs-sdk/)
* [Extensões do SDK de WebJobs do Azure](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a>Enlaces
Aqui está uma tabela de todos os enlaces suportados.

[!INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

Está a ter problemas com erros proveniente de associações? Reveja os [códigos de erro de ligação do Azure funções](functions-bindings-error-pages.md) documentação.

## <a name="reporting-issues"></a>Problemas de relatórios
[!INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)]

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações, consulte os seguintes recursos:

* [Best Practices for Azure Functions (Melhores Práticas para as Funções do Azure)](functions-best-practices.md)
* [Referência do Azure funções c# para programadores](functions-reference-csharp.md)
* [Referência do Azure funções F # para programadores](functions-reference-fsharp.md)
* [Referência do desenvolvedor do NodeJS de funções do Azure](functions-reference-node.md)
* [Acionadores de funções do Azure e enlaces](functions-triggers-bindings.md)
* [As funções do Azure: A jornada](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) no blog da equipe de serviço de aplicações do Azure. Um histórico de como as funções do Azure foi desenvolvida.

