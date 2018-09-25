---
title: Implementação do Azure Machine Learning guia de resolução de problemas | Documentos da Microsoft
description: Guia de resolução de problemas de implantação e a criação do serviço
services: machine-learning
author: aashishb
ms.author: aashishb
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 11/16/2017
ROBOTS: NOINDEX
ms.openlocfilehash: a2867545e454e2b13360d87f5282e684753d6476
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46994501"
---
# <a name="troubleshooting-service-deployment-and-environment-setup"></a>Resolução de problemas de implementação do serviço e configuração do ambiente

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]

As informações seguintes podem ajudar a determinar a causa dos erros quando configurar o ambiente de gestão de modelo.

## <a name="model-management-environment"></a>Ambiente de gestão de modelo
### <a name="contributor-permission-required"></a>Permissão de contribuinte necessária
Precisa de acesso de contribuinte à subscrição ou o grupo de recursos para configurar um cluster para a implementação dos seus serviços web.

### <a name="resource-availability"></a>Disponibilidade dos recursos
Tem de ter recursos suficientes disponíveis na sua subscrição, pelo que pode aprovisionar os recursos de ambiente.

### <a name="subscription-caps"></a>Limites de subscrição
A subscrição pode ter um limite que pode impedi-lo de aprovisionar os recursos de ambiente de faturação. Remova essa cap para ativar o aprovisionamento.

### <a name="enable-debug-and-verbose-options"></a>Ativar a depuração e opções verbosas
Utilize o `--debug` e `--verbose` sinalizadores no comando de configuração para mostrar informações de depuração e rastreamento, tal como o ambiente está a ser aprovisionado.

```
az ml env setup -l <location> -n <name> -c --debug --verbose 
```

## <a name="service-deployment"></a>Implementação do serviço
As informações seguintes podem ajudar a determinar a causa dos erros durante a implementação ou ao chamar o serviço web.

### <a name="service-logs"></a>Registos do serviço
O `logs` opção do serviço CLI fornece dados de registo do Docker e Kubernetes.

```
az ml service logs realtime -i <web service id>
```

Para as definições de registo adicionais, utilize o `--help` (ou `-h`) opção.

```
az ml service logs realtime -h
```

### <a name="debug-and-verbose-options"></a>Opções de depuração e verboso
Utilize o `--debug` sinalizador para mostrar registos de depuração, tal como o serviço está a ser implementado.

```
az ml service create realtime -m <modelfile>.pkl -f score.py -n <service name> -r python --debug
```

Utilize o `--verbose` sinalizador para ver detalhes adicionais, como o serviço está a ser implementado.

```
az ml service create realtime -m <modelfile>.pkl -f score.py -n <service name> -r python --verbose
```

### <a name="enable-request-logging-in-app-insights"></a>Ativar o pedido de início de sessão do App Insights
Definir o `-l` sinalizador como true quando criar um serviço web para ativar o registo ao nível do pedido. Os registos de pedido são escritos para a instância do App Insights para o seu ambiente no Azure. Pesquisa para esta instância com o nome de ambiente que utilizou quando utilizar o `az ml env setup` comando.

- Definir `-l` como true quando criar o serviço.
- Abra o App Insights no portal do Azure. Utilize o seu nome de ambiente para localizar a instância do App Insights.
- Uma vez na App Insights, clique em pesquisa no menu superior para ver os resultados.
- Ou aceda ao `Analytics`  >  `Exceptions`  >  `exceptions take | 10`.


### <a name="add-error-handling-in-scoring-script"></a>Adicionar a manipulação de erros no script de classificação
Utilizar exceção processar em seu `scoring.py` do script **executar** função para retornar a mensagem de erro como parte da sua saída de serviço da web.

Exemplo de Python:
```
    try:
        <code to load model and score>
   except Exception as e:
        return(str(e))
```

## <a name="other-common-problems"></a>Outros problemas comuns
- Se a instalação do pip do azure-cli-ml falhar com o erro `cannot find the path specified` num computador Windows, terá de ativar o suporte de caminho longos. Consulte https://blogs.msdn.microsoft.com/jeremykuhne/2016/07/30/net-4-6-2-and-long-paths-on-windows-10/. 
- Se o `env setup` comando falha com `LocationNotAvailableForResourceType`, provavelmente está usando a localização (região) errada para a máquina de recursos de aprendizado. Certifique-se a sua localização especificada com o `-l` é o parâmetro `eastus2`, `westcentralus`, ou `australiaeast`.
- Se o `env setup` comando falha com `Resource quota limit exceeded`, certifique-se de que tem núcleos suficientes disponíveis na sua subscrição e que os seus recursos não estão a ser utilizados a cópia de segurança em outros processos.
- Se o `env setup` comando falha com `Invalid environment name. Name must only contain lowercase alphanumeric characters`, certifique-se de que o nome do serviço não contém letras maiúsculas, símbolos ou caráter de sublinhado (_) (como mostrado na *my_environment*).
- Se o `service create` comando falha com `Service Name: [service_name] is invalid. The name of a service must consist of lower case alphanumeric characters (etc.)`, certifique-se o nome do serviço é entre 3 e 32 carateres de comprimento; começa e termina com carateres alfanuméricos minúsculos; e não contém letras maiúsculas, símbolos que não seja um hífen (-) e o período ( . ), ou o caráter de sublinhado (_) (como na *my_webservice*).
- Repita se obtiver um `502 Bad Gateway` Ocorreu um erro ao chamar o serviço web. Isso normalmente significa que o contentor não tenha sido implantado para o cluster ainda.
- Se obtiver `CrashLoopBackOff` erro ao criar um serviço, verifique os registos. Normalmente, é o resultado de dependências em falta na **init** função.
