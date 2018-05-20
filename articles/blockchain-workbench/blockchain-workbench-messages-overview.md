---
title: Descrição geral de mensagens do Azure Blockchain Workbench
description: Descrição geral da utilização de mensagens no Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 4/16/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: 4a2e85cc619d17745be9d8f72af5f99049ce7c6b
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="azure-blockchain-workbench-messages-overview"></a>Descrição geral de mensagens do Azure Blockchain Workbench

Além de fornecer uma API REST, Azure Blockchain Workbench também fornece mensagens com base na integração. Workbench publica eventos ledger centrada através de grelha de eventos do Azure, permitindo a jusante consumidores para ingestão de dados ou tome medidas com base nestes eventos. Para os clientes que necessitam de mensagens fiáveis, Azure Blockchain Workbench entrega mensagens para um ponto de final do Service Bus do Azure, bem como.

Os programadores também tem expresso interesse na capacidade de ter os sistemas externos comunicar iniciar transações para criar utilizadores, criar contratos e atualizar contratos numa ledger. Enquanto esta funcionalidade não está exposta atualmente em pré-visualização pública, uma amostra que fornece a capacidade de que pode ser encontrada em [ http://aka.ms/blockchain-workbench-integration-sample ](http://aka.ms/blockchain-workbench-integration-sample).


## <a name="event-notifications"></a>Notificações de eventos

As notificações de eventos podem ser utilizadas para notificar os utilizadores e sistemas a jusante dos eventos que ocorrem no Workbench e a rede de blockchain que está ligado a. As notificações de eventos podem ser consumidas diretamente no código ou utilizadas com ferramentas como Logic Apps e fluxo para acionar o fluxo de dados para sistemas a jusante.

Consulte [referência de mensagens de notificação](#notification-message-reference) para obter detalhes sobre várias mensagens que podem ser recebidas.

### <a name="consuming-event-grid-events-with-azure-functions"></a>Consumir eventos de grelha de eventos com as funções do Azure

Se um utilizador pretende utilizar grelha de evento para ser notificado sobre eventos que ocorrem no Blockchain Workbench, pode consumir eventos a partir de grelha de eventos, utilizando as funções do Azure.

1. Criar um **aplicação de função do Azure** no portal do Azure.
2. Crie uma nova função.
3. Localize o modelo de evento de grelha. Código do modelo básica para ler a mensagem é apresentado. Modificar o código conforme necessário.
4. Guarde a função. 
5. Selecione a grelha de eventos do grupo de recursos do Blockchain Workbench.

### <a name="consuming-event-grid-events-with-logic-apps"></a>Consumir eventos de grelha de eventos com as Logic Apps

1.  Crie um novo **aplicação de lógica de Azure** no portal do Azure.
2.  Ao abrir a aplicação de lógica do Azure no portal, será solicitado para selecionar um acionador. Selecione **grelha de eventos do Azure – quando ocorre um evento de recurso**.
3. Quando for apresentado o estruturador de fluxo de trabalho, será solicitado para iniciar sessão.
4. Selecione a subscrição. Recursos como **Microsoft.EventGrid.Topics**. Selecione o **nome do recurso** do nome do recurso do grupo de recursos do Azure Blockchain Workbench.
5. Selecione a grelha de eventos do grupo de recursos do Blockchain Workbench.

## <a name="using-service-bus-topics-for-notifications"></a>Utilizar tópicos de Service Bus para notificações

Tópicos de Service Bus pode ser utilizados para notificar os utilizadores sobre eventos que ocorrem no Blockchain Workbench. 

1.  Navegue para o Service Bus dentro do grupo de recursos do Workbench.
2.  Selecione **tópicos**.
3.  Selecione **workbench externo**.
4.  Crie uma nova subscrição para este tópico. Obter uma chave para o mesmo.
5.  Crie um programa, o que subscreve eventos desta subscrição.

### <a name="consuming-service-bus-messages-with-logic-apps"></a>Consumir mensagens de barramento de serviço com Logic Apps

1. Crie um novo **aplicação de lógica de Azure** no portal do Azure.
2.  Ao abrir a aplicação de lógica do Azure no portal, será solicitado para selecionar um acionador. Tipo **Service Bus** para a caixa de pesquisa e selecione o acionador adequado para o tipo de interação que pretende ter com o Service Bus. Por exemplo, **barramento de serviço – quando é recebida uma mensagem numa subscrição tópico (a conclusão automática)**.
3. Quando for apresentado o estruturador de fluxo de trabalho, especifique as informações de ligação para o Service Bus.
4. Selecione a sua subscrição e especifique o tópico **workbench externo**.
5. Desenvolva a lógica da aplicação que utiliza a mensagem deste acionador.

## <a name="notification-message-reference"></a>Referência de mensagens de notificação

Consoante o OperationName, as mensagens de notificação têm um dos seguintes tipos de mensagem.

### <a name="accountcreated"></a>AccountCreated

Indica que foi solicitada uma nova conta para ser adicionado a cadeia especificada.

| Nome    | Descrição  |
|----------|--------------|
| UserId  | ID do utilizador que foi criado |
| ChainIdentifier | Endereço do utilizador que foi criado na rede blockchain. Ethereum, isto seria o endereço do utilizador "na cadeia". |

``` csharp
public class NewAccountRequest : MessageModelBase
{
  public int UserID { get; set; }
  public string ChainIdentifier { get; set; }
}
```

### <a name="contractinsertedorupdated"></a>ContractInsertedOrUpdated

Indica que foi efetuado um pedido inserir ou atualizar um contrato de um ledger distribuída.

| Nome | Descrição |
|-----|--------------|
| ChainID | Um identificador exclusivo para a cadeia associado ao pedido.|
  BlockId | O identificador exclusivo de um bloco de ledger.|
  ContractId | Um identificador exclusivo para o contrato.|
  ContractAddress |       O endereço do contrato de ledger.|
  TransactionHash  |     O hash da transação de ledger.|
  OriginatingAddress |   O endereço do originador da transação.|
  actionName       |     O nome da ação.|
  IsUpdate        |      Identifica se se tratar de uma atualização.|
  Parâmetros       |     Uma lista de objetos que identifique o tipo de nome, valor e dados dos parâmetros enviados para uma ação.|
  TopLevelInputParams |  Em cenários onde um contrato está ligado a um ou mais contratos, estes são os parâmetros do contrato de nível superior. |

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

Indica que um pedido não foi efetuado a execução de uma ação de um contrato específico num ledger distribuída.

| Nome                     | Descrição                                                                                                                                                                   |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ContractActionId         | O identificador exclusivo para esta ação de contrato                                                                                                                                |
| ChainIdentifier          | O identificador exclusivo para a cadeia                                                                                                                                           |
| ConnectionId             | O identificador exclusivo para a ligação                                                                                                                                      |
| UserChainIdentifier      | Endereço do utilizador que foi criado na rede blockchain. Ethereum, isto seria o endereço do utilizador "na cadeia".                                                     |
| ContractLedgerIdentifier | Endereço do contrato de ledger.                                                                                                                                        |
| WorkflowFunctionName     | Nome da função de fluxo de trabalho.                                                                                                                                                |
| WorkflowName             | Nome do fluxo de trabalho.                                                                                                                                                         |
| WorkflowBlobStorageURL   | O url do contrato no blob storage.                                                                                                                                      |
| ContractActionParameters | Parâmetros para a ação de contrato.                                                                                                                                           |
| TransactionHash          | O hash da transação de ledger.                                                                                                                                    |
| Estado do Aprovisionamento      | O estado atual do aprovisionamento da ação.</br>0 – criado</br>1 – no processo</br>2 – concluir</br> Completa indica uma confirmação de ledger que este foi adicionado com sucesso.                                               |
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

Indica que foi efetuado um pedido para atualizar o equilíbrio de utilizador numa ledger distribuída específica.

> [!NOTE]
> Esta mensagem é gerada apenas para essas ledgers que requerem o financiamento de contas.
> 

| Nome    | Descrição                              |
|---------|------------------------------------------|
| Endereço | O endereço do utilizador que foi financiado. |
| Equilíbrio | O equilíbrio do equilíbrio de utilizador.         |
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

Mensagem indica que foi efetuado um pedido para adicionar um bloco de um ledger distribuída.

| Nome           | Descrição                                                            |
|----------------|------------------------------------------------------------------------|
| ChainId        | O identificador exclusivo da cadeia para o qual foi adicionado o bloco de mensagens em fila.             |
| BlockId        | O identificador exclusivo para o bloco dentro do Azure Blockchain Workbench. |
| BlockHash      | O hash do bloco.                                                 |
| BlockTimeStamp | O carimbo do bloco.                                            |

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

Mensagem fornece detalhes sobre um pedido para adicionar uma transação de um ledger distribuída.

| Nome            | Descrição                                                            |
|-----------------|------------------------------------------------------------------------|
| ChainId         | O identificador exclusivo da cadeia para o qual foi adicionado o bloco de mensagens em fila.             |
| BlockId         | O identificador exclusivo para o bloco dentro do Azure Blockchain Workbench. |
| TransactionHash | O hash da transação.                                           |
| De            | O endereço do originador da transação.                      |
| Para              | O endereço do destinatário pretendido da transação.              |
| Valor           | O valor incluído na transação.                                 |
| IsAppBuilderTx  | Identifica se se tratar de uma transação Blockchain Workbench.                         |

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

Fornece detalhes sobre a atribuição de um identificador de cadeia para um contrato. Por exemplo, no Ethereum blockchain, o endereço de um contrato de ledger.

| Nome            | Descrição                                                                       |
|-----------------|-----------------------------------------------------------------------------------|
| ContractId      | Este é o identificador exclusivo para o contrato no interior do Azure Blockchain Workbench. |
| ChainIdentifier | Este é o identificador para o contrato na cadeia.                             |

``` csharp
public class AssignContractChainIdentifierRequest : MessageModelBase
{
    public int ContractId { get; set; }
    public string ChainIdentifier { get; set; }
}
```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Padrões de integração do contrato inteligente](blockchain-workbench-integration-patterns.md)