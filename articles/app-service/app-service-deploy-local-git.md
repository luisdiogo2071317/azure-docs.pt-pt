---
title: Implementação de Git Local no Serviço de Aplicações do Azure
description: Saiba como ativar a implementação de Git local para o App Service do Azure.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
ms.assetid: ac50a623-c4b8-4dfd-96b2-a09420770063
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2018
ms.author: dariagrigoriu;cephalin
ms.openlocfilehash: a614dadae40fcfc28eba85e5943f60a38653224b
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35233908"
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

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="deploy-from-local-git-with-kudu-builds"></a>Implementar a partir de Git local com compilações do Kudu

A forma mais fácil para ativar a implementação de Git local para a sua aplicação com o servidor de compilação do Kudu consiste em utilizar a Shell de nuvem.

### <a name="create-a-deployment-user"></a>Criar um utilizador de implementação

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="enable-local-git-with-kudu"></a>Ativar o Git local com o Kudu

Para ativar a implementação de Git local para a sua aplicação com o servidor de compilação do Kudu, execute [ `az webapp deployment source config-local-git` ](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az_webapp_deployment_source_config_local_git) na Shell de nuvem.

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

### <a name="deploy-your-project"></a>Implementar o projeto

Regresse à _janela de terminal local_ e adicione um remoto do Azure ao seu repositório Git local. Substitua  _\<url >_ com o URL do Git remoto que recebeu do [ativar o Git para a sua aplicação](#enable-git-for-you-app).

```bash
git remote add azure <url>
```

Envie para o remoto do Azure para implementar a sua aplicação com o comando seguinte. Quando lhe for pedida uma palavra-passe, confirme que introduz a palavra-passe que criou em [Configurar um utilizador de implementação](#configure-a-deployment-user) e não a que utilizou para iniciar sessão no portal do Azure.

```bash
git push azure master
```

Poderá ver automatização específico de tempo de execução na saída, tais como MSBuild para ASP.NET, `npm install` para Node.js, e `pip install` para Python. 

Navegue até à sua aplicação para verificar que o conteúdo está implementado.

## <a name="deploy-from-local-git-with-vsts-builds"></a>Implementar a partir de Git local com VSTS compilações

> [!NOTE]
> Para o App Service criar a compilação necessária e as definições na sua conta VSTS de versão, a conta do Azure tem de ter a função de **proprietário** na sua subscrição do Azure.
>

Para ativar a implementação de Git local para a sua aplicação com o servidor de compilação do Kudu, navegue até à sua aplicação no [portal do Azure](https://portal.azure.com).

No painel de navegação esquerdo da sua página de aplicação, clique em **Centro de implementação** > **Local Git** > **continuar**. 

![](media/app-service-deploy-local-git/portal-enable.png)

Clique em **entrega contínua VSTS** > **continuar**.

![](media/app-service-deploy-local-git/vsts-build-server.png)

No **configurar** página, configurar uma nova conta VSTS ou especificar uma conta existente. Quando terminar, clique em **continuar**.

> [!NOTE]
> Se pretender utilizar uma conta VSTS existente que não esteja listada, é necessário [associar a conta VSTS à sua subscrição do Azure](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

No **teste** página, escolha se pretende ativar os testes de carga, em seguida, clique em **continuar**.

Consoante o [escalão de preço](/pricing/details/app-service/plans/) do seu plano de serviço de aplicações, também poderá ver um **implementar para transição** página. Escolha se pretende ativar as ranhuras de implementação, em seguida, clique em **continuar**.

No **resumo** página, verifique as suas opções e clique em **concluir**.

Demora alguns minutos para que a conta VSTS estar preparado. Quando estiver pronto, copie o URL do repositório de Git no Centro de implementação.

![](media/app-service-deploy-local-git/vsts-repo-ready.png)

Regresse à _janela de terminal local_ e adicione um remoto do Azure ao seu repositório Git local. Substitua  _\<url >_ com o URL recebeu do último passo.

```bash
git remote add vsts <url>
```

Envie para o remoto do Azure para implementar a sua aplicação com o comando seguinte. Quando lhe for pedido pelo Gestor de credenciais do Git, inicie sessão com o seu utilizador visualstudio.com. Para métodos de autenticação adicionais, consulte [descrição geral de authenticaiton VSTS](/vsts/git/auth-overview?view=vsts).

```bash
git push vsts master
```

Assim que a implementação estiver concluída, pode encontrar o progresso de compilação no `https://<vsts_account>.visualstudio.com/<project_name>/_build` e o progresso da implementação em `https://<vsts_account>.visualstudio.com/<project_name>/_release`.

Navegue até à sua aplicação para verificar que o conteúdo está implementado.

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="troubleshooting-kudu-deployment"></a>Resolução de problemas de implementação do Kudu

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
* **Módulo nativo que não tem uma distribuição binária para o Windows**:

  * `npm ERR! \cmd "/c" "node-gyp rebuild"\ failed with 1`

      OU
  * `npm ERR! [modulename@version] preinstall: \make || gmake\`

## <a name="additional-resources"></a>Recursos Adicionais

* [Documentação do Kudu projeto](https://github.com/projectkudu/kudu/wiki)
* [Implementação contínua do serviço de aplicações do Azure](app-service-continuous-deployment.md)
* [Exemplo: Criar a aplicação Web e implementar código a partir de um repositório de Git local (CLI do Azure)](./scripts/app-service-cli-deploy-local-git.md?toc=%2fcli%2fazure%2ftoc.json)
* [Exemplo: Criar a aplicação Web e implementar código a partir de um repositório de Git local (PowerShell)](./scripts/app-service-powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json)
