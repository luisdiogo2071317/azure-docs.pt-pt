---
title: Descrição geral da integração de mensagens do Azure Blockchain Workbench
description: Descrição geral da utilização de mensagens no Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 10/1/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: dcf31bd6b128115962e30dd6653364c4cd65047d
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/03/2018
ms.locfileid: "48242880"
---
# <a name="azure-blockchain-workbench-messaging-integration"></a>O Azure Blockchain Workbench, integração de mensagens

Além de fornecer uma API REST, o Azure Blockchain Workbench também fornece integração baseados em mensagens. Workbench publica eventos centrada em razão através do Azure Event Grid, permitindo que os consumidores de downstream para ingestão de dados ou tomar medidas com base nestes eventos. Para os clientes que necessitam de sistema de mensagens confiável, o Azure Blockchain Workbench entregam mensagens para um ponto final do Azure Service Bus também.

Os desenvolvedores também tem expressou interesse na capacidade de ter sistemas externos comunicar iniciar transações para criar utilizadores, criar contratos e atualizar contratos num livro razão. Enquanto esta funcionalidade não está exposta atualmente em pré-visualização pública, uma amostra que fornece essa funcionalidade pode ser encontrada em [ http://aka.ms/blockchain-workbench-integration-sample ](http://aka.ms/blockchain-workbench-integration-sample).

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
| ChainID | Um identificador exclusivo para a cadeia associado à solicitação.|
| BlockId | O identificador exclusivo para um bloco no razão.|
| ContractId | Um identificador exclusivo para o contrato.|
| ContractAddress |       O endereço do contrato no razão.|
| TransactionHash  |     O hash da transação no razão.|
| OriginatingAddress |   O endereço de origem da transação.|
| actionName       |     O nome da ação.|
| IsUpdate        |      Identifica se se tratar de uma atualização.|
| Parâmetros       |     Uma lista de objetos que identificam o tipo de nome, valor e dados enviados para uma ação de parâmetros.|
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
| ContractActionId         | O identificador exclusivo para esta ação de contrato                                                                                                                                |
| ChainIdentifier          | O identificador exclusivo para a cadeia                                                                                                                                           |
| ConnectionId             | O identificador exclusivo para a ligação                                                                                                                                      |
| UserChainIdentifier      | Endereço do utilizador que foi criado na rede de blockchain. Ethereum, isso seria o endereço do utilizador "na cadeia".                                                     |
| ContractLedgerIdentifier | Endereço do contrato no razão.                                                                                                                                        |
| WorkflowFunctionName     | Nome da função de fluxo de trabalho.                                                                                                                                                |
| WorkflowName             | Nome do fluxo de trabalho.                                                                                                                                                         |
| WorkflowBlobStorageURL   | O url do contrato no armazenamento de Blobs.                                                                                                                                      |
| ContractActionParameters | Parâmetros para a ação de contrato.                                                                                                                                           |
| TransactionHash          | O hash da transação no razão.                                                                                                                                    |
| Estado do Aprovisionamento      | O estado de aprovisionamento atual da ação.</br>0 – criado</br>1 – no processo</br>2 – concluir</br> Completo indica uma confirmação da razão que neste foram adicionadas com êxito.                                               |
|                          |                                                                                                                                                                               |

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
| Endereço | O endereço do utilizador que foi atendido. |
| Saldo | O equilíbrio entre o saldo de utilizador.         |
| ChainID | O identificador exclusivo para a cadeia.     |


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
| ChainId        | O identificador exclusivo da cadeia para o qual foi adicionado o bloco de mensagens em fila.             |
| BlockId        | O identificador exclusivo para o bloco de dentro do Azure Blockchain Workbench. |
| BlockHash      | O hash do bloco.                                                 |
| BlockTimeStamp | O carimbo de hora do bloco.                                            |

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
| ChainId         | O identificador exclusivo da cadeia para o qual foi adicionado o bloco de mensagens em fila.             |
| BlockId         | O identificador exclusivo para o bloco de dentro do Azure Blockchain Workbench. |
| TransactionHash | O hash da transação.                                           |
| De            | O endereço de origem da transação.                      |
| Para              | O endereço do destinatário da transação.              |
| Valor           | O valor incluído na transação.                                 |
| IsAppBuilderTx  | Identifica se se tratar de uma transação de Blockchain Workbench.                         |

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
| ContractId      | Este é o identificador exclusivo para o contrato de dentro do Azure Blockchain Workbench. |
| ChainIdentifier | Este é o identificador para o contrato da cadeia.                             |

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
| OperationName | O nome da operação.           |
| requestId     | Um identificador exclusivo para o pedido. |

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
| Nome  | O nome do parâmetro.  |
| Valor | O valor do parâmetro. |
| Tipo  | O tipo do parâmetro.  |

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
| Id    | O ID da propriedade.    |
| Nome  | O nome da propriedade.  |
| Valor | O valor da propriedade. |
| Tipo  | O tipo da propriedade.  |

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