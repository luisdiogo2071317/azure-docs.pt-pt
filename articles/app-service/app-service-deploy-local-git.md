---
title: "Implementação de Git Local no Serviço de Aplicações do Azure"
description: "Saiba como ativar a implementação de Git local para o App Service do Azure."
services: app-service
documentationcenter: 
author: cephalin
manager: cfowler
ms.assetid: ac50a623-c4b8-4dfd-96b2-a09420770063
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/05/2018
ms.author: dariagrigoriu;cephalin
ms.openlocfilehash: 4cbe26055bdbf906223a327ab8cf94bebe9e7998
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="local-git-deployment-to-azure-app-service"></a>Implementação de Git Local no Serviço de Aplicações do Azure

Este guia de procedimentos mostra-lhe como implementar o código para [App Service do Azure](app-service-web-overview.md) um repositório de Git no seu computador local.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

A seguir os passos neste guia de procedimentos:

* [Instalar o Git](http://www.git-scm.com/downloads).
* Manter um repositório de Git local com o código que pretende implementar.

Para utilizar um repositório de exemplo para acompanhar, execute o seguinte comando na janela de terminal do local:

```bash
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world.git
```

## <a name="prepare-your-repository"></a>Preparar o seu repositório

Certifique-se de que a raiz do repositório tem os ficheiros corretos no seu projeto.

| Runtime | Ficheiros do diretório de raiz |
|-|-|
| ASP.NET (apenas Windows) | _*.sln_, _*.csproj_, ou _default.aspx_ |
| Núcleo de ASP.NET | _*.sln_ or _*.csproj_ |
| PHP | _index.php_ |
| Ruby (apenas Linux) | _Gemfile_ |
| Node.js | _Server.js_, _app.js_, ou _Package. JSON_ com um script de início |
| Python (apenas Windows) | _\*. PY_, _requirements.txt_, ou _runtime.txt_ |
| HTML | _default.htm_, _default.html_, _default.asp_, _index.htm_, _index.html_, ou  _iisstart.htm_ |
| Trabalhos Web | _\<job_name > / executar. \<extensão >_ em _aplicação\_dados/tarefas/contínua_ (para WebJobs contínuos) ou _aplicação\_dados/tarefas/acionada_ (para acionada WebJobs). Para obter mais informações, consulte [documentação de WebJobs do Kudu](https://github.com/projectkudu/kudu/wiki/WebJobs) |
| Funções | Consulte [a implementação contínua para as funções do Azure](../azure-functions/functions-continuous-deployment.md#continuous-deployment-requirements). |

Para personalizar a sua implementação, pode incluir um _.deployment_ ficheiro na raiz do repositório. Para obter mais informações, consulte [personalizar implementações](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) e [script de implementação personalizada](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).

> [!NOTE]
> Certifique-se para `git commit` todas as alterações que pretende implementar.
>
>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user.md)]

## <a name="enable-git-for-your-app"></a>Ativar o Git para a sua aplicação

Para ativar a implementação de Git para uma aplicação de serviço de aplicações existente, execute [ `az webapp deployment source config-local-git` ](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az_webapp_deployment_source_config_local_git) na Shell de nuvem.

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group <group_name>
```

Para criar uma aplicação com o Git em vez disso, execute [ `az webapp create` ](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) na Shell do Cloud com o `--deployment-local-git` parâmetro.

```azurecli-interactive
az webapp create --name <app_name> --resource-group <group_name> --plan <plan_name> --deployment-local-git
```

O `az webapp create` comando deverá dar-lhe algo semelhante ao seguinte saída:

```json
Local git is configured with url of 'https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

## <a name="deploy-your-project"></a>Implementar o projeto

Regresse à _janela de terminal local_ e adicione um remoto do Azure ao seu repositório Git local. Substitua  _\<url >_ com o URL do Git remoto que recebeu do [ativar o Git para a sua aplicação](#enable-git-for-you-app).

```bash
git remote add azure <url>
```

Envie para o remoto do Azure para implementar a sua aplicação com o comando seguinte. Quando lhe for pedida uma palavra-passe, confirme que introduz a palavra-passe que criou em [Configurar um utilizador de implementação](#configure-a-deployment-user) e não a que utilizou para iniciar sessão no portal do Azure.

```bash
git push azure master
```

Poderá ver automatização específico de tempo de execução na saída, tais como MSBuild para ASP.NET, `npm install` para Node.js, e `pip install` para Python. 

Após a conclusão da implementação, a aplicação no portal do Azure agora deve ter um registo de sua `git push` no **opções de implementação** página.

![](./media/app-service-deploy-local-git/deployment_history.png)

Navegue até à sua aplicação para verificar que o conteúdo está implementado.

## <a name="troubleshooting"></a>Resolução de problemas

Seguem-se os erros ou problemas comuns quando utilizar o Git para publicar uma aplicação de serviço de aplicações no Azure:

---
**Sintoma**: `Unable to access '[siteURL]': Failed to connect to [scmAddress]`

**Causa**: Este erro pode acontecer se a aplicação não está em execução.

**Resolução**: iniciar a aplicação no portal do Azure. Implementação de Git não está disponível quando a aplicação Web está parada.

---
**Sintoma**: `Couldn't resolve host 'hostname'`

**Causa**: Este erro pode acontecer se as informações de endereço que introduziu durante a criação do azure remoto estavam incorretas.

**Resolução**: Utilize o `git remote -v` comando para listar todos os remotes, juntamente com o URL associado. Certifique-se de que o URL para 'azure' remoto está correto. Se for necessário, remova e recrie esta remoto utilizando o URL correto.

---
**Sintoma**: `No refs in common and none specified; doing nothing. Perhaps you should specify a branch such as 'master'.`

**Causa**: Este erro pode acontecer se não especificar um ramo durante `git push`, ou se ainda não configurou o `push.default` valor `.gitconfig`.

**Resolução**: executar `git push` novamente, especificando o ramo principal. Por exemplo:

```bash
git push azure master
```

---
**Sintoma**: `src refspec [branchname] does not match any.`

**Causa**: Este erro pode ocorrer se tentar ativar para um ramo diferente mestre do azure remoto.

**Resolução**: executar `git push` novamente, especificando o ramo principal. Por exemplo:

```bash
git push azure master
```

---
**Sintoma**: `RPC failed; result=22, HTTP code = 5xx.`

**Causa**: Este erro pode ocorrer se tentar push um repositório de git grande através de HTTPS.

**Resolução**: alterar a configuração do git no computador local para tornar o postBuffer superior

```bash
git config --global http.postBuffer 524288000
```

---
**Sintoma**: `Error - Changes committed to remote repository but your web app not updated.`

**Causa**: Este erro pode acontecer se implementar uma aplicação Node.js com um _Package. JSON_ ficheiro que especifica os módulos necessários adicionais.

**Resolução**: mensagens adicionais com 'npm ERR'! devem ser registados antes deste erro e pode fornecer contexto adicional sobre a falha. Os seguintes são conhecidos causas deste erro e a correspondente 'npm ERR!' mensagem:

* **Ficheiro Package. JSON com formato incorreto**: npm ERR! Não foi possível ler as dependências.
* **Módulo nativo que não tenha uma distribuição binária para o Windows**:

  * `npm ERR! \cmd "/c" "node-gyp rebuild"\ failed with 1`

      OU
  * `npm ERR! [modulename@version] preinstall: \make || gmake\`

## <a name="additional-resources"></a>Recursos Adicionais

* [Documentação do Kudu projeto](https://github.com/projectkudu/kudu/wiki)
* [Implementação contínua do serviço de aplicações do Azure](app-service-continuous-deployment.md)
