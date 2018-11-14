---
title: Descrição geral da integração de mensagens do Azure Blockchain Workbench
description: Descrição geral da utilização de mensagens no Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 11/12/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: f8f3584475415cf9ca19458f6da78d34df37f438
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/13/2018
ms.locfileid: "51614366"
---
# <a name="azure-blockchain-workbench-messaging-integration"></a>O Azure Blockchain Workbench, integração de mensagens

Além de fornecer uma API REST, o Azure Blockchain Workbench também fornece integração baseados em mensagens. Workbench publica eventos centrada em razão através do Azure Event Grid, permitindo que os consumidores de downstream para ingestão de dados ou tomar medidas com base nestes eventos. Para os clientes que necessitam de sistema de mensagens confiável, o Azure Blockchain Workbench entregam mensagens para um ponto final do Azure Service Bus também.

## <a name="input-apis"></a>APIs de entrada

Se quiser iniciar transações de sistemas externos para criar utilizadores, criar contratos e atualizar contratos, pode utilizar as APIs de entrada de mensagens para executar transações num livro razão. Ver [exemplos de integração de mensagens](https://aka.ms/blockchain-workbench-integration-sample) para obter um exemplo que demonstra as APIs de entrada.

Seguem-se a APIs de entrada está disponível.

### <a name="create-user"></a>Criar utilizador

Cria um novo utilizador.

O pedido requer os seguintes campos:

| **Nome**             | **Descrição**                                      |
|----------------------|------------------------------------------------------|
| requestId            | GUID fornecido pelo cliente                                |
| firstName            | Nome próprio do utilizador                              |
| Apelido             | Apelido do utilizador                               |
| Endereço de correio eletrónico         | Endereço de e-mail do utilizador                           |
| externalId           | ID de objeto do Azure AD do utilizador                      |
| ConnectionId         | Identificador exclusivo para a ligação de blockchain |
| messageSchemaVersion | Versão do esquema de mensagens                            |
| messageName          | **CreateUserRequest**                               |

Exemplo:

``` json
{
    "requestId": "e2264523-6147-41fc-bbbb-edba8e44562d",
    "firstName": "Ali",
    "lastName": "Alio",
    "emailAddress": "aa@contoso.com",
    "externalId": "6a9b7f65-ffff-442f-b3b8-58a35abd1bcd",
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateUserRequest"
}
```

Blockchain Workbench devolve uma resposta com os seguintes campos:

| **Nome**              | **Descrição**                                                                                                             |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------|
| requestId             | GUID fornecido pelo cliente |
| userId                | ID do utilizador que foi criado |
| UserChainIdentifier   | Endereço do utilizador que foi criado na rede de blockchain. Em Ethereum, o endereço é o usuário **na cadeia** endereço. |
| ConnectionId          | Identificador exclusivo para a ligação de blockchain|
| messageSchemaVersion  | Versão do esquema de mensagens |
| messageName           | **CreateUserUpdate** |
| status                | Estado do pedido de criação do utilizador.  Se tiver êxito, o valor é **êxito**. Em caso de falha, o valor é **falha**.     |
| AdditionalInformation | Fornecidas informações adicionais com base no status |

Exemplo com êxito **criar utilizador** resposta do Blockchain Workbench:

``` json
{ 
    "requestId": "e2264523-6147-41fc-bb59-edba8e44562d", 
    "userId": 15, 
    "userChainIdentifier": "0x9a8DDaCa9B7488683A4d62d0817E965E8f248398", 
    "connectionId": 1, 
    "messageSchemaVersion": "1.0.0", 
    "messageName": "CreateUserUpdate", 
    "status": "Success", 
    "additionalInformation": { } 
} 
```

Se o pedido não teve êxito, os detalhes sobre a falha são incluem informações adicionais.

``` json
{
    "requestId": "e2264523-6147-41fc-bb59-edba8e44562d", 
    "userId": 15, 
    "userChainIdentifier": null, 
    "connectionId": 1, 
    "messageSchemaVersion": "1.0.0", 
    "messageName": "CreateUserUpdate", 
    "status": "Failure", 
    "additionalInformation": { 
        "errorCode": 4000, 
        "errorMessage": "User cannot be provisioned on connection." 
    }
}
```

### <a name="create-contract"></a>Criar o contrato

Cria um novo contrato.

O pedido requer os seguintes campos:

| **Nome**             | **Descrição**                                                                                                           |
|----------------------|---------------------------------------------------------------------------------------------------------------------------|
| requestId            | GUID fornecido pelo cliente |
| UserChainIdentifier  | Endereço do utilizador que foi criado na rede de blockchain. Em Ethereum, este endereço é o usuário **na cadeia** endereço. |
| ApplicationName      | Nome da aplicação |
| WorkflowName         | Nome do fluxo de trabalho |
| parâmetros           | Entrada de parâmetros para a criação do contrato |
| ConnectionId         | Identificador exclusivo para a ligação de blockchain |
| messageSchemaVersion | Versão do esquema de mensagens |
| messageName          | **CreateContractRequest** |

Exemplo:

``` json
{ 
    "requestId": "ce3c429b-a091-4baa-b29b-5b576162b211", 
    "userChainIdentifier": "0x9a8DDaCa9B7488683A4d62d0817E965E8f248398", 
    "applicationName": "AssetTransfer", 
    "workflowName": "AssetTransfer", 
    "parameters": [ 
        { 
            "name": "description", 
            "value": "a 1969 dodge charger" 
        }, 
        { 
            "name": "price", 
            "value": "12345" 
        } 
    ], 
    "connectionId": 1, 
    "messageSchemaVersion": "1.0.0", 
    "messageName": "CreateContractRequest" 
}
```

Blockchain Workbench devolve uma resposta com os seguintes campos:

| **Nome**                 | **Descrição**                                                                   |
|--------------------------|-----------------------------------------------------------------------------------|
| requestId                | GUID fornecido pelo cliente                                                             |
| ContractId               | Identificador exclusivo para o contrato de dentro do Azure Blockchain Workbench |
| ContractLedgerIdentifier | Endereço do contrato no razão                                            |
| ConnectionId             | Identificador exclusivo para a ligação de blockchain                               |
| messageSchemaVersion     | Versão do esquema de mensagens                                                         |
| messageName              | **CreateContractUpdate**                                                      |
| status                   | Estado do pedido de criação do contrato.  Valores possíveis: **submetida**, **consolidado**, **falha**.  |
| AdditionalInformation    | Fornecidas informações adicionais com base no status                              |

Exemplo de um submetido **criar contrato** resposta do Blockchain Workbench:

``` json
{
    "requestId": "ce3c429b-a091-4baa-b29b-5b576162b211",
    "contractId": 55,
    "contractLedgerIdentifier": "0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe",
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractUpdate",
    "status": "Submitted"
    "additionalInformation": { }
}
```

Exemplo de um compromisso **criar contrato** resposta do Blockchain Workbench:

``` json
{
    "requestId": "ce3c429b-a091-4baa-b29b-5b576162b211",
    "contractId": 55,
    "contractLedgerIdentifier": "0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe",
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractUpdate",
    "status": "Committed",
    "additionalInformation": { }
}
```

Se o pedido não teve êxito, os detalhes sobre a falha são incluem informações adicionais.

``` json
{
    "requestId": "ce3c429b-a091-4baa-b29b-5b576162b211",
    "contractId": 55,
    "contractLedgerIdentifier": null,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractUpdate",
    "status": "Failure"
    "additionalInformation": {
        "errorCode": 4000,
        "errorMessage": "Contract cannot be provisioned on connection."
    }
}
```

### <a name="create-contract-action"></a>Criar uma ação de contrato

Cria uma nova ação de contrato.

O pedido requer os seguintes campos:

| **Nome**                 | **Descrição**                                                                                                           |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------|
| requestId                | GUID fornecido pelo cliente |
| UserChainIdentifier      | Endereço do utilizador que foi criado na rede de blockchain. No Ethereum, este é o usuário **na cadeia** endereço. |
| ContractLedgerIdentifier | Endereço do contrato no razão |
| WorkflowFunctionName     | Nome da função de fluxo de trabalho |
| parâmetros               | Entrada de parâmetros para a criação do contrato |
| ConnectionId             | Identificador exclusivo para a ligação de blockchain |
| messageSchemaVersion     | Versão do esquema de mensagens |
| messageName              | **CreateContractActionRequest** |

Exemplo:

``` json
{
    "requestId": "a5530932-9d6b-4eed-8623-441a647741d3",
    "userChainIdentifier": "0x9a8DDaCa9B7488683A4d62d0817E965E8f248398",
    "contractLedgerIdentifier": "0xde0B295669a9FD93d5F28D9Ec85E40f4cb697BAe",
    "workflowFunctionName": "modify",
    "parameters": [
        {
            "name": "description",
            "value": "a 1969 dodge charger"
        },
        {
            "name": "price",
            "value": "12345"
        }
    ],
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractActionRequest"
}
```

Blockchain Workbench devolve uma resposta com os seguintes campos:

| **Nome**              | **Descrição**                                                                   |
|-----------------------|-----------------------------------------------------------------------------------|
| requestId             | GUID fornecido pelo cliente|
| ContractId            | Identificador exclusivo para o contrato de dentro do Azure Blockchain Workbench |
| ConnectionId          | Identificador exclusivo para a ligação de blockchain |
| messageSchemaVersion  | Versão do esquema de mensagens |
| messageName           | **CreateContractActionUpdate** |
| status                | Estado do pedido de ação de contrato. Valores possíveis: **submetida**, **consolidado**, **falha**.                         |
| AdditionalInformation | Fornecidas informações adicionais com base no status |

Exemplo de um submetido **criar uma ação de contrato** resposta do Blockchain Workbench:

``` json
{
    "requestId": "a5530932-9d6b-4eed-8623-441a647741d3",
    "contractId": 105,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractActionUpdate",
    "status": "Submitted",
    "additionalInformation": { }
}
```

Exemplo de um compromisso **criar uma ação de contrato** resposta do Blockchain Workbench:

``` json
{
    "requestId": "a5530932-9d6b-4eed-8623-441a647741d3",
    "contractId": 105,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractActionUpdate",
    "status": "Committed"
    "additionalInformation": { }
}
```

Se o pedido não teve êxito, os detalhes sobre a falha são incluem informações adicionais.

``` json
{
    "requestId": "a5530932-9d6b-4eed-8623-441a647741d3",
    "contractId": 105,
    "connectionId": 1,
    "messageSchemaVersion": "1.0.0",
    "messageName": "CreateContractActionUpdate",
    "status": "Failure"
    "additionalInformation": {
        "errorCode": 4000,
        "errorMessage": "Contract action cannot be provisioned on connection."
    }
}
```

### <a name="input-api-error-codes-and-messages"></a>Códigos de erro de API entrados e de mensagens

**O código de erro 4000: erro de pedido incorreto**
- ConnectionId inválido
- Falhada de desserialização de CreateUserRequest
- Falhada de desserialização de CreateContractRequest
- Falhada de desserialização de CreateContractActionRequest
- Aplicação {identificada pelo nome da aplicação} não existe
- Aplicação {identificada pelo nome da aplicação} não tem o fluxo de trabalho
- UserChainIdentifier não existe
- Contrato {identificado pelo identificador de contabilidade} não existe
- Contrato {identificado pelo identificador de contabilidade} não tem a função {nome da função de fluxo de trabalho}
- UserChainIdentifier não existe

**O código de erro 4090: erro de entrar em conflito**
- O utilizador já existe
- Contrato já existe
- Ação de contrato já existe

**5000 do código de erro: erro de servidor interno**
- Mensagens de exceção

## <a name="event-notifications"></a>Notificações de eventos

Notificações de eventos podem ser utilizadas para notificar os utilizadores e a jusante sistemas de eventos que ocorrem no Blockchain Workbench e a rede de blockchain que está ligado a. Notificações de eventos podem ser consumidas diretamente no código ou podem ser utilizadas com ferramentas como o Logic Apps e Flow para acionar o fluxo de dados para sistemas a jusante.

Ver [referência de mensagem de notificação](#notification-message-reference) para obter detalhes de várias mensagens que podem ser recebidas.

### <a name="consuming-event-grid-events-with-azure-functions"></a>Consumo de eventos do Event Grid com as funções do Azure

Se um utilizador quiser utilizar o Event Grid para ser notificado sobre eventos que ocorrem na bancada de trabalho de Blockchain, pode consumir eventos do Event Grid, utilizando as funções do Azure.

1. Criar uma **aplicação de funções do Azure** no portal do Azure.
2. Crie uma nova função.
3. Localize o modelo para o Event Grid. Código de modelo básico para ler a mensagem é mostrado. Modificar o código conforme necessário.
4. Guarde a função. 
5. Selecione o Event Grid do grupo de recursos de Blockchain Workbench.

### <a name="consuming-event-grid-events-with-logic-apps"></a>Consumo de eventos do Event Grid com o Logic Apps

1.  Criar uma nova **aplicação lógica do Azure** no portal do Azure.
2.  Ao abrir a aplicação de lógica do Azure no portal, será solicitado a selecionar um acionador. Selecione **Azure Event Grid – quando ocorre um evento de recurso**.
3. Quando o designer de fluxo de trabalho é exibido, será solicitado para iniciar sessão.
4. Selecione a subscrição. Recursos como **Microsoft.EventGrid.Topics**. Selecione o **nome do recurso** do nome do recurso do grupo de recursos do Azure Blockchain Workbench.
5. Selecione o Event Grid do grupo de recursos de Blockchain Workbench.

## <a name="using-service-bus-topics-for-notifications"></a>Utilizar tópicos do Service Bus para notificações

Tópicos do Service Bus pode ser utilizados para notificar os utilizadores sobre eventos que ocorrem na bancada de trabalho de Blockchain. 

1.  Navegue para o Service Bus dentro do grupo de recursos a Bancada de trabalho.
2.  Selecione **tópicos**.
3.  Selecione **Bancada de trabalho externo**.
4.  Crie uma nova subscrição a esse tópico. Obter uma chave para o mesmo.
5.  Crie um programa, que subscreve eventos desta subscrição.

### <a name="consuming-service-bus-messages-with-logic-apps"></a>Consumir mensagens do Service Bus com o Logic Apps

1. Criar uma nova **aplicação lógica do Azure** no portal do Azure.
2. Ao abrir a aplicação de lógica do Azure no portal, será solicitado a selecionar um acionador. Tipo **do Service Bus** na caixa de pesquisa e selecione o acionador apropriado para o tipo de interação que pretende ter com o Service Bus. Por exemplo, **do Service Bus - quando uma mensagem é recebida numa subscrição de tópico (concluir automaticamente)**.
3. Quando o designer de fluxo de trabalho é exibido, especifique as informações de ligação para o Service Bus.
4. Selecione a sua subscrição e especifique o tópico da **Bancada de trabalho externo**.
5. Desenvolva a lógica para a sua aplicação que utiliza a mensagem deste acionador.

## <a name="notification-message-reference"></a>Referência de mensagem de notificação

Consoante a **OperationName**, as mensagens de notificação de ter um dos seguintes tipos de mensagem.

### <a name="accountcreated"></a>AccountCreated

Indica que uma nova conta tem sido pedida para ser adicionado a cadeia especificada.

| Nome    | Descrição  |
|----------|--------------|
| UserId  | ID do utilizador que foi criado. |
| ChainIdentifier | Endereço do utilizador que foi criado na rede de blockchain. Ethereum, isso seria o usuário **na cadeia** endereço. |

``` csharp
public class NewAccountRequest : MessageModelBase
{
  public int UserID { get; set; }
  public string ChainIdentifier { get; set; }
}
```

### <a name="contractinsertedorupdated"></a>ContractInsertedOrUpdated

Indica que foi efetuada uma solicitação para inserir ou atualizar um contrato de um livro razão distribuído.

| Nome | Descrição |
|-----|--------------|
| ChainID | Identificador exclusivo para a cadeia associado à solicitação |
| BlockId | Identificador exclusivo para um bloco no razão |
| ContractId | Um identificador exclusivo para o contrato |
| ContractAddress |       O endereço do contrato no razão |
| TransactionHash  |     O hash da transação no razão |
| OriginatingAddress |   O endereço de origem da transação |
| actionName       |     O nome da ação |
| IsUpdate        |      Identifica se se tratar de uma atualização |
| Parâmetros       |     Uma lista de objetos que identificam o tipo de nome, valor e dados enviados para uma ação de parâmetros |
| TopLevelInputParams |  Em cenários em que um contrato está ligado a um ou mais contratos, estes são os parâmetros do contrato de nível superior. |

``` csharp
public class ContractInsertOrUpdateRequest : MessageModelBase
{
    public int ChainId { get; set; }
    public int BlockId { get; set; }
    public int ContractId { get; set; }
    public string ContractAddress { get; set; }
    public string TransactionHash { get; set; }
    public string OriginatingAddress { get; set; }
    public string ActionName { get; set; }
    public bool IsUpdate { get; set; }
    public List<ContractProperty> Parameters { get; set; }
    public bool IsTopLevelUpdate { get; set; }
    public List<ContractInputParameter> TopLevelInputParams { get; set; }
}
```

#### <a name="updatecontractaction"></a>UpdateContractAction

Indica que um pedido foi efetuado a execução de uma ação num contrato específico num livro razão distribuído.

| Nome                     | Descrição                                                                                                                                                                   |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ContractActionId         | Identificador exclusivo para esta ação de contrato |
| ChainIdentifier          | Identificador exclusivo para a cadeia |
| ConnectionId             | Identificador exclusivo para a ligação |
| UserChainIdentifier      | Endereço do utilizador que foi criado na rede de blockchain. Em Ethereum, este endereço é o usuário **na cadeia** endereço. |
| ContractLedgerIdentifier | Endereço do contrato no razão |
| WorkflowFunctionName     | Nome da função de fluxo de trabalho |
| WorkflowName             | Nome do fluxo de trabalho |
| WorkflowBlobStorageURL   | O url do contrato no armazenamento de BLOBs |
| ContractActionParameters | Parâmetros para a ação de contrato |
| TransactionHash          | O hash da transação no razão |
| Estado do Aprovisionamento      | O estado de aprovisionamento atual da ação.</br>0 – criado</br>1 – no processo</br>2 – concluir</br> Completo indica uma confirmação da razão que neste foram adicionadas com êxito |

```csharp
public class ContractActionRequest : MessageModelBase
{
    public int ContractActionId { get; set; }
    public int ConnectionId { get; set; }
    public string UserChainIdentifier { get; set; }
    public string ContractLedgerIdentifier { get; set; }
    public string WorkflowFunctionName { get; set; }
    public string WorkflowName { get; set; }
    public string WorkflowBlobStorageURL { get; set; }
    public IEnumerable<ContractActionParameter> ContractActionParameters { get; set; }
    public string TransactionHash { get; set; }
    public int ProvisioningStatus { get; set; }
}
```

### <a name="updateuserbalance"></a>UpdateUserBalance

Indica que foi efetuada uma solicitação para atualizar o saldo de usuário num livro razão distribuído específico.

> [!NOTE]
> Esta mensagem é gerada apenas para os livros razão que requerem os fundos de contas.
> 

| Nome    | Descrição                              |
|---------|------------------------------------------|
| Endereço | O endereço do utilizador que foi atendido |
| Saldo | O equilíbrio entre o saldo de utilizador         |
| ChainID | Identificador exclusivo para a cadeia     |


``` csharp
public class UpdateUserBalanceRequest : MessageModelBase
{
    public string Address { get; set; }
    public decimal Balance { get; set; }
    public int ChainID { get; set; }
}
```

### <a name="insertblock"></a>InsertBlock

Mensagem indica que foi efetuada uma solicitação para adicionar um bloco de um livro razão distribuído.

| Nome           | Descrição                                                            |
|----------------|------------------------------------------------------------------------|
| ChainId        | Identificador exclusivo da cadeia para o qual foi adicionado o bloco             |
| BlockId        | Identificador exclusivo para o bloco de dentro do Azure Blockchain Workbench |
| BlockHash      | O hash do bloco                                                 |
| BlockTimeStamp | O carimbo de hora do bloco                                            |

``` csharp
public class InsertBlockRequest : MessageModelBase
{
    public int ChainId { get; set; }
    public int BlockId { get; set; }
    public string BlockHash { get; set; }
    public int BlockTimestamp { get; set; }
}
```

### <a name="inserttransaction"></a>InsertTransaction

Mensagem fornece detalhes sobre um pedido para adicionar uma transação num livro razão distribuído.

| Nome            | Descrição                                                            |
|-----------------|------------------------------------------------------------------------|
| ChainId         | Identificador exclusivo da cadeia para o qual foi adicionado o bloco             |
| BlockId         | Identificador exclusivo para o bloco de dentro do Azure Blockchain Workbench |
| TransactionHash | O hash da transação                                           |
| De            | O endereço de origem da transação                      |
| Para              | O endereço do destinatário pretendido da transação              |
| Valor           | O valor incluído na transação                                 |
| IsAppBuilderTx  | Identifica se se tratar de uma transação de Blockchain Workbench                         |

``` csharp
public class InsertTransactionRequest : MessageModelBase
{
    public int ChainId { get; set; }
    public int BlockId { get; set; }
    public string TransactionHash { get; set; }
    public string From { get; set; }
    public string To { get; set; }
    public decimal Value { get; set; }
    public bool IsAppBuilderTx { get; set; }
}
```

### <a name="assigncontractchainidentifier"></a>AssignContractChainIdentifier

Fornece detalhes sobre a atribuição de um identificador de cadeia para um contrato. Por exemplo, no blockchain Ethereum, o endereço de um contrato no razão.

| Nome            | Descrição                                                                       |
|-----------------|-----------------------------------------------------------------------------------|
| ContractId      | Identificador exclusivo para o contrato de dentro do Azure Blockchain Workbench |
| ChainIdentifier | Identificador para o contrato da cadeia                             |

``` csharp
public class AssignContractChainIdentifierRequest : MessageModelBase
{
    public int ContractId { get; set; }
    public string ChainIdentifier { get; set; }
}
```

## <a name="classes-used-by-message-types"></a>Classes usadas por tipos de mensagem

### <a name="messagemodelbase"></a>MessageModelBase

O modelo de base para todas as mensagens.

| Nome          | Descrição                          |
|---------------|--------------------------------------|
| OperationName | O nome da operação           |
| requestId     | Identificador exclusivo para o pedido |

``` csharp
public class MessageModelBase
{
    public string OperationName { get; set; }
    public string RequestId { get; set; }
}
```

### <a name="contractinputparameter"></a>ContractInputParameter

Contém o nome, o valor e o tipo de um parâmetro.

| Nome  | Descrição                 |
|-------|-----------------------------|
| Nome  | O nome do parâmetro  |
| Valor | O valor do parâmetro |
| Tipo  | O tipo do parâmetro  |

``` csharp
public class ContractInputParameter
{
    public string Name { get; set; }
    public string Value { get; set; }
    public string Type { get; set; }
}
```

#### <a name="contractproperty"></a>ContractProperty

Contém o ID, nome, valor e tipo de uma propriedade.

| Nome  | Descrição                |
|-------|----------------------------|
| Id    | O ID da propriedade    |
| Nome  | O nome da propriedade  |
| Valor | O valor da propriedade |
| Tipo  | O tipo da propriedade  |

``` csharp
public class ContractProperty
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Value { get; set; }
    public string DataType { get; set; }
}
```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Padrões de integração do contrato inteligente](integration-patterns.md)