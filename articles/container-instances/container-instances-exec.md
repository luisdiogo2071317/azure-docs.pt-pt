---
title: Executar comandos em contentores de execução em instâncias de contentor do Azure
description: Saiba como executar um comando num contentor que está atualmente em execução em instâncias de contentor do Azure
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 03/30/2018
ms.author: marsma
ms.openlocfilehash: 43211f620efb16cbcd722d3d386b1bb862fc6280
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="execute-a-command-in-a-running-azure-container-instance"></a>Executar um comando numa instância de contentor do Azure em execução

Instâncias de contentor do Azure suporta a execução de um comando num contentor em execução. Executar um comando num contentor que já tenha iniciado é especialmente útil durante a programação de aplicações e a resolução de problemas. A utilização desta funcionalidade mais comuns é a iniciar uma shell interativa, de modo a que pode depurar problemas num contentor em execução.

## <a name="run-a-command-with-azure-cli"></a>Executar um comando com a CLI do Azure

Executar um comando num contentor em execução com [exec de contentor az] [ az-container-exec] no [CLI do Azure][azure-cli]:

```azurecli
az container exec --resource-group <group-name> --name <container-group-name> --exec-command "<command>"
```

Por exemplo, para iniciar uma shell de deteção um contentor Nginx:

```azurecli
az container exec --resource-group myResourceGroup --name mynginx --exec-command "/bin/bash"
```

Na saída de exemplo abaixo, a shell de deteção é iniciada num contentor Linux em execução, fornecendo um terminal na qual `ls` é executada:

```console
$ az container exec --resource-group myResourceGroup --name mynginx --exec-command "/bin/bash"
root@caas-83e6c883014b427f9b277a2bba3b7b5f-708716530-2qv47:/# ls
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
root@caas-83e6c883014b427f9b277a2bba3b7b5f-708716530-2qv47:/# exit
exit
Bye.
```

Neste exemplo, a linha de comandos é iniciada num contentor Nanoserver em execução:

```console
$ az container exec --resource-group myResourceGroup --name myiis --exec-command "cmd.exe"
Microsoft Windows [Version 10.0.14393]
(c) 2016 Microsoft Corporation. All rights reserved.

C:\>dir
 Volume in drive C has no label.
 Volume Serial Number is 76E0-C852

 Directory of C:\

03/23/2018  09:13 PM    <DIR>          inetpub
11/20/2016  11:32 AM             1,894 License.txt
03/23/2018  09:13 PM    <DIR>          Program Files
07/16/2016  12:09 PM    <DIR>          Program Files (x86)
03/13/2018  08:50 PM           171,616 ServiceMonitor.exe
03/23/2018  09:13 PM    <DIR>          Users
03/23/2018  09:12 PM    <DIR>          var
03/23/2018  09:22 PM    <DIR>          Windows
               2 File(s)        173,510 bytes
               6 Dir(s)  21,171,609,600 bytes free

C:\>exit
Bye.
```

## <a name="multi-container-groups"></a>Grupos com vários contentores

Se o [grupo contentor](container-instances-container-groups.md) tem vários contentores, tais como um contentor de aplicação e um registo sidecar, especifique o nome do contentor no qual pretende executar o comando com `--container-name`.

Por exemplo, no grupo contentor *mynginx* são dois contentores, *nginx aplicação* e *registador*. Para iniciar uma shell o *nginx aplicação* contentor:

```azurecli
az container exec --resource-group myResourceGroup --name mynginx --container-name nginx-app --exec-command "/bin/bash"
```

## <a name="restrictions"></a>Restrições

Instâncias de contentor do Azure suporta atualmente a iniciar um processo único com [exec de contentor az][az-container-exec], e não poder passar os argumentos do comando. Por exemplo, não pode estar encadeados comandos, como no `sh -c "echo FOO && echo BAR"`, pode iniciar nem executar `echo FOO`.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre outras ferramentas de resolução de problemas e problemas comuns de implementação no [resolver problemas de implementação e contentor em instâncias de contentor do Azure](container-instances-troubleshooting.md).

<!-- LINKS - internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-exec]: /cli/azure/container#az-container-exec
[azure-cli]: /cli/azure