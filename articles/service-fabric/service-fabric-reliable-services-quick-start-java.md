---
title: "Criar a sua primeira microsserviço fiável de Azure em Java | Microsoft Docs"
description: "Introdução à criação de uma aplicação do Microsoft Azure Service Fabric com serviços sem monitorização de estado e com monitorização de estado."
services: service-fabric
documentationcenter: java
author: suhuruli
manager: timlt
editor: 
ms.assetid: 7831886f-7ec4-4aef-95c5-b2469a5b7b5d
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: suhuruli
ms.openlocfilehash: e885a482edcba48c18e425c54f4acc28ee650ddd
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="get-started-with-reliable-services"></a>Introdução ao Reliable Services
> [!div class="op_single_selector"]
> * [C# no Windows](service-fabric-reliable-services-quick-start.md)
> * [Java em Linux](service-fabric-reliable-services-quick-start-java.md)
>
>

Este artigo explica as noções básicas do Azure Service Fabric Reliable Services e explica-lhe como criar e implementar uma aplicação de serviço fiável simple escrita em Java. Este vídeo do Microsoft Virtual Academy também mostra-lhe como criar um serviço fiável sem monitorização de estado:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=DOX8K86yC_206218965">  
<img src="./media/service-fabric-reliable-services-quick-start-java/ReliableServicesJavaVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="installation-and-setup"></a>Instalação e configuração
Antes de começar, certifique-se de que tem o ambiente de desenvolvimento do Service Fabric configurar no seu computador.
Se precisar de configurá-lo, aceda a [introdução no Mac](service-fabric-get-started-mac.md) ou [introdução no Linux](service-fabric-get-started-linux.md).

## <a name="basic-concepts"></a>Conceitos básicos
Para começar a Reliable Services, só tem de compreender alguns conceitos básicos:

* **Tipo de serviço**: Esta é a implementação de serviço. Está definido pela classe de escrita que expande `StatelessService` e quaisquer outras código ou dependências utilizadas nas mesmas, juntamente com um nome e um número de versão.
* **Com o nome de instância de serviço**: ao executar o serviço, criar instâncias nomeadas do seu tipo de serviço, muito como criar instâncias do objecto de um tipo de classe. Instâncias de serviço são de facto instâncias de objeto da sua classe de serviço escreve.
* **Anfitrião do serviço**: as instâncias de serviço com nome que cria tem de executar no interior de um anfitrião. O anfitrião do serviço é apenas um processo em que podem executar instâncias do seu serviço.
* **Serviço de registo**: registo reúne tudo. O tipo de serviço tem de ser registado com o runtime Service Fabric num anfitrião de serviço para permitir que o Service Fabric criar instâncias do mesmo para ser executada.  

## <a name="create-a-stateless-service"></a>Criar um serviço sem monitorização de estado
Comece por criar uma aplicação de Service Fabric. O SDK de Service Fabric para Linux inclui um Yeoman gerador para fornecer andaime para uma aplicação de Service Fabric com um serviço sem estado. Comece por executar o Yeoman seguinte comando:

```bash
$ yo azuresfjava
```

Siga as instruções para criar um **sem monitorização de estado de serviço fiável**. Para este tutorial, atribua um nome de aplicação "HelloWorldApplication" e o serviço "Olámundo". O resultado inclui diretórios para o `HelloWorldApplication` e `HelloWorld`.

```bash
HelloWorldApplication/
├── build.gradle
├── HelloWorld
│   ├── build.gradle
│   └── src
│       └── statelessservice
│           ├── HelloWorldServiceHost.java
│           └── HelloWorldService.java
├── HelloWorldApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   └── _readme.txt
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── install.sh
├── settings.gradle
└── uninstall.sh
```
### <a name="service-registration"></a>Registo do serviço
Tipos de serviço tem de ser registados com o tempo de execução do Service Fabric. O tipo de serviço está definido no `ServiceManifest.xml` e a classe de serviço que implementa `StatelessService`. Registo do serviço é executado no ponto de entrada principal do processo. Neste exemplo, o ponto de entrada principal do processo é `HelloWorldServiceHost.java`:

```java
public static void main(String[] args) throws Exception {
    try {
        ServiceRuntime.registerStatelessServiceAsync("HelloWorldType", (context) -> new HelloWorldService(), Duration.ofSeconds(10));
        logger.log(Level.INFO, "Registered stateless service type HelloWorldType.");
        Thread.sleep(Long.MAX_VALUE);
    }
    catch (Exception ex) {
        logger.log(Level.SEVERE, "Exception in registration:", ex);
        throw ex;
    }
}
```

## <a name="implement-the-service"></a>Implementar o serviço

Abra **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**. Esta classe define o tipo de serviço e pode executar qualquer código. A API de serviço fornece dois pontos de entrada para o seu código:

* Um método de ponto de entrada open-ended, denominado `runAsync()`, onde pode começar a executar quaisquer cargas de trabalho, incluindo as cargas de trabalho de computação de execução longa.

```java
@Override
protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {
    ...
}
```

* Um ponto de entrada de comunicação em que pode plug-in a pilha de comunicação de eleição. Este é onde pode começar a receber pedidos de utilizadores e outros serviços.

```java
@Override
protected List<ServiceInstanceListener> createServiceInstanceListeners() {
    ...
}
```

Neste tutorial, iremos focar-se no `runAsync()` o método de ponto de entrada. Este é onde imediatamente pode começar a executar o seu código.

### <a name="runasync"></a>Runasync com
A plataforma chama este método quando uma instância de um serviço está pronto para executar e colocá-la. Para um serviço sem monitorização de estado, basta que significa quando a instância de serviço é aberta. Um token de cancelamento é fornecido para coordenar quando a instância de serviço tem de ser fechado. No Service Fabric, este ciclo de abertura/fecho de uma instância de serviço pode ocorrer demasiadas vezes ao longo da duração do serviço como um todo. Esta situação pode ocorrer por vários motivos, incluindo:

* O sistema move as instâncias de serviço para balanceamento de recurso.
* Falhas ocorrerem no seu código.
* A aplicação ou o sistema está atualizado.
* O hardware subjacente sofre uma falha.

Esta orquestração é gerida pelo serviço de recursos de infraestrutura para manter o seu serviço altamente disponível e corretamente equilibrada.

`runAsync()`não deverá bloquear forma síncrona. A implementação de runasync com deverá devolver um CompletableFuture para permitir que o tempo de execução continuar. Se a carga de trabalho tem de implementar uma tarefa de execução longa que deve ser efetuada dentro de CompletableFuture.

#### <a name="cancellation"></a>Cancelamento
O cancelamento da sua carga de trabalho é um esforço cooperative orquestrado pelo token de cancelamento fornecido. O sistema aguarda que a tarefa terminar (por conclusão com êxito, cancelamento ou falhas) antes de se move. É importante honrar o token de cancelamento, concluir qualquer trabalho e sair `runAsync()` mais rapidamente possível quando o sistema pede cancelamento. O exemplo seguinte demonstra como processar um evento de cancelamento:

```java
@Override
protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {

    // TODO: Replace the following sample code with your own logic
    // or remove this runAsync override if it's not needed in your service.

    return CompletableFuture.runAsync(() -> {
        long iterations = 0;
        while(true)
        {
        cancellationToken.throwIfCancellationRequested();
        logger.log(Level.INFO, "Working-{0}", ++iterations);

        try {
            Thread.sleep(1000);
        } catch (InterruptedException ex){}
        }
    });
}
```

Neste exemplo de serviço sem monitorização de estado, a contagem é armazenada numa variável local. Mas porque se trata de um serviço sem monitorização de estado, o valor que é armazenado existe apenas para o ciclo de vida atual da respetiva instância de serviço. Quando move ou reinicia o serviço, o valor é perdido.

## <a name="create-a-stateful-service"></a>Criar um serviço com estado
Serviço de recursos de infraestrutura apresenta um novo tipo de serviço que tem o estado monitorizado. Um serviço com estado pode manter o estado da forma fiável no serviço de si próprio, localizado conjuntamente com o código que está a ser utilizado. Estado é efetuado elevado pelo Service Fabric sem a necessidade de manter o estado para um arquivo de externo.

Para converter um valor de contador sem monitorização de estado de elevada disponibilidade e persistente, mesmo quando move ou reinicia o serviço, terá de um serviço com monitorização de estado.

No mesmo diretório como a aplicação de Olámundo, pode adicionar um novo serviço executando o `yo azuresfjava:AddService` comando. Escolha o fiável de monitorização de estado "serviço" para a sua estrutura e o serviço de "HelloWorldStateful" nomes. 

A aplicação deve agora ter dois serviços: o serviço sem estado Olámundo e a monitorização de estado do serviço HelloWorldStateful.

Um serviço com estado tem os mesmos pontos de entrada como um serviço sem estado. A principal diferença é que a disponibilidade de um fornecedor de estado que pode armazenar o estado de forma fiável. Service Fabric inclui uma implementação do fornecedor de estado designada coleções fiável, que permite-lhe criar estruturas de dados replicados através do Gestor de estado fiável. Um serviço fiável com monitorização de estado utiliza este fornecedor de estado por predefinição.

Abrir HelloWorldStateful.java no **HelloWorldStateful -> src**, que contém o seguinte método runasync com:

```java
@Override
protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {
    Transaction tx = stateManager.createTransaction();
    return this.stateManager.<String, Long>getOrAddReliableHashMapAsync("myHashMap").thenCompose((map) -> {
        return map.computeAsync(tx, "counter", (k, v) -> {
            if (v == null)
                return 1L;
            else
                return ++v;
            }, Duration.ofSeconds(4), cancellationToken)
                .thenCompose((r) -> tx.commitAsync())
                .whenComplete((r, e) -> {
            try {
                tx.close();
            } catch (Exception e) {
                logger.log(Level.SEVERE, e.getMessage());
            }
        });
    });
}
```

### <a name="runasync"></a>Runasync com
`RunAsync()`funciona da mesma forma nos serviços de monitorização de estado e sem monitorização de estado. No entanto, num serviço com monitorização de estado, a plataforma executa tarefas adicionais em seu nome antes de ser executada `RunAsync()`. Este trabalho pode incluir a garantir que o Gestor de estado fiável e coleções fiável, estará pronto a utilizar.

### <a name="reliable-collections-and-the-reliable-state-manager"></a>Coleções fiáveis e o Gestor de estado fiável
```java
ReliableHashMap<String,Long> map = this.stateManager.<String, Long>getOrAddReliableHashMapAsync("myHashMap")
```

[ReliableHashMap](https://docs.microsoft.com/java/api/microsoft.servicefabric.data.collections._reliable_hash_map) é uma implementação de dicionário que pode utilizar para armazenar o estado da forma fiável no serviço. Com o Service Fabric e Hashmaps fiável, pode armazenar dados diretamente no seu serviço sem a necessidade de um arquivo persistente externo. Hashmaps fiáveis tornar os dados de elevada disponibilidade. Service Fabric executa esta operação através da criação e gestão de vários *réplicas* do seu serviço para si. Também fornece uma API que deduz ausente complexidades de gerir as réplicas e os respetivos transições de estado.

Coleções fiáveis podem armazenar qualquer tipo de Java, incluindo os tipos personalizados, com algumas limitações:

* Recursos de infraestrutura de serviço faz com que o seu estado altamente disponível *replicar* Estado em nós e fiável Hashmap armazena os dados para disco local em cada réplica. Isto significa que tudo o que é armazenado no Hashmaps fiável tem de ser *serializável*. 
* Objetos são replicados para elevada disponibilidade ao consolidar transações em Hashmaps fiável. Objetos armazenados na Hashmaps fiável são mantidos na memória local no seu serviço. Isto significa que tem uma referência local para o objeto.
  
   É importante que não mutate locais instâncias desses objetos sem efetuar uma operação de atualização na coleção de fiável numa transação. Isto acontece porque as alterações ao locais instâncias de objetos não serão replicadas automaticamente. Tem de inserir novamente o objeto no dicionário ou utilize um do *atualizar* os métodos no dicionário.

O Gestor de estado fiável gere Hashmaps fiável para si. Pode simplesmente colocar o Gestor de estado fiável para uma coleção fiável pelo nome em qualquer altura e em qualquer lugar no seu serviço. O Gestor de estado fiável garante que obtém uma referência de volta. Não recomendamos que guarde as referências a instâncias de coleção fiável no membro de classe variáveis ou propriedades. Deve ser dada especial cuidado para garantir que a referência está definida para uma instância de todas as vezes no ciclo de vida do serviço. O Gestor de estado fiável processa este trabalho por si e está otimizado para visitas de repetições.


### <a name="transactional-and-asynchronous-operations"></a>Operações transacionais e assíncronas
```java
return map.computeAsync(tx, "counter", (k, v) -> {
    if (v == null)
        return 1L;
    else
        return ++v;
    }, Duration.ofSeconds(4), cancellationToken)
        .thenCompose((r) -> tx.commitAsync())
        .whenComplete((r, e) -> {
    try {
        tx.close();
    } catch (Exception e) {
        logger.log(Level.SEVERE, e.getMessage());
    }
});
```

As operações Hashmaps fiável são assíncronas. Isto acontece porque as operações de escrita de coleções fiável efetuarem operações de e/s para replicar e manter os dados para o disco.

As operações de Hashmap fiável *transacional*, para que pode manter estado consistente em vários Hashmaps fiável e operações. Por exemplo, pode obter um item de trabalho a partir de um dicionário fiável, efetuar uma operação no mesmo e guardar o resultado na anoter Hashmap fiável, tudo dentro de uma única transação. Esta é tratada como uma operação Atómica e garante que será efetuada com êxito a operação de toda ou irá reverter a operação de toda. Se ocorrer um erro depois de anular o item, mas antes de guardar o resultado, toda a transação é revertida e o item permanece na fila para processamento.


## <a name="run-the-application"></a>Executar a aplicação

O Yeoman andaime inclui um script de gradle para compilar a aplicação e a implementar e remover a aplicação de scripts de bash. Para executar a aplicação, primeiro crie a aplicação com o gradle:

```bash
$ gradle
```

Isto produz um pacote de aplicação de Service Fabric pode ser implementado utilizando a CLI de recursos de infraestrutura de serviço.

### <a name="deploy-with-service-fabric-cli"></a>Implementar com a CLI do Service Fabric

O script de install.sh contém os comandos da CLI de recursos de infraestrutura de serviço necessários para implementar o pacote de aplicação. Execute o script de install.sh para implementar a aplicação.

```bash
$ ./install.sh
```

## <a name="next-steps"></a>Passos Seguintes

* [Introdução à CLI do Service Fabric](service-fabric-cli.md)
