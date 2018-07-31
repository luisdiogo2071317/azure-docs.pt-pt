---
title: incluir ficheiro
description: incluir ficheiro
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: ab6fdbcd3d1a6a5e611809ccee2343fced05d1e0
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39189426"
---
Até aqui, tem executado o código da aplicação como se fosse o único programador a trabalhar na aplicação. Nesta secção, irá aprender até que ponto o Azure Dev Spaces simplifica o desenvolvimento em equipa ao:
* Permita que uma equipa de programadores trabalhe no mesmo ambiente, ao trabalhar num espaço de desenvolvimento partilhado ou em espaços de desenvolvimento distintos, conforme necessário...
* Suportar que cada programador faça iterações no respetivo código de forma isolada e sem receio de danificar o código de terceiros.
* Testar o código ponto a ponto, antes de proceder à consolidação do código, sem ter de criar cenários fictícios ou simular dependências.

### <a name="challenges-with-developing-microservices"></a>Desafios relativamente ao desenvolvimento de microsserviços
Este exemplo de aplicação não apresenta índices de complexidade elevados até ao momento. Todavia, num cenário de desenvolvimento real, os desafios começam a surgir à medida que são adicionados mais serviços e a equipa de desenvolvimento começa a aumentar.

Imagine que está a trabalhar num serviço que interage com dezenas de outros serviços.

- A execução de todos os elementos para fins de desenvolvimento a nível local pode tornar-se uma perspetiva irrealista. A máquina de desenvolvimento pode não ter recursos suficientes para executar a aplicação completa. Quem sabe, a aplicação tem pontos finais que têm de estar acessíveis publicamente (por exemplo, a aplicação responde a um webhook a partir de uma aplicação SaaS).

- Pode tentar executar apenas os serviços de que depende, mas isto significa que teria de conhecer o âmbito completo das dependências (por exemplo, as dependências das dependências). Por outro lado, pode ser uma questão de não saber exatamente como criar e executar as dependências visto não ter estado envolvido no desenvolvimento das mesmas.
- Alguns programadores recorrem à simulação ou criação de cenários fictícios de muitas das suas dependências de serviço. Esta pode ser uma abordagem útil. No entanto, mais cedo ou mais tarde, a gestão desses cenários fictícios acaba por exigir o seu próprio esforço de desenvolvimento. Além disso, esta abordagem faz com que o seu espaço de desenvolvimento acabe por divergir bastante do da produção, dando azo ao aparecimento de erros subtis.
- Daqui decorre que se torna difícil levar a cabo qualquer tipo de teste ponto a ponto. Realisticamente, o teste de integração só pode ocorrer após uma consolidação, o que significa que irá ver os problemas numa fase posterior no ciclo de desenvolvimento.

![](../media/common/microservices-challenges.png)


### <a name="work-in-a-shared-dev-space"></a>Trabalhar num espaço de desenvolvimento partilhado
O Azure Dev Spaces permite-lhe configurar um espaço de desenvolvimento *partilhado* no Azure. Cada programador pode concentrar-se unicamente na respetiva parte da aplicação e, assim, desenvolver o *código de pré-consolidação* de forma iterativa num espaço de desenvolvimento que já contém todos os outros serviços e recursos na cloud de que os respetivos cenários dependem. As dependências estão sempre atualizadas e os programadores trabalham de uma forma que reflete a produção.

### <a name="work-in-your-own-space"></a>Trabalhar no seu próprio espaço
À medida que desenvolve o código para o seu serviço e até estar preparado para o implementar, é frequente o código não estar em bom estado. A formulação, os testes e as experiências com soluções ao nível do código são um processo iterativo contínuo. O Azure Dev Spaces fornece o conceito de **espaço**, onde pode trabalhar de forma isolada e sem receio de prejudicar os membros da sua equipa.

> [!Note]
> Antes de continuar, feche todas as janelas do VS Code para ambos os serviços e, em seguida, execute `azds up -d` em cada uma das pastas raiz do serviço. (Esta é uma limitação da Pré-visualização.)

Vamos dar uma vista de olhos nos serviços que estão atualmente a ser executados. Execute o comando `azds list-up` e verá um resultado semelhante ao seguinte:

```
Name                          DevSpace  Type     Updated      Status
----------------------------  --------  -------  -----------  ----------------
mywebapi                      default   Service  10m 1s ago   Running
mywebapi-54f9cf5b59-bjnkm     default   Pod      10m 4s ago   Running
webfrontend-5b697958d6-b6v96  default   Pod      26m 38s ago  Init:1/3:mindaro-build
```

A coluna do DevSpace mostra que ambos os serviços estão em execução num espaço designado `default`. Qualquer pessoa que abra o URL público e navegue para a aplicação Web irá invocar o caminho do código que escreveu anteriormente que é executado em ambos os serviços. Agora suponha que pretende continuar a desenvolver `mywebapi`. Como pode fazer alterações ao código e testá-las, sem interromper outros programadores que estejam a utilizar o ambiente de desenvolvimento? Para tal, terá de configurar o seu próprio espaço.

### <a name="create-a-dev-space"></a>Criar um espaço de programador
Para executar a sua própria versão do `mywebapi` num espaço diferente do que `default`, pode criar o seu próprio espaço com o seguinte comando:

``` 
azds space select --name scott
```

Quando pedido, selecione `default` como o **espaço de programador principal**. Isto significa que o nosso novo espaço, `default/scott`, irá derivar do espaço `default`. Em breve verá como isto nos irá ajudar com os testes. 

No exemplo acima, utilizei o meu nome para o novo espaço, para que seja identificável perante os meus colegas de que é o espaço onde estou a trabalhar, mas pode dar o nome que quiser e ser flexível sobre o que significa, como "sprint4" ou "demo."

Execute o comando `azds space list` para ver uma lista de todos os espaços no ambiente de desenvolvimento. Aparece um asterisco (*) junto do espaço atualmente selecionado. No seu caso, o espaço designado "default/scott" foi selecionado automaticamente quando foi criado. Pode selecionar outro espaço em qualquer altura com o comando `azds space select`.
