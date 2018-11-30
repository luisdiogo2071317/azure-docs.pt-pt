---
title: Referência de tarefas de registo de contentor do Azure - YAML
description: Referência para definir tarefas em YAML para tarefas de ACR, incluindo propriedades da tarefa, tipos de passo, propriedades do passo e variáveis incorporadas.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 11/13/2018
ms.author: danlep
ms.openlocfilehash: e91b4e881c0f39304e3042d556f111db2089f7de
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/27/2018
ms.locfileid: "52334487"
---
# <a name="acr-tasks-reference-yaml"></a>Referência de tarefas do ACR: YAML

Definição de tarefa de várias etapas nas tarefas de ACR fornece um primitivo de centrada no contêiner de computação focado na criação, teste e contentores de aplicação de patches. Este artigo abrange os comandos, parâmetros, propriedades e sintaxe para os ficheiros YAML que definem suas tarefas de vários passos.

Este artigo contém uma referência para a criação de arquivos YAML de tarefas de vários passos para tarefas do ACR. Se desejar uma introdução às tarefas do ACR, consulte a [descrição geral de tarefas de ACR](container-registry-tasks-overview.md).

> [!IMPORTANT]
> A funcionalidade de tarefas de várias etapas das tarefas de ACR está atualmente em pré-visualização. As pré-visualizações ser-lhe-ão disponibilizadas na condição de concordar com os [termos suplementares de utilização][terms-of-use]. Alguns aspetos desta funcionalidade podem alterar-se após a disponibilidade geral (GA).

## <a name="acr-taskyaml-file-format"></a>formato de ficheiro ACR task.yaml

Tarefas de ACR oferece suporte a declaração de tarefa de várias etapas na sintaxe YAML padrão. Definir os passos de uma tarefa num ficheiro YAML, em seguida, pode executar manualmente, ou ter acionado automaticamente na atualização de imagem de consolidação ou a base do Git. Embora este artigo refere-se ao `acr-task.yaml` como o ficheiro que contém os passos, tarefas de ACR suporta qualquer nome de ficheiro válido com um [suportado extensão](#supported-task-filename-extensions).

O nível superior `acr-task.yaml` primitivos são **propriedades da tarefa**, **passo tipos**, e **passo propriedades**:

* [Propriedades da tarefa](#task-properties) aplicam-se a todos os passos em toda a execução da tarefa. Existem três propriedades de tarefa global:
  * versão
  * stepTimeout
  * totalTimeout
* [Passo tipos de tarefas](#task-step-types) representam os tipos de ações que podem ser executadas numa tarefa. Existem três tipos de passo:
  * Compilação
  * Push
  * cmd
* [Propriedades do passo da tarefa](#task-step-properties) são parâmetros que se aplicam a um passo individual. Existem várias propriedades do passo, incluindo:
  * startDelay
  * tempo limite
  * quando
  * ... e muito mais.

O formato de base de um `acr-task.yaml` ficheiro, incluindo algumas propriedades comuns do passo, segue-se. Ao não uma representação completa de todas as propriedades do passo disponíveis ou utilização de tipo de passo, ele fornece uma rápida visão geral do formato de arquivo básico.

```yaml
version: # acr-task.yaml format version.
stepTimeout: # Seconds each step may take.
totalTimeout: # Total seconds allowed for all steps to complete.
steps: # A collection of image or container actions.
    build: # Equivalent to "docker build," but in a multi-tenant environment
    push: # Push a newly built or retagged image to a registry.
      when: # Step property that defines either parallel or dependent step execution.
    cmd: # Executes a container, supports specifying an [ENTRYPOINT] and parameters.
      startDelay: # Step property that specifies the number of seconds to wait before starting execution.
```

### <a name="supported-task-filename-extensions"></a>Extensões de nome de ficheiro de tarefa suportados

Tarefas de ACR reservou várias extensões de nome de ficheiro, incluindo `.yaml`, que irá processar como um ficheiro de tarefa. Qualquer extensão *não* na lista seguinte é considerado pelas tarefas de ACR um Dockerfile: .yaml,. yml, .toml,. JSON,. SH, .bash, .zsh,. ps1, .ps,. cmd,. bat,. TS,. js, PHP,. PY,. RB, .lua

YAML é o único formato de ficheiro suportado atualmente por tarefas do ACR. Outras extensões de nome de ficheiro estão reservados para eventual suporte futuros.

## <a name="run-the-sample-tasks"></a>Executar as tarefas de exemplo

Existem vários ficheiros de tarefas de exemplo referenciados nas secções seguintes deste artigo. As tarefas de exemplo estão num repositório público do GitHub [Azure-amostras/acr-tarefas][acr-tasks]. Pode executá-los com o comando da CLI do Azure [acr az execute][az-acr-run]. Os comandos de exemplo são semelhantes a:

```azurecli
az acr run -f build-push-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

A formatação dos comandos de exemplo parte do princípio de que configurou um registro padrão na CLI do Azure, para que eles omitir o `--registry` parâmetro. Para configurar um registro padrão, utilize o [az configure] [ az-configure] comando com o `--defaults` parâmetro, que aceita um `acr=REGISTRY_NAME` valor.

Por exemplo, para configurar a CLI do Azure com um registro padrão chamada "myregistry":

```azurecli
az configure --defaults acr=myregistry
```

## <a name="task-properties"></a>Propriedades da tarefa

Propriedades da tarefa, normalmente, são apresentados na parte superior de um `acr-task.yaml` de ficheiros e são propriedades globais que se aplicam durante toda a execução completa da tarefa. Algumas dessas propriedades global podem ser substituídas dentro de um passo individual.

| Propriedade | Tipo | Opcional | Descrição | Suportada de substituição | Valor predefinido |
| -------- | ---- | -------- | ----------- | ------------------ | ------------- |
| `version` | cadeia | Não | A versão do `acr-task.yaml` de ficheiros como analisado pelo serviço de tarefas do ACR. Enquanto as tarefas de ACR tenta manter a compatibilidade com versões anteriores, este valor permite que tarefas de ACR manter a compatibilidade dentro de uma versão definida. | Não | Nenhuma |
| `stepTimeout` | Int (segundos) | Sim | O número máximo de segundos que pode executar uma etapa. Esta propriedade pode ser substituída numa etapa ao definir a etapa [tempo limite](#timeout) propriedade. | Sim | 600 (10 minutos) |
| `totalTimeout` | Int (segundos) | Sim | O número máximo de segundos que uma tarefa pode ser executada. "Executar" inclui a execução e a conclusão de todos os passos na tarefa, se com êxito ou falha. Também inclui tarefas de impressão o resultado como dependências de imagem detetadas e o estado de execução da tarefa. | Não | 3600 (1 hora) |

## <a name="task-step-types"></a>Tipos de passo de tarefa

Tarefas de ACR suporta três tipos de passo. Cada tipo de passo suporta várias propriedades, detalhadas na secção para cada tipo de passo.

| Tipo de passo | Descrição |
| --------- | ----------- |
| [`build`](#build) | Cria uma imagem de contentor através de familiar `docker build` sintaxe. |
| [`push`](#push) | Executa um `docker push` de recentemente criadas, ou retagged imagens para um registo de contentor. O Azure Container Registry, outros registos privados e o Docker Hub público são suportadas.
| [`cmd`](#cmd) | Execuções de um contentor como um comando, com parâmetros passado para o contentor `[ENTRYPOINT]`. O `cmd` tipo de passo suporta parâmetros como env e desanexar e outro familiar `docker run` as opções, permitindo que a unidade e testes funcionais com execução em simultâneo de contentor do comando. |

## <a name="build"></a>Compilação

Crie uma imagem de contentor. O `build` tipo de passo representa um meio seguro e de multi-inquilino de execução `docker build` na cloud como um primitivo de primeira classe.

### <a name="syntax-build"></a>Sintaxe: criar

```yaml
version: 1.0-preview-1
steps:
    - [build]: -t [imageName]:[tag] -f [Dockerfile] [context]
      [property]: [value]
```

O `build` tipo de passo suporta os parâmetros na tabela seguinte. O `build` tipo de passo também oferece suporte a todas as opções de compilação do [compilação do docker](https://docs.docker.com/engine/reference/commandline/build/) comando, tal como `--build-arg` para definir as variáveis de tempo de compilação.

| Parâmetro | Descrição | Opcional |
| --------- | ----------- | :-------: |
| `-t` &#124; `--image` | Define o completamente qualificado `image:tag` da imagem incorporada.<br /><br />Como as imagens podem ser utilizadas para validações de tarefas interna, como testes funcionais, nem todas as imagens exigem `push` para um registo. No entanto, para uma imagem dentro de uma execução de tarefa de instância, a imagem tem um nome para fazer referência.<br /><br />Ao contrário de `az acr build`, execução de tarefas do ACR não fornece o comportamento de push padrão. Com tarefas do ACR, o cenário de predefinição pressupõe que a capacidade de criar, validar, em seguida, enviar uma imagem. Ver [push](#push) para saber como opcionalmente as imagens de push incorporado. | Sim |
| `-f` &#124; `--file` | Especifica o Dockerfile passado para `docker build`. Se não for especificado, a predefinição Dockerfile na raiz do contexto é assumido. Para especificar uma alternativa Dockerfile, passe o nome de ficheiro relativo à raiz do contexto. | Sim |
| `context` | O diretório de raiz passado para `docker build`. O diretório de raiz de cada tarefa está definido para um partilhada [workingDirectory](#task-step-properties)e inclui a raiz do diretório clonado Git associado. | Não |

### <a name="properties-build"></a>Propriedades: criar

O `build` tipo de passo suporta as seguintes propriedades. Pode encontrar os detalhes destas propriedades no [propriedades do passo de tarefa](#task-step-properties) seção deste artigo.

| | | |
| -------- | ---- | -------- |
| `detach` | Bool | Opcional |
| `entryPoint` | cadeia | Opcional |
| `env` | [string, string,...] | Opcional |
| `id` | cadeia | Opcional |
| `ignoreErrors` | Bool | Opcional |
| `keep` | Bool | Opcional |
| `startDelay` | Int (segundos) | Opcional |
| `timeout` | Int (segundos) | Opcional |
| `when` | [string, string,...] | Opcional |
| `workingDirectory` | cadeia | Opcional |

### <a name="examples-build"></a>Exemplos: criar

#### <a name="build-image---context-in-root"></a>Criar imagem - contexto na raiz

```azurecli
az acr run -f build-hello-world.yaml https://github.com/AzureCR/acr-tasks-sample.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/build-hello-world.yaml --> [!code-yml[task](~/acr-tasks/build-hello-world.yaml)]

#### <a name="build-image---context-in-subdirectory"></a>Criar imagem - contexto no subdiretório

```yaml
version: 1.0-preview-1
steps:
- build: -t {{.Run.Registry}}/hello-world -f hello-world.dockerfile ./subDirectory
```

## <a name="push"></a>Push

Push um ou mais criados ou retagged imagens para um registo de contentor. Suporta o envio de registos privados, como o Azure Container Registry ou o Docker Hub público.

### <a name="syntax-push"></a>Sintaxe: push

O `push` suporta o tipo de passo uma coleção de imagens. Sintaxe de coleção de YAML suporta formatos aninhados e inline. Envio de uma única imagem normalmente é representado através de sintaxe de inline:

```yaml
version: 1.0-preview-1
steps:
  # Inline YAML collection syntax
  - push: ["{{.Run.Registry}}/hello-world:{{.Run.ID}}"]
```

Para maior legibilidade, utilize sintaxe aninhada quando enviar várias imagens:

```yaml
version: 1.0-preview-1
steps:
  # Nested YAML collection syntax
  - push:
    - {{.Run.Registry}}/hello-world:{{.Run.ID}}
    - {{.Run.Registry}}/hello-world:latest
```

### <a name="properties-push"></a>Propriedades: push

O `push` tipo de passo suporta as seguintes propriedades. Pode encontrar os detalhes destas propriedades no [propriedades do passo de tarefa](#task-step-properties) seção deste artigo.

| | | |
| -------- | ---- | -------- |
| `env` | [string, string,...] | Opcional |
| `id` | cadeia | Opcional |
| `ignoreErrors` | Bool | Opcional |
| `startDelay` | Int (segundos) | Opcional |
| `timeout` | Int (segundos) | Opcional |
| `when` | [string, string,...] | Opcional |

### <a name="examples-push"></a>Exemplos: push

#### <a name="push-multiple-images"></a>Push várias imagens

```azurecli
az acr run -f build-push-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/build-push-hello-world.yaml --> [!code-yml[task](~/acr-tasks/build-push-hello-world.yaml)]

#### <a name="build-push-and-run"></a>Criar, push e executar

```azurecli
az acr run -f build-run-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/build-run-hello-world.yaml --> [!code-yml[task](~/acr-tasks/build-run-hello-world.yaml)]

## <a name="cmd"></a>cmd

O `cmd` tipo de passo é executado um contentor.

### <a name="syntax-cmd"></a>Sintaxe: cmd

```yaml
version: 1.0-preview-1
steps:
    - [cmd]: [containerImage]:[tag (optional)] [cmdParameters to the image]
```

### <a name="properties-cmd"></a>Propriedades: cmd

O `cmd` tipo de passo suporta as seguintes propriedades:

| | | |
| -------- | ---- | -------- |
| `detach` | Bool | Opcional |
| `entryPoint` | cadeia | Opcional |
| `env` | [string, string,...] | Opcional |
| `id` | cadeia | Opcional |
| `ignoreErrors` | Bool | Opcional |
| `keep` | Bool | Opcional |
| `startDelay` | Int (segundos) | Opcional |
| `timeout` | Int (segundos) | Opcional |
| `when` | [string, string,...] | Opcional |
| `workingDirectory` | cadeia | Opcional |

Pode encontrar os detalhes destas propriedades no [propriedades do passo de tarefa](#task-step-properties) seção deste artigo.

### <a name="examples-cmd"></a>Exemplos: cmd

#### <a name="run-hello-world-image"></a>Execute a imagem hello-world

Este comando executa o `hello-world.yaml` ficheiro de tarefa, o que faz referência a [hello-world](https://hub.docker.com/_/hello-world/) imagem no Docker Hub.

```azurecli
az acr run -f hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.yaml --> [!code-yml[task](~/acr-tasks/hello-world.yaml)]

#### <a name="run-bash-image-and-echo-hello-world"></a>Execute o bash imagem e eco "hello world"

Este comando executa o `bash-echo.yaml` ficheiro de tarefa, o que faz referência a [bash](https://hub.docker.com/_/bash/) imagem no Docker Hub.

```azurecli
az acr run -f bash-echo.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/bash-echo.yaml --> [!code-yml[task](~/acr-tasks/bash-echo.yaml)]

#### <a name="run-specific-bash-image-tag"></a>Execute o bash específico tag de imagem

Para executar uma versão de imagem específica, especifique a marca no `cmd`.

Este comando executa o `bash-echo-3.yaml` ficheiro de tarefa, o que faz referência a [bash: 3.0](https://hub.docker.com/_/bash/) imagem no Docker Hub.

```azurecli
az acr run -f bash-echo-3.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/bash-echo-3.yaml --> [!code-yml[task](~/acr-tasks/bash-echo-3.yaml)]

#### <a name="run-custom-images"></a>Executar imagens personalizadas

O `cmd` referencia o tipo de passo imagens usando o padrão `docker run` formato. Se originar de docker.io devem imagens não prefaciadas com um registo. O exemplo anterior igualmente pode ser representado como:

```yaml
version: 1.0-preview-1
steps:
    - cmd: docker.io/bash:3.0 echo hello world
```

Utilizando o padrão `docker run` Convenção de referência, de imagem `cmd` pode executar imagens que residem em qualquer registo privado ou o Docker Hub público. Se está a referenciar o imagens no mesmo registo em que a tarefa de ACR está em execução, não precisa de especificar quaisquer credenciais de registo.

* Executar uma imagem que residem num Azure container registry

    Substitua `[myregistry]` com o nome do seu registo:

    ```yaml
    version: 1.0-preview-1
    steps:
        - cmd: [myregistry].azurecr.io/bash:3.0 echo hello world
    ```

* Generalizar a referência de registro com uma variável de execução

    Em vez de embutir o nome do seu registo numa `acr-task.yaml` arquivo, pode torná-lo mais portátil, usando um [executar variável](#run-variables). O `Run.Registry` variável expande-se em tempo de execução para o nome do registo em que a tarefa está em execução.

    Para generalizar a tarefa anterior para que ele funcione em qualquer registo de contentor do Azure, fazer referência a [Run.Registry](#runregistry) variável no nome da imagem:

    ```yaml
    version: 1.0-preview-1
    steps:
        - cmd: {{.Run.Registry}}/bash:3.0 echo hello world
    ```

## <a name="task-step-properties"></a>Propriedades do passo de tarefa

Cada tipo de passo suporta várias propriedades adequadas para o respetivo tipo. A tabela seguinte define todas as propriedades do passo disponíveis. Nem todos os tipos de passo suportam todas as propriedades. Para ver qual destas propriedades estão disponíveis para cada tipo de passo, consulte a [cmd](#cmd), [crie](#build), e [push](#push) passo secções de referência de tipo.

| Propriedade | Tipo | Opcional | Descrição |
| -------- | ---- | -------- | ----------- |
| `detach` | Bool | Sim | Se o contêiner deve ser desligado quando em execução. |
| `entryPoint` | cadeia | Sim | Substitui o `[ENTRYPOINT]` do contentor de um passo. |
| `env` | [string, string,...] | Sim | Matriz de cadeias de caracteres em `key=value` formato que definem as variáveis de ambiente para o passo. |
| [`id`](#example-id) | cadeia | Sim | Identifica exclusivamente a etapa na tarefa. Outros passos da tarefa podem fazer referência a uma etapa `id`, por exemplo, para a dependência a verificar com `when`.<br /><br />O `id` também é o nome do contentor em execução. Processos em execução nos outros contentores na tarefa podem consultar o `id` como seu nome de anfitrião DNS, ou para aceder ao mesmo com os registos do docker [id], por exemplo. |
| `ignoreErrors` | Bool | Sim | Quando definido como `true`, a etapa é marcada como concluída, independentemente de se Ocorreu um erro durante sua execução. Predefinição: `false`. |
| `keep` | Bool | Sim | Se o contentor de um passo deve ser mantida após a execução. |
| `startDelay` | Int (segundos) | Sim | Número de segundos de execução de um passo de atraso. |
| `timeout` | Int (segundos) | Sim | Número máximo de segundos que um passo pode executar antes de a ser terminadas. |
| [`when`](#example-when) | [string, string,...] | Sim | Configura a dependência de um passo num ou mais outros passos na tarefa. |
| `workingDirectory` | cadeia | Sim | Define o diretório de trabalho de um passo. Por predefinição, as tarefas de ACR cria um diretório de raiz como o diretório de trabalho. No entanto, se sua compilação possui várias etapas, os passos anteriores podem partilhar artefactos com passos posteriores ao especificar o mesmo diretório de trabalho. |

### <a name="examples-task-step-properties"></a>Exemplos: Propriedades do passo de tarefa

#### <a name="example-id"></a>Exemplo: id

Crie duas imagens, criação de instância de uma imagem de teste funcional. Cada passo é identificado por um exclusivo `id` que fazem referência a outros passos da tarefa no seu `when` propriedade.

```azurecli
az acr run -f when-parallel-dependent.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel-dependent.yaml --> [!code-yml[task](~/acr-tasks/when-parallel-dependent.yaml)]

#### <a name="example-when"></a>Exemplo: quando

O `when` propriedade especifica a dependência de um passo em outros passos na tarefa. Ele oferece suporte a dois valores de parâmetro:

* `when: ["-"]` -Não indica a nenhuma dependência de outros passos. Especificar um passo `when: ["-"]` irá começar imediatamente a execução e permite a execução do passo em simultâneo.
* `when: ["id1", "id2"]` -Indica a etapa é dependente de passos com `id` "id1" e `id` "id2". Este passo não será executado até concluir os passos "id1" e "id2".

Se `when` não for especificado um passo, essa etapa é dependente da conclusão do passo anterior no `acr-task.yaml` ficheiro.

Execução do passo seqüencial sem `when`:

```azurecli
az acr run -f when-sequential-default.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-sequential-default.yaml --> [!code-yml[task](~/acr-tasks/when-sequential-default.yaml)]

Execução do passo seqüencial com `when`:

```azurecli
az acr run -f when-sequential-id.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-sequential-id.yaml --> [!code-yml[task](~/acr-tasks/when-sequential-id.yaml)]

Criação da imagem paralela:

```azurecli
az acr run -f when-parallel.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel.yaml --> [!code-yml[task](~/acr-tasks/when-parallel.yaml)]

Compilação de imagem e dependentes de teste em paralelo:

```azurecli
az acr run -f when-parallel-dependent.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel-dependent.yaml --> [!code-yml[task](~/acr-tasks/when-parallel-dependent.yaml)]

## <a name="run-variables"></a>Executar variáveis

Tarefas de ACR inclui um conjunto predefinido de variáveis que estão disponíveis para passos de tarefas quando eles são executados. Estas variáveis podem ser acedidas utilizando o formato `{{.Run.VariableName}}`, onde `VariableName` é um dos seguintes:

* `Run.ID`
* `Run.Registry`
* `Run.Date`

### <a name="run46id"></a>Execute&#46;ID

Cada execução, através de `az acr run`, ou a acionar a execução com base de tarefas criada por meio de `az acr task create` tem um ID exclusivo. O ID representa a execução atualmente a ser executada.

Normalmente utilizado para uma forma exclusiva de marcar uma imagem:

```yaml
version: 1.0-preview-1
steps:
    - build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} .
```

### <a name="runregistry"></a>Run.Registry

O nome de servidor completamente qualificado do Registro. Normalmente utilizado para referenciar genericamente o registo em que a tarefa está a ser executada.

```yaml
version: 1.0-preview-1
steps:
    - build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} .
```

### <a name="rundate"></a>Run.Date

A hora UTC atual a execução começou.

## <a name="next-steps"></a>Passos Seguintes

Para uma descrição geral das tarefas de vários passos, consulte a [executar vários passo compilação, teste e as tarefas de patch no ACR tarefas](container-registry-tasks-multi-step.md).

Para as compilações do passo a passo, consulte a [descrição geral de tarefas de ACR](container-registry-tasks-overview.md).

<!-- IMAGES -->

<!-- LINKS - External -->
[acr-tasks]: https://github.com/Azure-Samples/acr-tasks
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[az-acr-run]: /cli/azure/acr#az-acr-run
[az-configure]: /cli/azure/reference-index#az-configure