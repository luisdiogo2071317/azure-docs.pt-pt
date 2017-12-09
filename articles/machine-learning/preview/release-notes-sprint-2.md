---
title: "Notas de versão do Azure ML Workbench para sprint 2 de Dezembro de 2017"
description: "Este documento fornece detalhes sobre as atualizações para a versão de sprint 2 do Azure ML"
services: machine-learning
author: raymondlaghaeian
ms.author: raymondl
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 12/04/2017
ms.openlocfilehash: 6fede727a36b0f3243975bf23df2ef53baf1b15c
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2017
---
# <a name="sprint-2---december-2017"></a>Sprint 2 - dezembro de 2017 

#### <a name="version-number-01171115263"></a>Número de versão: 0.1.1711.15263

>Eis como pode [determinar o número de versão](https://docs.microsoft.com/azure/machine-learning/preview/known-issues-and-troubleshooting-guide).

Bem-vindo à actualização terceira do Workbench do Azure Machine Learning. Esta atualização inclui melhoramentos na aplicação workbench, a Interface de linha de comandos (CLI) e os serviços de back-end. Obrigado muito para enviar os smiles e frowns. Muitos das seguintes atualizações que são efetuados como diretos resultados dos seus comentários. 

## <a name="notable-new-features"></a>Funcionalidades novas importantes
- [Suporte para o SQL Server e BD SQL do Azure como uma origem de dados](https://docs.microsoft.com/azure/machine-learning/preview/data-prep-appendix2-supported-data-sources#types) 
- [Aprendizagem profunda Spark com suporte para a GPU utilizando MMLSpark](https://github.com/Azure/mmlspark/blob/master/docs/gpu-setup.md)
- [Todos os contentores AML são compatíveis com dispositivos de limite de IoT do Azure quando implementado (não existem passos adicionais necessários)](http://aka.ms/aml-iot-edge-blog)
- Portal do Azure disponível de vistas de lista de modelo registado e os detalhes
- Aceder aos destinos de computação, utilizando a autenticação baseada em chave SSH para além de acesso baseado em nome de utilizador/palavra-passe. 
- Novo Inspector de frequência de padrão nos dados de preparação experiência. 

## <a name="detailed-updates"></a>Atualizações de detalhado
Segue-se uma lista das atualizações de detalhado cada área de componente do Azure Machine Learning neste sprint.

### <a name="installer"></a>Instalador
- Instalador pode atualização automática, de modo a que erros correções e novas funcionalidades podem ser suportadas sem necessidade de reinstalá-lo do utilizador

### <a name="workbench-authentication"></a>Autenticação do Workbench
- Várias correções para o sistema de autenticação. Indique se ainda ocorrerem problemas de início de sessão.
- Alterações de IU que tornam mais fácil localizar as definições do Gestor de Proxy.

### <a name="workbench"></a>Workbench
- Vista de ficheiro só de leitura tem agora em segundo plano azul leve
- Botão Editar movido para a direita para torná-lo mais Detetáveis.
- "dsource", "dprep" e "ipynb" formatos de ficheiro agora podem ser compostos num formato de texto em bruto
- O workbench tem agora uma nova experiência de edição guias de utilizadores para utilizar externo IDE para editar os scripts e utilize Workbench apenas para editar os tipos de ficheiros que tenham uma experiência avançada de edição (por exemplo, blocos de notas, origens de dados, pacotes de preparação de dados)
- Carregar a lista de áreas de trabalho e projetos de que o utilizador tem acesso ao é significativamente mais rápido agora

### <a name="data-preparation"></a>Preparação de dados 
- Um padrão de frequência Inspector para ver os padrões de cadeia numa coluna. Também pode filtrar os dados utilizando estes padrões. Isto mostra-lhe uma vista semelhante inspector contagens de valor. A diferença é que a frequência de padrão mostra o número de padrões de exclusivos dos dados, em vez de contagens de dados exclusivos. Também pode filtrar a entrada ou saída todas as linhas que se enquadram em determinados padrão.

![Imagem de inspector de frequência de padrão no número de produto](media/release-notes-sprint-2/pattern-inspector-product-number.png)

- Melhorias de desempenho ao Recomendamos casos de contorno para rever na transformação 'deriva, por exemplo, o coluna'

- [Suporte para o SQL Server e BD SQL do Azure como uma origem de dados](https://docs.microsoft.com/azure/machine-learning/preview/data-prep-appendix2-supported-data-sources#types) 

![Imagem da criação de uma nova origem de dados do SQL server](media/release-notes-sprint-2/sql-server-data-source.png)

- Ativado "rapidamente" vista de contagens de linha e coluna

![Imagem da contagem de colunas de linha numa tolerância](media/release-notes-sprint-2/row-col-count.png)

- Preparação de dados está ativada em todos os contextos de computação
- Origens de dados que utilizam uma base de dados do SQL Server estão ativadas em todos os contextos de computação
- Dados de preparação grelha colunas podem ser filtradas pelo tipo de dados
- Fixo problema com a conversão de várias colunas à data
- Corrigido o problema que o utilizador selecionar a coluna de resultado como uma origem derivar coluna por exemplo se o utilizador alterar o nome de coluna de saída no modo avançado.

### <a name="job-execution"></a>Execução da tarefa
Agora pode criar e aceder a um destino de computação de tipo remotedocker ou cluster utilizando a autenticação baseada em chave SSH seguindo estes passos:
- Anexar um destino de computação utilizando o seguinte comando na CLI

    ```azure-cli
    $ az ml computetarget attach remotedocker --name "remotevm" --address "remotevm_IP_address" --username "sshuser" --use-azureml-ssh-key
    ```
[!NOTE] Especifica uma opção -k (ou - utilização-azureml--chave ssh) no comando para gerar e utilizar a chave SSH.

- Azure ML Workbench irá gerar uma chave pública e de saída que na sua consola. Inicie sessão no destino computação utilizando o mesmo nome de utilizador e de acréscimo ~/.ssh/authorized_keys ficheiros com esta chave pública.

- Pode preparar este destino de computação e utilizá-lo para execução e do Azure ML Workbench esta chave será utilizada para autenticação.  

Para obter mais informações sobre a criação de destinos de computação, consulte [configurar o serviço do Azure Machine Learning experimentação](https://docs.microsoft.com/azure/machine-learning/preview/experimentation-service-configuration)

### <a name="visual-studio-tools-for-ai"></a>Visual Studio Tools para IA
- Foi adicionado suporte para [Visual Studio Tools para AI](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vstoolsai-vs2017). 

### <a name="command-line-interface-cli"></a>Interface de Linha de Comandos (CLI)
- Adicionar `az ml datasource create` o comando permite criar um ficheiro de origem de dados a partir da linha de comandos

### <a name="model-management-and-operationalization"></a>Gestão de modelo e Operationalization
- [Todos os contentores AML são compatíveis com dispositivos de limite de IoT do Azure quando operationalized (não existem passos adicionais necessários)](http://aka.ms/aml-iot-edge-blog) 
- Melhoramentos de mensagens de erro a CLI o16n
- Correções de erros no portal de gestão de modelo UX  
- Letra consistente tem maiúsculas e minúsculas para atributos de gestão de modelo na página de detalhes
- Em tempo real da classificação de tempo limite de chamadas definido como 60 segundos
- Modelo registado lista e os detalhes vistas disponíveis no portal do Azure

![detalhes de modelo no portal](media/release-notes-sprint-2/model-list.jpg)

![Descrição geral do modelo no portal](media/release-notes-sprint-2/model-overview-portal.jpg)

### <a name="mmlspark"></a>MMLSpark
- Ligação avançada Learning no Spark com [suporte GPU](https://github.com/Azure/mmlspark/blob/master/docs/gpu-setup.md)
- Suporte para modelos do Resource Manager para a implementação de recursos mais fácil
- Suporte para o ecossistema de SparklyR
- [Integração de AZTK](https://github.com/Azure/aztk/wiki/Spark-on-Azure-for-Python-Users#optional-set-up-mmlspark)

### <a name="sample-projects"></a>Projetos de exemplo
- [Iris](https://github.com/Azure/MachineLearningSamples-Iris) e [MMLSpark](https://github.com/Azure/mmlspark) amostras atualizadas com a nova versão do SDK do Azure ML

## <a name="breaking-changes"></a>ALTERAÇÕES DE ÚLTIMA HORA
- Promovido a `--type` no comutador `az ml computetarget attach` para um comando secundárias. 

    - `az ml computetarget attach --type remotedocker`é agora`az ml computetarget attach remotedocker`
    - `az ml computetarget attach --type cluster`é agora`az ml computetarget attach cluster`
