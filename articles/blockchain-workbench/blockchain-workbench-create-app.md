---
title: Criar uma aplicação de blockchain no Azure Blockchain Workbench
description: Como criar uma aplicação blockchain no Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/17/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: a4b704f433f02afcff7b94f98c19a478caaa02b2
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/06/2018
ms.locfileid: "34808065"
---
# <a name="create-a-blockchain-application-in-azure-blockchain-workbench"></a>Criar uma aplicação de blockchain no Azure Blockchain Workbench

Pode utilizar o Azure Blockchain Workbench para criar aplicações blockchain que representam os fluxos de trabalho de múltiplos intervenientes definidos pela configuração e o código de contrato inteligente.

Saiba como:

> [!div class="checklist"]
> * Configurar uma aplicação blockchain
> * Criar um ficheiro de código de contrato inteligente
> * Adicionar uma aplicação blockchain Blockchain Workbench
> * Adicionar membros à aplicação blockchain

## <a name="prerequisites"></a>Pré-requisitos

* Uma implementação de Blockchain Workbench. Para obter mais informações, consulte [implementação do Azure Blockchain Workbench](blockchain-workbench-deploy.md) para obter detalhes sobre a implementação.
* Utilizadores do Active Directory do Azure no inquilino associado aos Blockchain Workbench. Para obter mais informações, consulte [adicionar utilizadores do Azure AD no Azure Blockchain Workbench](blockchain-workbench-manage-users.md#add-azure-ad-users).
* Uma conta de administrador Blockchain Workbench. Para obter mais informações, consulte Adicionar [administradores Blockchain Workbench no Azure Blockchain Workbench](blockchain-workbench-manage-users.md#manage-blockchain-workbench-administrators).

## <a name="hello-blockchain"></a>Olá, Blockchain!

Vamos criar uma aplicação básica no qual um requerente envia um pedido e um dispositivo de resposta enviar uma resposta ao pedido. Por exemplo, pode ser um pedido, "Olá, como está?" e pode a resposta ser, "Estou excelente!". O pedido e resposta são registadas no ficheiro de blockchain subjacente. 

Siga os passos para criar os ficheiros de aplicação ou pode [transferir o exemplo a partir do GitHub](https://github.com/Azure-Samples/blockchain/tree/master/blockchain-workbench/application-and-smart-contract-samples/hello-blockchain). 

## <a name="configuration-file"></a>Ficheiro de configuração

Metadados de configuração definem os fluxos de trabalho de alto nível e o modelo de interação da aplicação blockchain. Metadados de configuração representam as fases de fluxo de trabalho e o modelo de interação da aplicação blockchain.

1. No seu editor favorito, crie um ficheiro denominado `HelloBlockchain.json`.
2. Adicione o seguinte JSON para definir a configuração da aplicação blockchain.

    ``` json
    {
      "ApplicationName": "HelloBlockchain",
      "DisplayName": "Hello, Blockchain!",
      "Description": "A simple application to send request and get response",
      "ApplicationRoles": [
        {
          "Name": "Requestor",
          "Description": "A person sending a request."
        },
        {
          "Name": "Responder",
          "Description": "A person responding to a request"
        }
      ],
      "Workflows": [
        {
          "Name": "HelloBlockchain",
          "DisplayName": "Request Response",
          "Description": "A simple workflow to send a request and receive a response.",
          "Initiators": [ "Requestor" ],
          "StartState": "Request",
          "Properties": [
            {
              "Name": "State",
              "DisplayName": "State",
              "Description": "Holds the state of the contract.",
              "Type": {
                "Name": "state"
              }
            },
            {
              "Name": "Requestor",
              "DisplayName": "Requestor",
              "Description": "A person sending a request.",
              "Type": {
                "Name": "Requestor"
              }
            },
            {
              "Name": "Responder",
              "DisplayName": "Responder",
              "Description": "A person sending a response.",
              "Type": {
                "Name": "Responder"
              }
            },
            {
              "Name": "RequestMessage",
              "DisplayName": "Request Message",
              "Description": "A request message.",
              "Type": {
                "Name": "string"
              }
            },
            {
              "Name": "ResponseMessage",
              "DisplayName": "Response Message",
              "Description": "A response message.",
              "Type": {
                "Name": "string"
              }
            }
          ],
          "Constructor": {
            "Parameters": [
              {
                "Name": "message",
                "Description": "...",
                "DisplayName": "Request Message",
                "Type": {
                  "Name": "string"
                }
              }
            ]
          },
          "Functions": [
            {
              "Name": "SendRequest",
              "DisplayName": "Request",
              "Description": "...",
              "Parameters": [
                {
                  "Name": "requestMessage",
                  "Description": "...",
                  "DisplayName": "Request Message",
                  "Type": {
                    "Name": "string"
                  }
                }
              ]
            },
            {
              "Name": "SendResponse",
              "DisplayName": "Response",
              "Description": "...",
              "Parameters": [
                {
                  "Name": "responseMessage",
                  "Description": "...",
                  "DisplayName": "Response Message",
                  "Type": {
                    "Name": "string"
                  }
                }
              ]
            }
          ],
          "States": [
            {
              "Name": "Request",
              "DisplayName": "Request",
              "Description": "...",
              "PercentComplete": 50,
              "Value": 0,
              "Style": "Success",
              "Transitions": [
                {
                  "AllowedRoles": ["Responder"],
                  "AllowedInstanceRoles": [],
                  "Description": "...",
                  "Function": "SendResponse",
                  "NextStates": [ "Respond" ],
                  "DisplayName": "Send Response"
                }
              ]
            },
            {
              "Name": "Respond",
              "DisplayName": "Respond",
              "Description": "...",
              "PercentComplete": 90,
              "Value": 1,
              "Style": "Success",
              "Transitions": [
                {
                  "AllowedRoles": [],
                  "AllowedInstanceRoles": ["Requestor"],
                  "Description": "...",
                  "Function": "SendRequest",
                  "NextStates": [ "Request" ],
                  "DisplayName": "Send Request"
                }
              ]
            }
          ]
        }
      ]
    }
    ```

3. Guarde o ficheiro `HelloBlockchain.json`.

O ficheiro de configuração tem várias secções. Detalhes sobre cada secção são os seguintes:

### <a name="application-metadata"></a>Metadados da aplicação

O início do ficheiro de configuração contém informações sobre a aplicação, incluindo o nome da aplicação e uma descrição.

### <a name="application-roles"></a>Funções da aplicação

A secção de funções de aplicação define as funções de utilizador a quem podem agir ou participar na aplicação blockchain. Definir um conjunto de funções distintas com base na funcionalidade. No cenário de resposta-pedido, há uma distinção entre a funcionalidade de um requerente como uma entidade que produz pedidos e um dispositivo de resposta como uma entidade que produz respostas.

### <a name="workflows"></a>Fluxos de trabalho

Fluxos de trabalho definem um ou mais fases e ações do contrato. No cenário de resposta-pedido, a primeira fase (estado) do fluxo de trabalho é que um requerente (função) executa uma ação (transição) para enviar um pedido (função). A próxima fase (estado) é que um dispositivo de resposta (função) executa uma ação (transição) para enviar uma resposta (função). Fluxo de trabalho de uma aplicação pode envolver propriedades, funções, e Estados necessários descrevem o fluxo de um contrato. 

Para obter mais informações sobre o conteúdo dos ficheiros de configuração, consulte [referência da configuração do fluxo de trabalho do Azure Blockchain](blockchain-workbench-configuration-overview.md).

## <a name="smart-contract-code-file"></a>Ficheiro de código de contrato inteligente

Contratos inteligentes representam a lógica de negócio da aplicação blockchain. Atualmente, Blockchain Workbench suporta Ethereum para o blockchain ledger. Utiliza Ethereum [Solidity](https://solidity.readthedocs.io) como a linguagem de programação de escrita de lógica de negócio Self-imposição para contratos inteligentes.

Contratos inteligentes Solidity são semelhantes às classes em idiomas orientado para objetos. Cada contrato contém o estado e as funções para implementar fases e ações do contrato inteligente.

No seu editor favorito, crie um ficheiro chamado `HelloBlockchain.sol`.

### <a name="version-pragma"></a>Pragma de versão

Como melhor prática, indique a versão do Solidity está a filtrar. Especificar a versão ajuda a evitar a incompatibilidades nas versões futuras do Solidity. 

Adicione o seguinte pragma de versão no topo da `HelloBlockchain.sol` ficheiro de código de contrato inteligente.


  ``` solidity
  pragma solidity ^0.4.20;
  ```

### <a name="base-class"></a>Classe base

**WorkbenchBase** classe base permite Blockchain Workbench criar e atualizar o contrato. A classe base é necessária para o código de contrato inteligente específico Blockchain Workbench. O contrato tem de herdar o **WorkbenchBase** classe base.

No `HelloBlockchain.sol` smart o ficheiro de código do contrato, adicione o **WorkbenchBase** classe no início do ficheiro. 

```
contract WorkbenchBase {
    event WorkbenchContractCreated(string applicationName, string workflowName, address originatingAddress);
    event WorkbenchContractUpdated(string applicationName, string workflowName, string action, address originatingAddress);

    string internal ApplicationName;
    string internal WorkflowName;

    function WorkbenchBase(string applicationName, string workflowName) internal {
        ApplicationName = applicationName;
        WorkflowName = workflowName;
    }

    function ContractCreated() internal {
        WorkbenchContractCreated(ApplicationName, WorkflowName, msg.sender);
    }

    function ContractUpdated(string action) internal {
        WorkbenchContractUpdated(ApplicationName, WorkflowName, action, msg.sender);
    }
}
```
A classe base inclui duas funções importantes:

|Função de classe base  | Objetivo  | Quando chamar  |
|---------|---------|---------|
| ContractCreated() | Notifica Blockchain Workbench foi criado um contrato | Antes de sair o construtor de contrato |
| ContractUpdated() | Notifica Blockchain Workbench um Estado de contrato tiver sido atualizado | Antes de sair de uma função de contrato |

### <a name="configuration-and-smart-contract-code-relationship"></a>Configuração e a relação de código de contrato inteligente

Blockchain Workbench utiliza o ficheiro de configuração e o ficheiro de código de contrato inteligente para criar uma aplicação blockchain. Há uma relação entre o que está definido na configuração e o código no contrato inteligente. Detalhes do contrato, funções, parâmetros e tipos têm de corresponder ao criar a aplicação. Blockchain Workbench verifica os ficheiros antes da criação da aplicação. 

### <a name="contract"></a>Contrato

Para Blockchain Workbench, contratos tem de herdar o **WorkbenchBase** classe base. Quando declarar o contrato, tem de transmitir o nome da aplicação e o nome do fluxo de trabalho como argumentos.

Adicionar o **contrato** cabeçalho para o `HelloBlockchain.sol` ficheiro de código de contrato inteligente. 

```
contract HelloBlockchain is WorkbenchBase('HelloBlockchain', 'HelloBlockchain') {
```

O contrato tem de herdar o **WorkbenchBase** classe base e transmita os parâmetros **ApplicationName** e o fluxo de trabalho **nome** conforme definido na configuração ficheiro. Neste caso, o nome da aplicação e o nome de fluxo de trabalho são os mesmos.

### <a name="state-variables"></a>Variáveis de estado

Variáveis de estado armazenam os valores do Estado para cada instância de contrato. As variáveis de estado no seu contrato tem de corresponder às propriedades de fluxo de trabalho definidas no ficheiro de configuração.

Adicione as variáveis de estado para o contrato no seu `HelloBlockchain.sol` ficheiro de código de contrato inteligente. 

```
    //Set of States
    enum StateType { Request, Respond}
    
    //List of properties
    StateType public  State;
    address public  Requestor;
    address public  Responder;
    
    string public RequestMessage;
    string public ResponseMessage;
```

### <a name="constructor"></a>Construtor

O construtor define os parâmetros de entrada para uma nova instância de contrato inteligente do fluxo de trabalho. O construtor está declarado como uma função com o mesmo nome que o contrato. Os parâmetros necessários para o construtor são definidos como parâmetros do construtor no ficheiro de configuração. O número, a ordem e o tipo de parâmetros têm de corresponder em ambos os ficheiros.

Na função construtor, escreva qualquer lógica empresarial que pretende executar antes de criar o contrato. Por exemplo, inicializar as variáveis de estado com valores de iniciar.

Antes de sair do construtor de função, chame o `ContractCreated()` função. Esta função notifica Blockchain Workbench foi criado um contrato.

Adicione a função do construtor para o contrato no seu `HelloBlockchain.sol` ficheiro de código de contrato inteligente. 

```
    // constructor function
    function HelloBlockchain(string message) public
    {
        Requestor = msg.sender;
        RequestMessage = message;
        State = StateType.Request;
    
        // call ContractCreated() to create an instance of this workflow
        ContractCreated();
    }
```

### <a name="functions"></a>Funções

As funções são as unidades executável de lógica de negócio dentro de um contrato. Os parâmetros necessários para a função são definidos como parâmetros de função no ficheiro de configuração. O número, a ordem e o tipo de parâmetros têm de corresponder em ambos os ficheiros. As funções associadas a transições num fluxo de trabalho Blockchain Workbench no ficheiro de configuração. Uma transição é uma ação executada para mover para a próxima fase de fluxo de trabalho de uma aplicação conforme determinado pelo contrato.

Escreva qualquer lógica empresarial que pretende executar na função. Por exemplo, modificar o valor da variável de estado.

Antes de sair a função, chame o `ContractUpdated()` função. A função notifica Blockchain Workbench contrato estado foi atualizado. Se pretende anular as alterações de estado na função, chame revert(). Reverter as alterações efetuadas desde a última chamada para ContractUpdated() de estado de rejeições.

1. Adicione as seguintes funções para o contrato no seu `HelloBlockchain.sol` ficheiro de código de contrato inteligente. 

    ```
        // call this function to send a request
        function SendRequest(string requestMessage) public
        {
            if (Requestor != msg.sender)
            {
                revert();
            }
    
            RequestMessage = requestMessage;
            State = StateType.Request;
    
            // call ContractUpdated() to record this action
            ContractUpdated('SendRequest');
        }
    
        // call this function to send a response
        function SendResponse(string responseMessage) public
        {
            Responder = msg.sender;
    
            // call ContractUpdated() to record this action
            ResponseMessage = responseMessage;
            State = StateType.Respond;
            ContractUpdated('SendResponse');
        }
    }
    ```

2. Guardar o `HelloBlockchain.sol` ficheiro de código de contrato inteligente.

## <a name="add-blockchain-application-to-blockchain-workbench"></a>Adicionar aplicação blockchain Blockchain Workbench

Para adicionar uma aplicação blockchain Blockchain Workbench, carregar a configuração e os ficheiros de contrato inteligente para definir a aplicação.

1. Num browser, navegue para o endereço de web Blockchain Workbench. Por exemplo, `https://{workbench URL}.azurewebsites.net/` a aplicação web é criada quando implementar Blockchain Workbench. Para obter informações sobre como localizar o Blockchain Workbench endereço web, consulte [Blockchain Workbench URL da Web](blockchain-workbench-deploy.md#blockchain-workbench-web-url)
2. Inicie sessão como um [administrador Blockchain Workbench](blockchain-workbench-manage-users.md#manage-blockchain-workbench-administrators).
3. Selecione **aplicações** > **novo**. O **nova aplicação** é apresentado o painel.
4. Selecione **carregar a configuração de contrato** > **procurar** para localizar o **HelloBlockchain.json** ficheiro de configuração que criou. O ficheiro de configuração é validado automaticamente. Selecione o **mostrar** ligação para ver os erros de validação. Corrija os erros de validação antes de implementar a aplicação.
5. Selecione **carregar o código de contrato** > **procurar** para localizar o **HelloBlockchain.sol** ficheiro de código de contrato inteligente. O ficheiro de código é validado automaticamente. Selecione o **mostrar** ligação para ver os erros de validação. Corrija os erros de validação antes de implementar a aplicação.
6. Selecione **implementar** ao criar a aplicação de blockchain com base na configuração e ficheiros contrato inteligente.

Implementação da aplicação blockchain demora alguns minutos. Quando a implementação estiver concluída, a nova aplicação é apresentada no **aplicações**. 

> [!NOTE]
> Também pode criar aplicações blockchain utilizando o [API REST da Azure Blockchain Workbench](https://docs.microsoft.com/rest/api/azure-blockchain-workbench). 

## <a name="add-blockchain-application-members"></a>Adicionar blockchain membros de aplicação

Adicione membros de aplicação a sua aplicação para iniciar e executar ações contratos. Para adicionar membros de aplicação, tem de ser um [administrador Blockchain Workbench](blockchain-workbench-manage-users.md#manage-blockchain-workbench-administrators).

1. Selecione **aplicações** > **Olá, Blockchain!**.
2. O número de membros associados à aplicação é apresentado no canto superior direito da página. Para uma nova aplicação, o número de membros vai ser zero.
3. Selecione o **membros** ligação no canto superior direito da página. É apresentada uma lista de membros para a aplicação atual.
4. Na lista de associação, selecione **adicionar membros**.
5. Selecione ou introduza o nome do membro que pretende adicionar. Apenas os utilizadores do AD do Azure que existe no inquilino do Blockchain Workbench estão listados. Se o utilizador não for encontrado, terá de [adicionar utilizadores do Azure AD](blockchain-workbench-manage-users.md#add-azure-ad-users).
6. Selecione o **função** para o membro. Para o primeiro membro, selecione **requerente** como a função.
7. Selecione **adicionar** para adicionar o membro com a função associada à aplicação.
8. Adicionar outro membro para a aplicação com o **dispositivo de resposta** função.

Para obter mais informações sobre a gestão de utilizadores no Blockchain Workbench, consulte [gestão de utilizadores no Azure Blockchain Workbench](blockchain-workbench-manage-users.md)

## <a name="next-steps"></a>Passos Seguintes

Neste artigo procedimentos, criou uma aplicação básica de pedido e resposta. Para saber como utilizar a aplicação, avance para o seguinte artigo procedimentos.

> [!div class="nextstepaction"]
> [Utilizar uma aplicação blockchain](blockchain-workbench-use.md)
