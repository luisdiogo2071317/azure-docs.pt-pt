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
ms.openlocfilehash: f77a036d41ce551d9eab0250eaf4dc16444b24da
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
## <a name="build-and-run-code-in-kubernetes"></a>Compilar e executar código no Kubernetes
Vamos executar nosso código! Na janela de terminal, execute este comando a partir de **pasta raiz de código**, webfrontend:

```cmd
azds up
```

Manter a par no resultado do comando, verá vários aspetos como se avança:
- Código de origem está sincronizado com o ambiente de desenvolvimento no Azure.
- Uma imagem de contentor baseia-se no Azure, tal como especificado pelos ativos de Docker na sua pasta de código.
- Objetos de Kubernetes são criados que utilizam a imagem de contentor especificado, o gráfico de Helm na sua pasta de código.
- São apresentadas informações sobre pontos finais do contentor. No nosso caso, iremos está à espera de um URL público de HTTP.
- Pressupondo que as fases acima concluída com êxito, deve começar a ver `stdout` (e `stderr`) de saída como o contentor é iniciado.

> [!Note]
> Estes passos irão demorar mais tempo a primeira vez o `up` comando é executado, mas as execuções subsequentes devem ser mais rápidas.

## <a name="test-the-web-app"></a>Testar a aplicação Web
Analisar o resultado da consola para obter informações sobre o URL público que foi criada com o `up` comando. Será sob a forma: 

`Running at public URL: http://<servicename>-<environmentname>.<guid>.<region>.aksapp.io` 

Abra este URL numa janela do browser e deverá ver a aplicação web de carga. Como o contentor executa, `stdout` e `stderr` saída é transmitida em fluxo para a janela de terminal.
