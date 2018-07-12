---
title: Código de ciência de dados de teste no Azure com o UCI rendimento para adultos predição conjunto de dados - processo de ciência de dados de equipa e o Visual Studio Team Services
description: Código de ciência de dados de teste com dados de predição de rendimento para adultos UCI
services: machine-learning, team-data-science-process
documentationcenter: ''
author: weig
manager: deguhath
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2018
ms.author: weig
ms.openlocfilehash: 7d9d63d6c3d5c8ccf1777a46832457670d307d4a
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38970864"
---
# <a name="data-science-code-testing-with-the-uci-adult-income-prediction-dataset"></a>Código de ciência de dados de teste com o conjunto de dados de predição de rendimento para adultos UCI
Este artigo fornece diretrizes preliminares para testar o código num fluxo de trabalho de ciência de dados. Esses testes fornece os cientistas de dados uma forma sistemática e eficiente para verificar a qualidade e o resultado esperado do seu código. Um Team Data Science Process (TDSP), usamos [projeto que usa o conjunto de dados de rendimento para adultos de UCI](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) que publicamos anteriormente para mostrar como teste de código pode ser feito. 

## <a name="introduction-on-code-testing"></a>Introdução no código de teste
"Testes de unidade" é uma prática de longa data para o desenvolvimento de software. Mas para ciência de dados, muitas vezes, não está claro o que significa que e como deve testar o código para diferentes estágios de um ciclo de vida de ciência de dados, tais como:

* Preparação de dados
* Análise de qualidade de dados
* Modelação
* Implementação do modelo 

Este artigo substitui o termo "testes de unidade" com "código de teste." Refere-se para testar como as funções que ajudam a avaliar se o código de um determinado passo de um ciclo de vida de ciência de dados é produzir resultados "conforme o esperado." A pessoa que está a escrever que o teste define o que é "tal como esperado," consoante o resultado da função – por exemplo, a verificação de qualidade de dados ou modelagem.

Este artigo fornece referências, como recursos úteis.

## <a name="visual-studio-team-services-for-the-testing-framework"></a>Visual Studio Team Services para a estrutura de testes
Este artigo descreve como realizar e automatizar os testes com o Visual Studio Team Services (VSTS). Pode optar por utilizar as ferramentas alternativas. Também vamos mostrar como configurar uma compilação automática com o VSTS e agentes de compilação. Agentes de compilação, vamos utilizar máquinas de virtuais de ciência de dados de Azure (DSVMs).

## <a name="flow-of-code-testing"></a>Fluxo de código de teste
O fluxo de trabalho geral de código de teste num projeto de ciência de dados tem esta aparência: 

![Fluxograma de teste de código](./media/code-test/test-flow-chart.PNG)

    
## <a name="detailed-steps"></a>Passos detalhados

Utilize os seguintes passos para configurar e executar testes de código e uma compilação automatizada com um agente de compilação e VSTS:

1. Crie um projeto no aplicativo de desktop do Visual Studio:

    ![Ecrã "Criar novo projeto de" no Visual Studio](./media/code-test/create_project.PNG)

   Depois de criar seu projeto, vai encontrá-lo no Explorador de soluções, no painel da direita:
    
    ![Passos para criar um projeto](./media/code-test/create_python_project_in_vs.PNG)

    ![Explorador de Soluções](./media/code-test/solution_explorer_in_vs.PNG)

3. Feed seu código de projeto para o repositório de código do projeto VSTS: 

    ![Repositório de código do projeto](./media/code-test/create_repo.PNG)

4. Suponha que fez um trabalho de preparação de dados, como a ingestão de dados, engenharia de funcionalidades e criação de colunas de etiqueta. Pretende certificar-se de que seu código está a gerar os resultados esperados. Este é o código que pode usar para testar se o código de processamento de dados está a funcionar corretamente:

    * Verifique se os nomes de coluna são certos:
    
      ![Código para efetuar a correspondência de nomes de colunas](./media/code-test/check_column_names.PNG)

    * Verifique que os níveis de resposta estão certos:

      ![Código para efetuar a correspondência de níveis](./media/code-test/check_response_levels.PNG)

    * Certifique-se de que a percentagem de resposta está razoável:

      ![Código de percentagem de resposta](./media/code-test/check_response_percentage.PNG)

    * Verificar a taxa de falta de cada coluna nos dados:
    
      ![Código para obter uma taxa em falta](./media/code-test/check_missing_rate.PNG)


5. Depois de fazer o processamento de dados e o trabalho de engenharia de funcionalidades e Treinou um bom modelo, certifique-se de que o modelo treinado por pode classificar novos conjuntos de dados corretamente. Pode utilizar os seguintes dois testes para verificar os níveis de previsão e distribuição de valores de etiqueta:

    * Verifique os níveis de predição:
    
      ![Código para verificar os níveis de predição](./media/code-test/check_prediction_levels.PNG)

    * Verifique a distribuição dos valores de predição:

      ![Código para verificar os valores de previsão](./media/code-test/check_prediction_values.PNG)

6. PUT todos testar as funções em conjunto num script de Python chamado **test_funcs.py**:

    ![Script de Python para funções de teste](./media/code-test/create_file_test_func.PNG)


7. Depois dos códigos de teste estão preparados, pode configurar o ambiente de teste no Visual Studio.

   Crie um ficheiro Python denominado **test1.py**. Este ficheiro, crie uma classe que inclui todos os testes que deseja fazer. O exemplo seguinte mostra seis testes preparados:
    
    ![Ficheiro de Python com uma lista de testes numa classe](./media/code-test/create_file_test1_class.PNG)

8. Esses testes podem ser detetados automaticamente se colocar **codetest.testCase** após o nome da sua classe. Abra o Gerenciador de testes no painel da direita e selecione **executar todos**. Todos os testes serão executados sequencialmente e informará se o teste é efetuada com êxito ou não.

    ![Executar os testes](./media/code-test/run_tests.PNG)

9. Verifique no seu código para o repositório do projeto ao utilizar comandos do Git. Seu trabalho mais recente será refletido daqui a pouco no VSTS.

    ![Comandos do Git para a verificação no código](./media/code-test/git_check_in.PNG)

    ![Trabalho mais recente no VSTS](./media/code-test/git_check_in_most_recent_work.PNG)

10. Configurar a compilação automática e teste no VSTS:

    a. No repositório do projeto, selecione **criar e lançar**e, em seguida, selecione **+ novo** para criar um novo processo de compilação.

       ![Seleções para iniciar um novo processo de compilação](./media/code-test/create_new_build.PNG)

    b. Siga as instruções para selecionar a localização do código de origem, nome do projeto, repositório e informações do ramo.
    
       ![Origem, o nome, o repositório e informações do ramo](./media/code-test/fill_in_build_info.PNG)

    c. Selecione um modelo. Como não há nenhum modelo de projeto de Python, comece por selecionar **vazio processo**. 

       ![Lista de modelos e botão "Vazio processo"](./media/code-test/start_empty_process_template.PNG)

    d. Nomeie a compilação e selecione o agente. Se pretender utilizar uma DSVM para concluir o processo de compilação, pode escolher o padrão aqui. Para obter mais informações sobre agentes de definição, consulte [criar e lançar agentes](https://docs.microsoft.com/vsts/build-release/concepts/agents/agents?view=vsts).
    
       ![Selecione as opções de compilação e o agente](./media/code-test/select_agent.PNG)

    e. Selecione **+** no painel esquerdo, para adicionar uma tarefa para esta fase de compilação. Como nós vamos para executar o script de Python **test1.py** para concluir a todas as verificações, esta tarefa está a utilizar um comando do PowerShell para executar o código de Python.
    
       ![Painel "Adicionar tarefas" com o PowerShell selecionado](./media/code-test/add_task_powershell.PNG)

    f. Nos detalhes do PowerShell, preencha as informações necessárias, como o nome e versão do PowerShell. Escolher **o Inline Script de** como o tipo. 
    
       Na caixa em **Inline Script**, pode escrever **python test1.py**. Certifique-se de que a variável de ambiente está configurada corretamente para o Python. Se precisar de uma versão diferente ou o kernel do Python, é possível especificar explicitamente o caminho como mostrado na figura: 
    
       ![Detalhes do PowerShell](./media/code-test/powershell_scripts.PNG)

    g. Selecione **guardar e colocar em fila** para concluir o processo de definição de compilação.

       ![Botão "Guardar e colocar em fila"](./media/code-test/save_and_queue_build_definition.PNG)

Agora sempre que uma consolidação novo é emitida para o repositório de código, o processo de compilação será iniciada automaticamente. (Aqui, vamos utilizar mestre como o repositório, mas pode definir qualquer ramo.) O processo é executado o **test1.py** ficheiro no computador agente para certificar-se de que tudo definidas no código é executado corretamente. 

Se os alertas estão configurados corretamente, será notificado por e-mail quando a compilação estiver concluída. Também pode verificar o estado de compilação no VSTS. Se falhar, pode verificar os detalhes da compilação e descobrir qual parte foi interrompida.

![Notificação de e-mail de êxito de compilação](./media/code-test/email_build_succeed.PNG)

![Notificação de VSTS de sucesso de compilação](./media/code-test/vs_online_build_succeed.PNG)

## <a name="next-steps"></a>Passos Seguintes
* Consulte a [repositório de predição de renda UCI](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) para exemplos concretos de testes de unidade para cenários de ciência de dados.
* Siga a estrutura de tópicos e exemplos de cenário de predição de renda UCI em seus próprios projetos de ciência de dados anterior.

## <a name="references"></a>Referências
* [Processo de Ciência de Dados de Equipa](https://aka.ms/tdsp)
* [Ferramentas de teste do Visual Studio](https://www.visualstudio.com/vs/features/testing-tools/)
* [Recursos de teste do VSTS](https://www.visualstudio.com/team-services/)
* [Máquinas de virtuais de ciência de dados](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/)