---
title: "Atualização assíncrona de modelos do Azure Analysis Services | Microsoft Docs"
description: "Saiba como código de atualização assíncrona utilizando a REST API."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 12/18/2017
ms.author: owend
ms.openlocfilehash: 06d807b83f700c675c6979998dd8f74372a4845f
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2017
---
# <a name="asynchronous-refresh-with-the-rest-api"></a>Atualização assíncrona com a API REST
Ao utilizar qualquer linguagem de programação que suporta chamadas REST, pode efetuar operações de atualização de dados assíncrona nos seus modelos em tabela do Analysis Services do Azure. Isto inclui a sincronização de réplicas de só de leitura para consulta Escalamento horizontal. 

Operações de atualização de dados podem demorar algum tempo, dependendo de vários fatores, incluindo o volume de dados, o nível de otimização utilizando partições, etc. Estas operações tradicionalmente tem foi invocadas com métodos existentes como a [personalizada](https://docs.microsoft.com/sql/analysis-services/tabular-model-programming-compatibility-level-1200/introduction-to-the-tabular-object-model-tom-in-analysis-services-amo) (modelo de objeto em tabela), [PowerShell](https://docs.microsoft.com/sql/analysis-services/powershell/analysis-services-powershell-reference) cmdlets, ou [TMSL](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference) (modelo em tabela Scripting idioma). No entanto, estes métodos podem exigir a muitas vezes, as ligações de HTTP pouco fiáveis, de execução longa.

A API REST para o Analysis Services do Azure permite operações de atualização de dados a ser executadas no modo assíncrono. Ao utilizar a API REST, as ligações HTTP de longa execução de aplicações de cliente não necessárias. Existem também outras funcionalidades incorporadas para fiabilidade, por exemplo, tentativas automática e consolidações de batches.

## <a name="base-url"></a>URL Base

O URL de base segue este formato:

```
https://<rollout>.asazure.windows.net/servers/<serverName>/models/<resource>/
```

Por exemplo, considere um modelo com o nome AdventureWorks, num servidor com o nome myserver, localizado na região do Azure do Oeste dos EUA, o nome do servidor é:

```
asazure://westus.asazure.windows.net/myserver 
```

O URL de base para este nome de servidor é:

```
https://westus.asazure.windows.net/servers/myserver/models/AdventureWorks/ 
```

Ao utilizar o URL de base, recursos e operações possam ser acrescentadas com base no seguinte: 

![Atualização assíncrona](./media/analysis-services-async-refresh/aas-async-refresh-flow.png)

- Tudo o que termine em **s** é uma coleção.
- Tudo o que termina com **()** é uma função.
- Tudo o resto é um objeto/recursos.

Por exemplo, pode utilizar o verbo POST na coleção de atualizações para efetuar uma operação de atualização, como esta:

```
https://westus.asazure.windows.net/servers/myserver/models/AdventureWorks/refreshes
```

## <a name="authentication"></a>Autenticação

Todas as chamadas têm de ser autenticadas com um token do Azure Active Directory (OAuth 2) válido no cabeçalho de autorização e tem de cumprir os seguintes requisitos:

- O token tem de ser um token de utilizador ou um principal de serviço de aplicações.
- O utilizador ou a aplicação tem de ter permissões suficientes no servidor ou do modelo para efetuar a chamada de pedido. O nível de permissão é determinado pelas funções dentro do modelo ou do grupo de administrador no servidor.
- O token tem de ter o público-alvo correto definido como `https://*.asazure.windows.net`.

## <a name="post-refreshes"></a>POST /refreshes

Para efetuar uma operação de atualização, utilize o verbo POST na coleção /refreshes para adicionar um novo item de atualização na coleção. O cabeçalho de localização na resposta inclui o ID de atualização. A aplicação cliente pode desligar e verificar o estado mais tarde, se necessário, porque é assíncrona.

Operação de atualização apenas uma é aceite a uma hora para um modelo. Se existir uma operação de atualização em execução atual e outro é submetido, é devolvido o código de estado de conflito HTTP 409.

O corpo poderá assemelhar-se do seguinte:

```
{
    "Type": "Full",
    "CommitMode": "transactional",
    "MaxParallelism": 2,
    "RetryCount": 2,
    "Objects": [
        {
            "table": "DimCustomer",
            "partition": "DimCustomer"
        },
        {
            "table": "DimDate"
        }
    ]
}
```

### <a name="parameters"></a>Parâmetros
Não é necessário especificar parâmetros. A predefinição é aplicada.

|Nome  |Tipo  |Descrição  |Predefinição  |
|---------|---------|---------|---------|
|Tipo     |  Enum       |  O tipo de processamento para executar. Os tipos estão alinhados com o TMSL [atualizar comando](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/refresh-command-tmsl) tipos: completa, clearValues, calcular apenas dados automático, adicione e desfragmentar.       |   Automática      |
|CommitMode     |  Enum       |  Determina se os objetos serão confirmados em lotes ou apenas após concluir o procedimento. Modos incluem: partialBatch transacional, predefinição,.  |  transacional       |
|MaxParallelism     |   Int      |  Este valor determina o número máximo de threads para executar comandos de processamento em paralelo. Isto alinhada com a propriedade MaxParallelism que pode ser definida no TMSL [sequência comando](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/sequence-command-tmsl) ou através de outros métodos.       | 10        |
|retryCount    |    Int     |   Indica o número de vezes que a operação será repetida antes de falhar.      |     0    |
|Objetos     |   Matriz      |   Uma matriz de objetos a serem processados. Cada objeto inclui: "tabela" ao processar a tabela inteira ou "tabela" e "partição" ao processar uma partição. Se não existem objetos forem especificados, é atualizar o modelo de todo. |   Processar o modelo completo      |

CommitMode é igual ao partialBatch. É utilizado ao fazer uma carga de grandes conjuntos de dados inicial pode demorar horas. Se a operação de atualização falhar após a consolidação com êxito um ou mais lotes, permanecerá consolidadas os lotes consolidadas com êxito (esta irá não reverter lotes consolidadas com êxito).

> [!NOTE]
> No momento da escrita, o tamanho do lote é o valor de MaxParallelism, mas este valor foi alterado.

## <a name="get-refreshesrefreshid"></a>GET /refreshes/\<refreshId >

Para verificar o estado de uma operação de atualização, utilize o verbo GET o ID de atualização. Eis um exemplo do corpo da resposta. Se a operação está em curso, **em curso** é devolvido em estado.

```
{
    "startTime": "2017-12-07T02:06:57.1838734Z",
    "endTime": "2017-12-07T02:07:00.4929675Z",
    "type": "full",
    "status": "succeeded",
    "currentRefreshType": "full",
    "objects": [
        {
            "table": "DimCustomer",
            "partition": "DimCustomer",
            "status": "succeeded"
        },
        {
            "table": "DimDate",
            "partition": "DimDate",
            "status": "succeeded"
        }
    ]
}
```

## <a name="get-refreshes"></a>OBTER /refreshes

Para obter uma lista de operações de histórico de atualização para um modelo, utilize o verbo GET na coleção /refreshes. Eis um exemplo do corpo da resposta. 

> [!NOTE]
> No momento da escrita, os últimos 30 dias das operações de atualização são armazenados e devolvidos, mas pode alterar este número.

```
[
    {
        "refreshId": "1344a272-7893-4afa-a4b3-3fb87222fdac",
        "startTime": "2017-12-09T01:58:04.76",
        "endTime": "2017-12-09T01:58:12.607",
        "status": "succeeded"
    },
    {
        "refreshId": "474fc5a0-3d69-4c5d-adb4-8a846fa5580b",
        "startTime": "2017-12-07T02:05:48.32",
        "endTime": "2017-12-07T02:05:54.913",
        "status": "succeeded"
    }
]
```

## <a name="delete-refreshesrefreshid"></a>Eliminar /refreshes/\<refreshId >

Para cancelar uma operação de atualização em curso, utilize o verbo de eliminar o ID de atualização.

## <a name="post-sync"></a>POST /sync

Ter efetuar operações de atualização, poderá ser necessário sincronizar os novos dados com réplicas de escalamento horizontal de consulta. Para efetuar uma operação de sincronização para um modelo, utilize o verbo POST da função de /sync. O cabeçalho de localização na resposta inclui o ID de operação de sincronização.

## <a name="get-sync-status"></a>OBTER o estado de /sync

Para verificar o estado de uma operação de sincronização, utilize o verbo GET transmitir o ID de operação como parâmetro. Eis um exemplo do corpo da resposta:

```
{
    "operationId": "cd5e16c6-6d4e-4347-86a0-762bdf5b4875",
    "database": "AdventureWorks2",
    "UpdatedAt": "2017-12-09T02:44:26.18",
    "StartedAt": "2017-12-09T02:44:20.743",
    "syncstate": 2,
    "details": null
}
```

Valores da syncstate:

- 0: a replicar. Ficheiros de base de dados estão a ser replicados para uma pasta de destino.
- 1: reativar. A base de dados está a ser rehydrated em instâncias de servidor só de leitura.
- 2: concluir. A operação de sincronização foi concluída com êxito.
- 3: falha. A operação de sincronização falhou.
- 4: a finalizar o. A operação de sincronização foi concluída, mas está a efetuar os passos de limpeza.

## <a name="code-sample"></a>Exemplo de código

Eis um exemplo de código c# para ajudar a começar, [RestApiSample no GitHub](https://github.com/Microsoft/Analysis-Services/tree/master/RestApiSample).

### <a name="to-use-the-code-sample"></a>Para utilizar o exemplo de código

1.  Clonar ou transferir o repositório. Abra a solução de RestApiSample.
2.  Localizar a linha **cliente. BaseAddress =...** e forneça o [base URL](#base-url).

Pode utilizar o exemplo de código início de sessão interativo, nome de utilizador/palavra-passe, ou [principal de serviço](#service-principle).

#### <a name="interactive-login-or-usernamepassword"></a>Início de sessão interativo ou o nome de utilizador/palavra-passe

Esta forma de autenticação necessita de ser criada uma aplicação do Azure com as permissões de API necessárias atribuídas. 

1.  No portal do Azure, clique em **novo** > **do Azure Active Directory** > **registos de aplicação** > **novo registo de aplicação**.

    ![Novo registo de aplicação](./media/analysis-services-async-refresh/aas-async-app-reg.png)


2.  No **criar**, escreva um nome, selecione **nativo** tipo de aplicação. Para **URI de redirecionamento**, introduza **urn: ietf:wg:oauth:2.0:oob**e, em seguida, clique em **criar**.

    ![Definições](./media/analysis-services-async-refresh/aas-async-app-reg-name.png)

3.  Selecione a sua aplicação e, em seguida, copie e guarde o **ID da aplicação**.

    ![ID da aplicação de cópia](./media/analysis-services-async-refresh/aas-async-app-id.png)

4.  No **definições**, clique em **as permissões necessárias** > **adicionar**.

    ![Adicionar acesso à API](./media/analysis-services-async-refresh/aas-async-add.png)

5.  No **selecionar um API**, tipo **SQL Server Analysis Services** para a caixa de pesquisa e, em seguida, selecione **Azure Analysis Services (SQL Server Analysis Services Azure)**.

    ![Selecionar API](./media/analysis-services-async-refresh/aas-async-select-api.png)

6.  Selecione **ler e escrever todos os modelos**e, em seguida, clique em **selecione**. Quando ambas são selecionadas, clique em **feito** para adicionar as permissões. Pode demorar alguns minutos para propagar.

    ![Selecione a leitura e escrita todos os modelos](./media/analysis-services-async-refresh/aas-async-select-read.png)

7.  O exemplo de código, localize o **UpdateToken()** método. Observe o conteúdo deste método.
8.  Localizar **cadeia clientID =...** e, em seguida, introduza o **ID da aplicação** que copiou no passo 3.
9.  Execute o exemplo.

#### <a name="service-principal"></a>Principal de serviço

Consulte o [automatização do Azure Analysis Services com principais de serviço e o PowerShell](https://azure.microsoft.com/blog/automation-of-azure-analysis-services-with-service-principals-and-powershell/) blogue para saber como configurar um principal de serviço e atribuir as permissões necessárias no Azure Analysis Services. Depois de concluir os passos detalhados na mensagem de blogue, conclua os seguintes passos adicionais:

1.  O código de exemplo, encontrar **cadeia autoridade =...** , substitua **comuns** com a sua organização ID de inquilino.
2.  Comentário/anule os comentários para a classe de ClientCredential é utilizada para instanciar o objeto de cred. Certifique-se a \<ID da aplicação > e \<chave da aplicação > valores são acedidos de forma segura ou utilizar a autenticação baseada em certificado para principais de serviço.
3.  Execute o exemplo.


## <a name="see-also"></a>Consulte também

[Exemplos](analysis-services-samples.md)   
[API REST](https://docs.microsoft.com/rest/api/analysisservices/servers)   


