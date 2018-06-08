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
ms.openlocfilehash: 484567dd9d9c3d050e7be25bd685a5b8d3de0687
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/06/2018
ms.locfileid: "34825523"
---
## <a name="build-and-run-code-in-kubernetes"></a>Criar e executar códigos no Kubernetes
Vamos executar o nosso código! Na janela do terminal, execute este comando a partir da **pasta de código raiz**, webfrontend:

```cmd
azds up
```

Fique de olho no resultado do comando, vai reparar em várias coisas à medida que progride:
- O código fonte é sincronizado com o espaço de programação no Azure.
- É criada uma imagem de contentor no Azure, conforme especificado pelos ativos do Docker na sua pasta de códigos.
- São criados objetos do Kubernetes que utilizam a imagem do contentor, conforme especificado pelo gráfico Helm na sua pasta de códigos.
- São apresentadas as informações sobre o(s) ponto(s) final(ais) do contentor. No nosso caso, contamos com um URL HTTP público.
- Assumindo que os estágios acima sejam concluídos com sucesso, deve começar a ver o resultado `stdout` (e `stderr`), à medida que o contentor é iniciado.

> [!Note]
> Estes passos vão demorar mais tempo quando o comando `up` é executado pela primeira vez, mas as execuções seguintes deverão ser mais rápidas.

### <a name="test-the-web-app"></a>Testar a aplicação Web
Analise o resultado da consola para obter informações sobre o URL público que foi criado pelo comando `up`. Estará na forma: 

`Running at public URL: http://<servicename>-<cluster-name>.<guid>.<region>.aksapp.io` 

Abra este URL numa janela do browser e deve ver o carregamento da aplicação Web. À medida que o contentor é executado, os resultados `stdout` e `stderr` são transmitidos para a janela do terminal.
