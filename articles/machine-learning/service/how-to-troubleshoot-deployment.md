---
title: Guia de resolução de problemas de implementação
titleSuffix: Azure Machine Learning service
description: Saiba como a solução, resolver e resolver problemas relacionados com os erros de implementação comuns do Docker com o AKS e ACI com o serviço Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: haining
author: hning86
ms.reviewer: jmartens
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 71b4cf5d44ec6cb3fb8b70975193320a4eabfc3f
ms.sourcegitcommit: c31a2dd686ea1b0824e7e695157adbc219d9074f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2019
ms.locfileid: "54401321"
---
# <a name="troubleshooting-azure-machine-learning-service-aks-and-aci-deployments"></a>Resolver problemas de implementações de AKS e ACI de serviço do Azure Machine Learning

Neste artigo, aprenderá a contornar ou resolver os erros de implementação comuns do Docker com o Azure Container Instances (ACI) e o Azure Kubernetes Service (AKS) com o serviço Azure Machine Learning.

Ao implementar um modelo no serviço Azure Machine Learning, o sistema executa um número de tarefas. Esta é uma sequência complexa de eventos e, às vezes, surgem problemas. As tarefas de implementação são:

1. Registe o modelo no registo do modelo de área de trabalho.

2. Criar uma imagem do Docker, incluindo:
    1. Baixe o modelo registado no registo. 
    2. Crie um dockerfile, com um ambiente de Python com base nas dependências que especificar no arquivo de yaml de ambiente.
    3. Adicione seus arquivos de modelo e o script de classificação que fornece no dockerfile.
    4. Crie uma nova imagem de Docker com o dockerfile.
    5. Registre-se a imagem do Docker com o Azure Container Registry associadas com a área de trabalho.

3. Implemente a imagem do Docker para o serviço de instância de contentor do Azure (ACI) ou para o Azure Kubernetes Service (AKS).

4. Inicie um novo contentor (ou contentores) no ACI ou AKS. 

Saiba mais sobre esse processo no [gestão de modelos](concept-model-management-and-deployment.md) introdução.

## <a name="before-you-begin"></a>Antes de começar

Caso se depare com algum problema, a primeira coisa a fazer é dividir a tarefa de implementação (descrito anterior) em etapas individuais para isolar o problema. 

Isso é particularmente útil se estiver a utilizar o `Webservice.deploy` API, ou `Webservice.deploy_from_model` API, uma vez que essas funções agrupar os passos mencionados anteriormente numa única ação. Normalmente, essas APIs são bastante convenientes, mas ajuda a dividir os passos quando a resolução de problemas substituí-los com o abaixo chamadas de API.

1. Registe o modelo. Eis alguns exemplos de código:

    ```python
    # register a model out of a run record
    model = best_run.register_model(model_name='my_best_model', model_path='outputs/my_model.pkl')

    # or, you can register a file or a folder of files as a model
    model = Model.register(model_path='my_model.pkl', model_name='my_best_model', workspace=ws)
    ```

2. Crie a imagem. Eis alguns exemplos de código:

    ```python
    # configure the image
    image_config = ContainerImage.image_configuration(runtime="python",
                                                      execution_script="score.py",
                                                      conda_file="myenv.yml")

    # create the image
    image = Image.create(name='myimg', models=[model], image_config=image_config, workspace=ws)

    # wait for image creation to finish
    image.wait_for_creation(show_output=True)
    ```

3. Implemente a imagem como serviço. Eis alguns exemplos de código:

    ```python
    # configure an ACI-based deployment
    aci_config = AciWebservice.deploy_configuration(cpu_cores=1, memory_gb=1)

    aci_service = Webservice.deploy_from_image(deployment_config=aci_config, 
                                               image=image, 
                                               name='mysvc', 
                                               workspace=ws)
    aci_service.wait_for_deployment(show_output=True)    
    ```

Assim que já dividiu o processo de implantação em tarefas individuais, podemos ver alguns dos erros mais comuns.

## <a name="image-building-fails"></a>Falha de criação de imagem
Se o sistema não consegue criar a imagem do Docker, o `image.wait_for_creation()` chamada falha com algumas mensagens de erro que podem oferecer algumas dicas. Também pode encontrar mais detalhes sobre os erros de log da compilação de imagem. Abaixo é um código de exemplo que mostra como detetar o uri de log da compilação de imagem.

```python
# if you already have the image object handy
print(image.image_build_log_uri)

# if you only know the name of the image (note there might be multiple images with the same name but different version number)
print(ws.images['myimg'].image_build_log_uri)

# list logs for all images in the workspace
for name, img in ws.images.items():
    print (img.name, img.version, img.image_build_log_uri)
```
O uri de registo de imagem é um URL de SAS que aponta para um ficheiro de registo armazenado no armazenamento de Blobs do Azure. Basta copiar e colar o uri numa janela do browser e podem transferir e ver o ficheiro de registo.


## <a name="service-launch-fails"></a>Falha de inicialização de serviço
Depois da imagem é criada com êxito, o sistema tenta iniciar um contentor no ACI ou AKS consoante a configuração de implementação. Em geral, é recomendado para tentar uma implementação do ACI em primeiro lugar, uma vez que é uma implantação mais simples de contentor único. Dessa forma pode, então, excluir qualquer problema específico do AKS.

Como parte do processo de cópia de segurança a partir do contentor, o `init()` função no seu script de classificação é invocada pelo sistema. Se existirem exceções não identificadas no `init()` funcione, poderá ver **CrashLoopBackOff** erro na mensagem de erro. Seguem-se algumas sugestões para ajudar a resolver o problema.

### <a name="inspect-the-docker-log"></a>Inspecione o registo do Docker
Pode imprimir mensagens de registo do Docker motor detalhadas do objeto de serviço.

```python
# if you already have the service object handy
print(service.get_logs())

# if you only know the name of the service (note there might be multiple services with the same name but different version number)
print(ws.webservices['mysvc'].get_logs())
```

### <a name="debug-the-docker-image-locally"></a>Depurar a imagem do Docker localmente
Algumas vezes o registo do Docker não emitir informações suficientes sobre o que está errado. Pode ir além e puxar a imagem do Docker incorporada, iniciar um contentor local e depurar diretamente dentro do contentor do live interativamente. Para iniciar um contentor local, tem de ter um motor do Docker a executar localmente e que seria muito mais fácil se também tiver [cli do azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) instalado.

Primeiro, precisamos saber a localização de imagem:

```python
# print image location
print(image.image_location)
```

A localização da imagem tem este formato: `<acr-name>.azurecr.io/<image-name>:<version-number>`, tais como `myworkpaceacr.azurecr.io/myimage:3`. 

Agora, para a janela da linha de comandos. Se tiver a cli do azure instalada, pode digitar os seguintes comandos para iniciar sessão no ACR (Azure Container Registry) associado à área de trabalho onde está armazenada a imagem. 

```sh
# log on to Azure first if you haven't done so before
$ az login

# make sure you set the right subscription in case you have access to multiple subscriptions
$ az account set -s <subscription_name_or_id>

# now let's log in to the workspace ACR
# note the acr-name is the domain name WITHOUT the ".azurecr.io" postfix
# e.g.: az acr login -n myworkpaceacr
$ az acr login -n <acr-name>
```
Se não tiver a cli do azure instalada, pode utilizar `docker login` comando para iniciar sessão para o ACR. Mas precisa obter o nome de utilizador e palavra-passe do ACR do portal do Azure pela primeira vez.

Depois de ter de iniciar sessão para o ACR, pode obter a imagem do Docker e iniciar um contentor localmente e, em seguida, inicie uma sessão do bash para depuração, utilizando o `docker run` comando:

```sh
# note the image_id is <acr-name>.azurecr.io/<image-name>:<version-number>
# for example: myworkpaceacr.azurecr.io/myimage:3
$ docker run -it <image_id> /bin/bash
```

Depois de iniciar uma sessão do bash o contentor em execução, pode encontrar os scripts de classificação no `/var/azureml-app` pasta. Em seguida, pode iniciar uma sessão de Python para depurar seus scripts de classificação. 

```sh
# enter the directory where scoring scripts live
cd /var/azureml-app

# find what Python packages are installed in the python environment
pip freeze

# sanity-check on score.py
# you might want to edit the score.py to trigger init().
# as most of the errors happen in init() when you are trying to load the model.
python score.py
```
Caso seja necessário um editor de texto para modificar seus scripts, pode instalar vim, nano, Emacs ou outro seu editor favorito.

```sh
# update package index
apt-get update

# install a text editor of your choice
apt-get install vim
apt-get install nano
apt-get install emacs

# launch emacs (for example) to edit score.py
emacs score.py

# exit the container bash shell
exit
```

Também pode iniciar o serviço web localmente e enviar o tráfego HTTP para o mesmo. O servidor do Flask no contentor do Docker está em execução na porta 5001. Pode mapear para quaisquer outras portas disponíveis na máquina host.
```sh
# you can find the scoring API at: http://localhost:8000/score
$ docker run -p 8000:5001 <image_id>
```

## <a name="function-fails-getmodelpath"></a>Falha de função: get_model_path()
Muitas vezes, o `init()` função no script de classificação, `Model.get_model_path()` função é chamada para localizar um ficheiro de modelo ou de uma pasta de ficheiros de modelo no contentor. Geralmente essa é uma origem da falha se não é possível localizar o ficheiro de modelo ou pasta. A maneira mais fácil para depurar este erro é executar o abaixo o código de Python no shell do contentor:

```python
from azureml.core.model import Model
print(Model.get_model_path(model_name='my-best-model'))
```

Isso seria imprimir o caminho local (relação ao `/var/azureml-app`) no contentor onde o seu script de classificação está esperando para localizar o ficheiro de modelo ou a pasta. Em seguida, pode verificar se o ficheiro ou pasta está, de fato, onde é esperado que seja.


## <a name="function-fails-runinputdata"></a>Falha de função: run(input_data)
Se o serviço é implementado com êxito, mas ele falha quando postar dados para o ponto final de classificação, pode adicionar erro capturando instrução em seu `run(input_data)` funcionar, para que ele retorne a mensagem de erro detalhada em vez disso. Por exemplo:

```python
def run(input_data):
    try:
        data = json.loads(input_data)['data']
        data = np.array(data)
        result = model.predict(data)
        return json.dumps({"result": result.tolist()})
    except Exception as e:
        result = str(e)
        # return error message back to the client
        return json.dumps({"error": result})
```
**Nota**: Devolver mensagens de erro do `run(input_data)` chamada deve ser feita para apenas a fins de depuração. Não pode ser uma boa idéia fazer isso num ambiente de produção por motivos de segurança.


## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre a implementação: 
* [Como implementar e, em que](how-to-deploy-and-where.md)

* [Tutorial: Preparar e implementar modelos](tutorial-train-models-with-aml.md)
