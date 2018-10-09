---
title: Automatizar a criação da imagem, teste e patch com tarefas do Azure Container Registry com vários passos
description: Um tarefas de vários passos de introdução, uma funcionalidade do ACR tarefas no Azure Container Registry fornece fluxos de trabalho baseados em tarefas para criar, testar e aplicar patches em imagens de contentor na cloud.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 09/24/2018
ms.author: danlep
ms.openlocfilehash: cdabafc4f70b08076820e7e0d39300b3eb0bc1e7
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/08/2018
ms.locfileid: "48856723"
---
# <a name="run-multi-step-build-test-and-patch-tasks-in-acr-tasks"></a>Execução de vários passo compilação, teste e as tarefas de patch no ACR tarefas

Tarefas de vários passos expandir a capacidade de compilação de push e de imagem única das tarefas de ACR com fluxos de trabalho com vários passos, baseado no várias container. Utilizar tarefas de vários passos para criar e enviar várias imagens, série ou paralelamente e executar essas imagens como comandos dentro de uma única tarefa executar. Cada passo define uma imagem de contentor compilar ou enviar a operação e também pode definir a execução de um contentor. Cada passo numa tarefa com vários passo utiliza um contentor, como o seu ambiente de execução.

> [!IMPORTANT]
> Se tiver criado anteriormente tarefas durante a pré-visualização com o `az acr build-task` comando, essas tarefas têm de ser recriado com o [tarefa do az acr] [ az-acr-task] comando.

Por exemplo, pode executar uma tarefa com os passos que automatizam o seguinte:

1. Crie uma imagem de aplicação web
1. Executar o contentor da aplicação web
1. Crie uma imagem de teste de aplicações web
1. Executar o contentor de teste de aplicativos web que executa os testes em relação a execução do contentor da aplicação
1. Se os testes forem bem-sucedidos, criar um pacote de arquivo do gráfico Helm
1. Executar um `helm upgrade` usando o novo pacote de arquivo do gráfico Helm

Todos os passos são executados no Azure, descarregar o trabalho para recursos de computação do Azure e libera da gestão da infraestrutura. Para além do seu registo de contentor do Azure, paga apenas pelos recursos que utilizar. Para obter informações sobre os preços, consulte a **criar o contentor** secção [preços do Azure Container Registry][pricing].

> [!IMPORTANT]
> Esta funcionalidade encontra-se em pré-visualização. As pré-visualizações ser-lhe-ão disponibilizadas na condição de concordar com os [termos suplementares de utilização][terms-of-use]. Alguns aspetos desta funcionalidade podem alterar-se após a disponibilidade geral (GA).

## <a name="common-task-scenarios"></a>Cenários comuns de tarefas

Tarefas de vários passos ativar cenários como o seguinte:

* Crie, marca e enviar por push uma ou mais imagens de contentor, em série ou paralelamente.
* Execute e capturar os resultados de cobertura de código e teste de unidade.
* Execute e capturar testes funcionais. Tarefas de ACR suporta a execução de vários contentores, execução de uma série de pedidos entre eles.
* Execute tarefas com base em execução, incluindo os passos de pré/pós de uma compilação de imagem de contentor.
* Implemente um ou mais contentores com o motor de implementação favorito para o seu ambiente de destino.

## <a name="multi-step-task-definition"></a>Definição de tarefa de vários passos

Uma tarefa de várias etapa nas tarefas do ACR é definida como uma série de passos dentro de um ficheiro YAML. Cada etapa pode especificar as dependências a conclusão com êxito de um ou mais passos anteriores. Os seguintes tipos de passo de tarefa estão disponíveis:

* [`build`](container-registry-tasks-reference-yaml.md#build): Criar um ou mais imagens de contentor com o familiar `docker build` sintaxe, em série ou paralelamente.
* [`push`](container-registry-tasks-reference-yaml.md#push): Push criadas imagens para um registo de contentor. São suportados registos privados, como o Azure Container Registry, como é o Docker Hub público.
* [`cmd`](container-registry-tasks-reference-yaml.md#cmd): Execute um contentor, que ele pode operar como uma função dentro do contexto da tarefa em execução. Pode passar parâmetros para o contentor `[ENTRYPOINT]`e especifique propriedades como env, desligar e outro familiar `docker run` parâmetros. O `cmd` permite que o tipo de passo de unidade e testes funcionais, com a execução de contentor em simultâneo.

Tarefas de vários passos podem ser tão simples quanto a criação e envio de uma única imagem:

```yaml
version: 1.0-preview-1
steps:
  - build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} .
  - push: ["{{.Run.Registry}}/hello-world:{{.Run.ID}}"]
```

Ou, mais complexo, por exemplo, esta tarefa, que inclui passos para a compilação, teste, o pacote do helm e o helm implementar:

```yaml
version: 1.0-preview-1
steps:
  - id: build-web
    build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} .
    when: ["-"]
  - id: build-tests
    build -t {{.Run.Registry}}/hello-world-tests ./funcTests
    when: ["-"]
  - id: push
    push: ["{{.Run.Registry}}/helloworld:{{.Run.ID}}"]
    when: ["build-web", "build-tests"]
  - id: hello-world-web
    cmd: {{.Run.Registry}}/helloworld:{{.Run.ID}}
  - id: funcTests
    cmd: {{.Run.Registry}}/helloworld:{{.Run.ID}}
    env: ["host=helloworld:80"]
  - cmd: {{.Run.Registry}}/functions/helm package --app-version {{.Run.ID}} -d ./helm ./helm/helloworld/
  - cmd: {{.Run.Registry}}/functions/helm upgrade helloworld ./helm/helloworld/ --reuse-values --set helloworld.image={{.Run.Registry}}/helloworld:{{.Run.ID}}
```

## <a name="run-a-sample-task"></a>Executar uma tarefa de exemplo

Tarefas suportam ambos os execução manual, chamado "verificada rapidamente," e atualizar a execução automática numa imagem de consolidação ou a base do Git.

Para executar uma tarefa, primeiro definir passos da tarefa num ficheiro YAML, em seguida, execute o comando da CLI do Azure [acr az execute][az-acr-run].

Eis um exemplo de comando da CLI do Azure que executa uma tarefa através de um ficheiro YAML de tarefas de exemplo. Os passos criar e, em seguida, enviar uma imagem. Atualização `\<acrName\>` com o nome do seu próprio registo de contentor do Azure antes de executar o comando.

```azurecli
az acr run --registry <acrName> -f build-push-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

Quando executar a tarefa, a saída deve mostrar o progresso de cada passo definido no ficheiro YAML. Na seguinte saída, os passos são apresentados como `acb_step_0` e `acb_step_1`.

```console
$ az acr run --registry myregistry -f build-push-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
Sending context to registry: myregistry...
Queued a run with ID: yd14
Waiting for an agent...
2018/09/12 20:08:44 Using acb_vol_0467fe58-f6ab-4dbd-a022-1bb487366941 as the home volume
2018/09/12 20:08:44 Creating Docker network: acb_default_network
2018/09/12 20:08:44 Successfully set up Docker network: acb_default_network
2018/09/12 20:08:44 Setting up Docker configuration...
2018/09/12 20:08:45 Successfully set up Docker configuration
2018/09/12 20:08:45 Logging in to registry: myregistry.azurecr-test.io
2018/09/12 20:08:46 Successfully logged in
2018/09/12 20:08:46 Executing step: acb_step_0
2018/09/12 20:08:46 Obtaining source code and scanning for dependencies...
2018/09/12 20:08:47 Successfully obtained source code and scanned for dependencies
Sending build context to Docker daemon  109.6kB
Step 1/1 : FROM hello-world
 ---> 4ab4c602aa5e
Successfully built 4ab4c602aa5e
Successfully tagged myregistry.azurecr-test.io/hello-world:yd14
2018/09/12 20:08:48 Executing step: acb_step_1
2018/09/12 20:08:48 Pushing image: myregistry.azurecr-test.io/hello-world:yd14, attempt 1
The push refers to repository [myregistry.azurecr-test.io/hello-world]
428c97da766c: Preparing
428c97da766c: Layer already exists
yd14: digest: sha256:1a6fd470b9ce10849be79e99529a88371dff60c60aab424c077007f6979b4812 size: 524
2018/09/12 20:08:55 Successfully pushed image: myregistry.azurecr-test.io/hello-world:yd14
2018/09/12 20:08:55 Step id: acb_step_0 marked as successful (elapsed time in seconds: 2.035049)
2018/09/12 20:08:55 Populating digests for step id: acb_step_0...
2018/09/12 20:08:57 Successfully populated digests for step id: acb_step_0
2018/09/12 20:08:57 Step id: acb_step_1 marked as successful (elapsed time in seconds: 6.832391)
The following dependencies were found:
- image:
    registry: myregistry.azurecr-test.io
    repository: hello-world
    tag: yd14
    digest: sha256:1a6fd470b9ce10849be79e99529a88371dff60c60aab424c077007f6979b4812
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/hello-world
    tag: latest
    digest: sha256:0add3ace90ecb4adbf7777e9aacf18357296e799f81cabc9fde470971e499788
  git: {}


Run ID: yd14 was successful after 19s
```

Para obter mais informações sobre as compilações automáticas em atualização de imagem de consolidação ou a base do Git, consulte a [automatizar compilações de imagem](container-registry-tutorial-build-task.md) e [compilações de atualização da imagem de Base](container-registry-tutorial-base-image-update.md) artigos de tutorial.

## <a name="preview-feedback"></a>Comentários de pré-visualização

Embora a funcionalidade de tarefas de várias etapas das tarefas de ACR está em pré-visualização, convidamos a fornecer comentários. Estão disponíveis vários canais de comentários:

* [Problemas](https://aka.ms/acr/issues) - ver problemas e erros existentes e novas etiquetas de registo
* [UserVoice](https://aka.ms/acr/uservoice) -votar em existente pedidos de funcionalidades ou criar novos pedidos
* [Discutir](https://aka.ms/acr/feedback) -participar de discussão do Azure Container Registry com a Comunidade do Stack Overflow

## <a name="next-steps"></a>Passos Seguintes

Pode encontrar a referência de tarefas de vários passos e exemplos aqui:

* [Referência de tarefas](container-registry-tasks-reference-yaml.md) -passo tipos, suas propriedades e da utilização de tarefas.
* [Exemplos de tarefas] [ task-examples] -exemplo `task.yaml` arquivos para vários cenários simples a complexas.

<!-- IMAGES -->

<!-- LINKS - External -->
[pricing]: https://azure.microsoft.com/pricing/details/container-registry/
[task-examples]: https://github.com/Azure-Samples/acr-tasks
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-run]: /cli/azure/acr/run#az-acr-run
[az-acr-task]: /cli/azure/acr#az-acr-task