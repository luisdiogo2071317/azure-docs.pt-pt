---
title: "Próximos passos de criação do Service Fabric projeto | Microsoft Docs"
description: "Saiba mais sobre o projeto de aplicação que acabou de criar no Visual Studio.  Saiba como criar serviços utilizando tutoriais e obter mais informações sobre como desenvolver serviços para o Service Fabric."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 299d1f97-1ca9-440d-9f81-d1d0dd2bf4df
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/07/2017
ms.author: rwike77
ms.openlocfilehash: 17eb1e7c2184fe9cae19685a47ea80716292b754
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/09/2017
---
# <a name="your-service-fabric-application-and-next-steps"></a>A aplicação de Service Fabric e os passos seguintes
Foi criada a sua aplicação de Service Fabric do Azure. Este artigo descreve tutoriais para experimentar, makeup do seu projeto, algumas informações mais que poderá estar interessado no e potenciais próximos passos.

## <a name="get-started-with-tutorials-walk-throughs-and-samples"></a>Introdução ao tutoriais, passagens e amostras
Pronto para começar?  

Completar o tutorial de aplicações de .NET. Saiba como [compilar uma aplicação](service-fabric-tutorial-create-dotnet-app.md) com um front-end do ASP.NET Core e um back-end com monitorização de estado, [implementar a aplicação](service-fabric-tutorial-deploy-app-to-party-cluster.md) para um cluster, [configurar CI/CD](service-fabric-tutorial-deploy-app-with-cicd-vsts.md), e [configurar monitorização e diagnóstico](service-fabric-tutorial-monitoring-aspnet.md).

Ou, experimente um dos passagens seguintes e criar o primeiro...
- [Serviço de c# Reliable Services no Windows](service-fabric-reliable-services-quick-start.md) 
- [Serviço de c# Reliable Actors no Windows](service-fabric-reliable-actors-get-started.md) 
- [Serviço de convidado executável no Windows](quickstart-guest-app.md) 
- [Aplicação de contentor do Windows](service-fabric-get-started-containers.md) 

Também poderá estar interessado em experimentar nosso [aplicações de exemplo](http://aka.ms/servicefabricsamples).

## <a name="have-questions-or-feedback--need-to-report-an-issue"></a>Tem perguntas ou comentários?  Tem de comunicar um problema?
Leia [perguntas comuns](service-fabric-common-questions.md) e encontrar respostas sobre o Service Fabric pode fazer e como deve ser utilizada.

[Suporta opções](service-fabric-support.md) lista fóruns StackOverflow e MSDN para solicitar perguntas, bem como as opções de relatórios de problemas, obter suporte e submeter comentários sobre o produto.

## <a name="the-application-project"></a>O projeto de aplicação
Cada nova aplicação inclui um projeto de aplicação. Pode haver um ou dois projetos adicionais, dependendo do tipo de serviço escolhido.

O projeto de aplicação é composta por:

* Um conjunto de referências para os serviços que compõem a sua aplicação.
* Publicar três perfis (1-nó Local, 5-nó Local e nuvem) que pode utilizar para manter as preferências para trabalhar com ambientes diferentes – como preferências relacionados com um ponto final de cluster e se pretende efetuar implementações de atualização por predefinição.
* Parâmetro de aplicação três ficheiros (igual ao acima) que pode utilizar para manter as configurações de aplicação específico do ambiente, como o número de partições para criar um serviço. Saiba como [configurar a sua aplicação para vários ambientes](service-fabric-manage-multiple-environment-app-configuration.md).
* Um script de implementação que pode utilizar para implementar a aplicação na linha de comandos ou como parte de um pipeline de integração e a implementação contínuo automatizado. Saiba mais sobre [implementar aplicações com o PowerShell](service-fabric-deploy-remove-applications.md).
* O manifesto da aplicação, que descreve a aplicação. Pode encontrar o manifesto na pasta ApplicationPackageRoot. Saiba mais sobre [manifestos de aplicação e serviços](service-fabric-application-model.md).



## <a name="learn-more-about-the-programming-models"></a>Saiba mais sobre os modelos de programação
Recursos de infraestrutura de serviço oferece várias formas para escrever e gerir os seus serviços.  Eis o descrição geral e informações conceptuais [sem monitorização de estado e com monitorização de estado Reliable Services](service-fabric-reliable-services-introduction.md), [Reliable Actors](service-fabric-reliable-actors-introduction.md), [contentores](service-fabric-containers-overview.md), [executáveis de convidado ](service-fabric-deploy-existing-app.md), e [sem monitorização de estado e com monitorização de estado dos serviços de ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md).

## <a name="learn-about-service-communication"></a>Saiba mais sobre a comunicação de serviço
Uma aplicação de Service Fabric é composta por diferentes serviços, onde cada serviço executa uma tarefa de especializado. Estes serviços podem comunicar entre si e pode existir aplicações de cliente fora do cluster, que ligar a e comunicam com serviços. Saiba como [comunicação com o operador and entre os serviços de multimédia](service-fabric-connect-and-communicate-with-services.md) no Service Fabric. 

## <a name="learn-about-configuring-application-security"></a>Saiba mais sobre como configurar a segurança da aplicação
Pode proteger aplicações em execução no cluster em contas de utilizador diferente. Serviço de recursos de infraestrutura também ajuda a proteger os recursos utilizados por aplicações no momento da implementação sob as contas de utilizador - por exemplo, ficheiros, diretórios e certificados. Isto faz com que aplicações em execução, mesmo num ambiente alojado partilhado, mais segura entre si.  Saiba como [configurar políticas de segurança para a sua aplicação](service-fabric-application-runas-security.md).

A aplicação pode conter informações confidenciais, tais como cadeias de ligação de armazenamento, as palavras-passe ou outros valores que não devem ser processados em texto simples. Saiba como [gerir segredos na sua aplicação](service-fabric-application-secret-management.md).

## <a name="learn-about-the-application-lifecycle"></a>Saiba mais sobre o ciclo de vida de aplicação
Com outras plataformas, uma aplicação de Service Fabric normalmente realiza as seguintes fases: estrutura, desenvolvimento, teste, implementação, a atualização, manutenção e remoção. [Este artigo](service-fabric-application-lifecycle.md) fornece uma descrição geral de APIs e como são utilizados pelas funções de diferentes durante as fases do ciclo de vida de aplicação de Service Fabric.

## <a name="next-steps"></a>Passos seguintes
- [Criar um cluster do Windows no Azure](service-fabric-tutorial-create-vnet-and-windows-cluster.md).
- Visualizar o cluster, incluindo aplicações implementadas e o esquema físico, com [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
- [Versão e atualizar os serviços](service-fabric-application-upgrade-tutorial.md)


