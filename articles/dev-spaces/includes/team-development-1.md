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
ms.openlocfilehash: 96a749c0cb59759e9294f52bd4f631d7fdc2275f
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
Até ao momento que tiver sido executar código da aplicação como se atualizasse o programador apenas a trabalhar na aplicação. Nesta secção, irá aprender como espaços de programador do Azure simplifica o desenvolvimento de equipa:
* Ative uma equipa de programadores para funcionar no mesmo ambiente de desenvolvimento.
* Suporta cada programador iterating no respetivo código no isolamento e sem fear de danificando a outras pessoas.
* Testar código ponto-a-ponto, antes da consolidação de código, sem ter de criar mocks ou simular dependências.

## <a name="challenges-with-developing-microservices"></a>Desafios com o desenvolvimento micro-serviços
O exemplo de aplicação não é muito complexo neste momento. Mas de desenvolvimento do mundo real, desafios em breve surgir como adicionar mais serviços e o crescimentos de equipa de desenvolvimento.

Imagem por si trabalhar num serviço que interage com dezenas de outros serviços.

- Pode ficar irreal tudo localmente para o desenvolvimento de executar. A máquina de desenvolvimento pode não ter recursos suficientes para executar a aplicação completa. Ou, talvez a sua aplicação tenha pontos finais que têm de ser publicamente acessível (por exemplo, a aplicação responde a um webhook a partir de uma aplicação SaaS).

- Pode tentar executar apenas os serviços que depende do, mas isto significa que iria precisar de saber o fecho completo das dependências (por exemplo, dependências de dependências). Em alternativa, é um fim de não facilmente saber como criar e executar as suas dependências, porque não funciona nos mesmos.
- Alguns programadores recorrer a simulando ou mocking cópias de segurança, muitos das respetivas dependências do serviço. Esta abordagem pode ajudar a por vezes, mas gerir esses mocks logo que pode efetuar no seu próprio esforço de programação. Plus, esta abordagem leva ao seu ambiente de desenvolvimento à procura muito diferentes para produção e erros subtis podem creep.
- Segue que efetuar qualquer tipo de teste de ponto a ponto torna-se difícil. Teste de integração pode acontecer apenas verdade consolidação pós-implementação, o que significa que consulte problemas mais à frente do ciclo de desenvolvimento.

![](../media/common/microservices-challenges.png)


## <a name="work-in-a-shared-development-environment"></a>Trabalhar num ambiente de desenvolvimento partilhado
Com os espaços de programador do Azure, pode configurar um *partilhado* ambiente de desenvolvimento no Azure. Cada programador pode concentrar-se apenas por parte da aplicação e pode desenvolver iteratively *previamente consolidar o código* num ambiente que já contém todos os outros serviços e recursos de nuvem que os seus cenários dependem. As dependências estão sempre atualizadas e os programadores estão a funcionar de forma que reflete a produção.

## <a name="work-in-your-own-space"></a>No seu próprio espaço de trabalho
Como desenvolver o código para o seu serviço e antes de está pronto para a verificar no, código, muitas vezes, não será em bom estado. Iteratively ainda está a formação, testá-lo e conseguirmos uma com soluções. Os espaços de programador do Azure fornece o conceito de uma **espaço**, que permite-lhe trabalhar de forma isolada e sem fear de interrompendo os membros do agrupamento.

> [!Note]
> Antes de continuar, feche todas as janelas de VS Code para ambos os serviços e, em seguida, execute `azds up -d` em cada uma das pastas de raiz do serviço. (Esta é uma limitação de pré-visualização).

Vamos um olhar onde os serviços estão atualmente a ser executada. Execute o `azds list` comando e, irá ver um resultado semelhante ao seguinte:

```
Name         Space     Chart              Ports   Updated     Access Points
-----------  --------  -----------------  ------  ----------  -------------------------
mywebapi     default  mywebapi-0.1.0     80/TCP  2m ago     <not attached>
webfrontend  default  webfrontend-0.1.0  80/TCP  1m ago     https://webfrontend-contosodev.1234abcdef.westeurope.aksapp.io
```

A coluna de espaço mostra que ambos os serviços estão em execução num espaço de nome `default`. Qualquer pessoa que abre o URL público e navega para a aplicação web será invocar o caminho de código que escreveu anteriormente, que é executada através de ambos os serviços. Agora suponha que pretende continuar a desenvolver `mywebapi`. Como pode fazer alterações ao código e testá-los e não interrompem outros programadores que estiver a utilizar o ambiente de desenvolvimento? Para tal, configure o seus próprios espaço.

## <a name="create-a-space"></a>Criar um espaço
Para executar a sua própria versão do `mywebapi` num espaço diferente `default`, pode criar o seu próprio espaço usando o seguinte comando:

``` 
azds space create --name scott
```

No exemplo acima, posso tiver utilizado o meu nome para o novo espaço para que seja identificável a minha elementos de rede que é o espaço estou a funcionar no, mas pode chamá-lo tudo o que gosta e ser flexível sobre o que significa, como 'sprint4' ou 'demonstração'.

Execute o `azds space list` comando para ver uma lista de todos os espaços no ambiente de desenvolvimento. Surja um asterisco (*) junto do espaço atualmente selecionado. No seu caso, o espaço de nome 'blogue' foi selecionado automaticamente quando foi criado. Pode selecionar outro espaço em qualquer altura com o `azds space select` comando.
