---
title: Simular uma falha ao aceder ao armazenamento redundante com acesso de leitura no Azure | Microsoft Docs
description: Simular um erro ao aceder ao armazenamento georedundante com acesso de leitura
services: storage
author: ruthogunnnaike
manager: jeconnoc
ms.service: storage
ms.tgt_pltfrm: na
ms.devlang: 
ms.topic: tutorial
ms.date: 12/23/2017
ms.author: v-ruogun
ms.openlocfilehash: 9ebf773cf39d832416dce820e67201c21a679296
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2018
---
# <a name="simulate-a-failure-in-accessing-read-access-redundant-storage"></a>Simular uma falha ao aceder ao armazenamento redundante com acesso de leitura

Este tutorial é a segunda parte de uma série.  Neste tutorial, pode utilizar o [Fiddler](#simulate-a-failure-with-fiddler) ou o [Encaminhamento Estático](#simulate-a-failure-with-an-invalid-static-route) para simular a falha de pedidos para o ponto final primário da sua conta de armazenamento [ georredundante com acesso de leitura](../common/storage-redundancy.md#read-access-geo-redundant-storage) (RA-GRS) e fazer com que a aplicação leia do ponto final secundário.

![Aplicação de cenário](media/storage-simulate-failure-ragrs-account-app/scenario.png)

Para concluir este tutorial, tem de ter concluído o tutorial de armazenamento anterior: [Tornar os dados da aplicação altamente disponíveis com o armazenamento do Azure][previous-tutorial].

Na segunda parte da série, saiba como:

> [!div class="checklist"]
> * Executar e colocar em pausa a aplicação
> * Simular uma falha com o [fiddler](#simulate-a-failure-with-fiddler) ou [uma rota estática inválida](#simulate-a-failure-with-an-invalid-static-route) 
> * Simular o restauro do ponto final primário


## <a name="prerequisites"></a>Pré-requisitos

Para simular uma falha com o Fiddler: 

* Transferir e [instalar o Fiddler](https://www.telerik.com/download/fiddler)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="simulate-a-failure-with-fiddler"></a>Simular uma falha com o Fiddler

Para simular uma falha com o Fiddler, insira uma resposta com falhas de pedidos no ponto final primário da sua conta de armazenamento RA-GRS para simular uma falha.

Siga os passos seguintes para simular uma falha e o restauro do ponto final primário com o fiddler.

### <a name="launch-fiddler"></a>Iniciar o fiddler

Abra o Fiddler, selecione **Regras** e **Personalizar Regras**.

![Personalizar regras do Fiddler](media/storage-simulate-failure-ragrs-account-app/figure1.png)

O Fiddler ScriptEditor é iniciado, mostrando o ficheiro **SampleRules.js**. Este ficheiro é utilizado para personalizar o Fiddler. Cole o seguinte exemplo de código na função `OnBeforeResponse`. O novo código é comentado para garantir que a lógica criada não é implementada imediatamente. Depois de concluído, selecione **Ficheiro** e **Guardar** para guardar as alterações.

```javascript
    /*
        // Simulate data center failure
        // After it is successfully downloading the blob, pause the code in the sample,
        // uncomment these lines of script, and save the script.
        // It will intercept the (probably successful) responses and send back a 503 error. 
        // When you're ready to stop sending back errors, comment these lines of script out again 
        //     and save the changes.

        if ((oSession.hostname == "contosoragrs.blob.core.windows.net") 
            && (oSession.PathAndQuery.Contains("HelloWorld"))) {
            oSession.responseCode = 503;  
        }
    */
```

![Colar regra personalizada](media/storage-simulate-failure-ragrs-account-app/figure2.png)

### <a name="start-and-pause-the-application"></a>Iniciar e colocar em pausa a aplicação

Execute a aplicação no seu IDE ou editor de texto. Assim que a aplicação começar a ler do ponto final primário, prima **qualquer tecla** da janela da consola para colocar a aplicação em pausa.

### <a name="simulate-failure"></a>Simular falha

Com a aplicação em pausa, pode agora anular os comentários da regra personalizada que foi guardada no Fiddler num passo anterior. O código de exemplo procura pedidos para a conta de armazenamento RA-GRS e, se o caminho contiver o nome da imagem, `HelloWorld`, devolve um código de resposta de `503 - Service Unavailable`.

Navegue para o Fiddler e selecione **Regras** -> **Personalizar Regras...**.  Anule os comentários das seguintes linhas, substitua `STORAGEACCOUNTNAME` pelo nome da conta de armazenamento. Selecione **Ficheiro** -> **Guardar** para guardar as alterações. 

> [!NOTE]
> Se estiver a executar a aplicação de exemplo no Linux, tem de reiniciar o Fiddler sempre que edita o ficheiro **CustomRule.js**, para que o Fiddler instale a lógica personalizada. 
> 
> 


```javascript
         if ((oSession.hostname == "STORAGEACCOUNTNAME.blob.core.windows.net")
         && (oSession.PathAndQuery.Contains("HelloWorld"))) {
         oSession.responseCode = 503;
         }
```

Para retomar a aplicação, prima **qualquer tecla**.

Assim que a aplicação começar a ser novamente executada, os pedidos para o ponto final primário começam a falhar. A aplicação tenta restabelecer a ligação ao ponto final primário 5 vezes. Após o limiar de cinco tentativas falhadas, solicita a imagem do ponto final secundário só de leitura. Depois de a aplicação obter a imagem 20 vezes com êxito do ponto final secundário, a aplicação tenta ligar ao ponto final primário. Se o ponto final primário continuar inacessível, a aplicação retoma a leitura a partir do ponto final secundário. Este padrão é o padrão de [Disjuntor Automático](https://docs.microsoft.com/azure/architecture/patterns/circuit-breaker) descrito no tutorial anterior.

![Colar regra personalizada](media/storage-simulate-failure-ragrs-account-app/figure3.png)

### <a name="simulate-primary-endpoint-restoration"></a>Simular o restauro do ponto final primário

Com o conjunto de regras personalizadas do Fiddler definido no passo anterior, os pedidos para o ponto final primário falham. Para simular novamente o funcionamento do ponto final primário, remova a lógica para inserir o erro `503`.

Para colocar a aplicação em pausa, prima **qualquer tecla**.

Navegue para o Fiddler e selecione **Regras** e **Personalizar Regras...**.  Comente ou remova a lógica personalizada da função `OnBeforeResponse`, mantendo a função predefinida. Selecione **Ficheiro** e **Guardar** para guardar as alterações.

![Remover regra personalizada](media/storage-simulate-failure-ragrs-account-app/figure5.png)

Quando estiver concluído, prima **qualquer tecla** para retomar a aplicação. A aplicação continua a ler do ponto final primário até atingir 999 leituras.

![Retomar aplicação](media/storage-simulate-failure-ragrs-account-app/figure4.png)


## <a name="simulate-a-failure-with-an-invalid-static-route"></a>Simular uma falha com uma rota estática inválida 
Pode criar uma rota estática inválida para todos os pedidos para o ponto final primário da sua conta de armazenamento [georredundante com acesso de leitura](../common/storage-redundancy.md#read-access-geo-redundant-storage) (RA-GRS). Neste tutorial, o anfitrião local é utilizado como o gateway de encaminhamento de pedidos para a conta de armazenamento. A utilização do anfitrião local como o gateway faz com que todos os pedidos para o ponto final primário da conta de armazenamento sejam redirecionados para dentro do anfitrião, o que, posteriormente, origina falhas. Siga os passos seguintes para simular uma falha e o restauro do ponto final primário com uma rota estática inválida. 

### <a name="start-and-pause-the-application"></a>Iniciar e colocar em pausa a aplicação

Execute a aplicação no seu IDE ou editor de texto. Assim que a aplicação começar a ler do ponto final primário, prima **qualquer tecla** da janela da consola para colocar a aplicação em pausa. 

### <a name="simulate-failure"></a>Simular falha

Com a aplicação em pausa, inicie a linha de comandos no Windows como administrador ou execute o terminal como raiz no Linux. Para obter informações sobre o domínio de ponto final primário da conta de armazenamento, introduza o seguinte comando numa linha de comandos ou terminal.

```
nslookup STORAGEACCOUNTNAME.blob.core.windows.net
``` 
 Substitua `STORAGEACCOUNTNAME` pelo nome de sua conta de armazenamento. Copie o endereço IP da sua conta de armazenamento para um editor de texto, para utilização posterior. Para obter o endereço IP do anfitrião local, escreva `ipconfig` na linha de comandos do Windows, ou `ifconfig` no terminal do Linux. 

Para adicionar uma rota estática para um anfitrião de destino, escreva o seguinte comando numa linha de comandos do Windows ou no terminal do Linux. 


# <a name="linuxtablinux"></a>[Linux](#tab/linux)

  route add <destination_ip> gw <gateway_ip>

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

  route add <destination_ip> <gateway_ip>

---
 
Substitua `<destination_ip>` pelo seu endereço IP da conta de armazenamento, e `<gateway_ip>` pelo seu endereço IP do anfitrião local. Para retomar a aplicação, prima **qualquer tecla**.

Assim que a aplicação começar a ser novamente executada, os pedidos para o ponto final primário começam a falhar. A aplicação tenta restabelecer a ligação ao ponto final primário 5 vezes. Após o limiar de cinco tentativas falhadas, solicita a imagem do ponto final secundário só de leitura. Depois de a aplicação obter a imagem 20 vezes com êxito do ponto final secundário, a aplicação tenta ligar ao ponto final primário. Se o ponto final primário continuar inacessível, a aplicação retoma a leitura a partir do ponto final secundário. Este padrão é o padrão de [Disjuntor Automático](/azure/architecture/patterns/circuit-breaker.md) descrito no tutorial anterior.

### <a name="simulate-primary-endpoint-restoration"></a>Simular o restauro do ponto final primário

Para simular novamente o funcionamento do ponto final primário, elimine a rota estática do ponto final primário da tabela de encaminhamento. Isto permite que todos os pedidos para o ponto final primário sejam encaminhados através de um gateway predefinido. 

Para eliminar a rota estática de um anfitrião de destino, a conta de armazenamento, escreva o seguinte comando numa linha de comandos do Windows ou num terminal do Linux. 
 
# <a name="linuxtablinux"></a>[Linux](#tab/linux)

route del <destination_ip> gw <gateway_ip>

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

route delete <destination_ip> <gateway_ip>

---

Prima **qualquer tecla** para retomar a aplicação. A aplicação continua a ler do ponto final primário até atingir 999 leituras.

![Retomar aplicação](media/storage-simulate-failure-ragrs-account-app/figure4.png)


## <a name="next-steps"></a>Passos seguintes

Na segunda parte da série, aprendeu a simular uma falha para testar o armazenamento georredundante com acesso de leitura, como:

> [!div class="checklist"]
> * Executar e colocar em pausa a aplicação
> * Simular uma falha com o [fiddler](#simulate-a-failure-with-fiddler) ou [uma rota estática inválida](#simulate-a-failure-with-an-invalid-static-route) 
> * Simular o restauro do ponto final primário

Siga esta ligação para ver os exemplos de armazenamento pré-criados.

> [!div class="nextstepaction"]
> [Exemplos de script de armazenamento do Azure](storage-samples-blobs-cli.md)

[previous-tutorial]: storage-create-geo-redundant-storage.md