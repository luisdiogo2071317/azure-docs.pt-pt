---
title: Código de ciência de dados de teste no Azure com o UCI receitas para adultos predição conjunto de dados - o processo de ciência de dados de equipa e o Visual Studio Team Services
description: Código de ciência de dados de teste com dados de predição de receitas para adultos UCI
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
ms.openlocfilehash: de1ed0b85957413a254503fc72375866dfd1bea1
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "34837162"
---
# <a name="data-science-code-testing-with-the-uci-adult-income-prediction-dataset"></a>Código de ciência de dados de teste com o conjunto de dados de predição de receitas para adultos UCI
Este artigo fornece diretrizes preliminares para testar o código num fluxo de trabalho de ciências de dados. Esses testes dá-cientistas de dados de forma systematic e eficiente para verificar a qualidade e o resultado esperado do seu código. Utilizamos o processo de ciência de dados (TDSP) uma equipa [projeto que utiliza o conjunto de dados de receitas para adultos UCI](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) que iremos publicadas anteriormente para mostrar como testar código pode ser efetuada. 

## <a name="introduction-on-code-testing"></a>Introdução no código de teste
"Unidade de teste" é uma prática longa data para o desenvolvimento de software. Mas para ciência de dados, muitas vezes, não é claro que que significa e como deve testar o código para diferentes fases de um ciclo de vida de ciência de dados, tais como:

* Preparação de dados
* Análises de qualidade de dados
* Modelação
* Modelo de implementação 

Este artigo substitui o termo "unidade de teste" com "código de teste." Refere-se para testar como as funções que ajudam a avaliar se o código de um determinado passo de um ciclo de vida de ciência de dados está a produzir resultados "tal como era esperado." A pessoa que está a escrever que o teste define o que é "tal como era esperado," consoante o resultado da função – por exemplo, a verificação de qualidade de dados ou modelação.

Este artigo fornece referências sobre como recursos úteis.

## <a name="visual-studio-team-services-for-the-testing-framework"></a>Visual Studio Team Services para a estrutura de teste
Este artigo descreve como efetuar e automatizar o teste, utilizando os serviços de equipa do Visual Studio (VSTS). Pode optar por utilizar ferramentas alternativas. Podemos também mostram como configurar uma compilação automática utilizando VSTS e criar agentes. Para agentes de compilação, podemos utilizar dados de ciência de Virtual Machines do Azure (DSVMs).

## <a name="flow-of-code-testing"></a>Fluxo de código de teste
O fluxo de trabalho geral do código de teste num projeto de ciência de dados tem o seguinte aspeto: 

![Fluxograma de teste de código](./media/code-test/test-flow-chart.PNG)

    
## <a name="detailed-steps"></a>Passos detalhados

Utilize os seguintes passos para configurar e executar o código de teste e uma compilação automatizada através da utilização de um agente de compilação e VSTS:

1. Crie um projeto da aplicação de ambiente de trabalho do Visual Studio:

    ![Ecrã "Criar novo projeto" no Visual Studio](./media/code-test/create_project.PNG)

   Depois de criar o projeto, pode encontrá-la no Explorador de soluções no painel da direita:
    
    ![Passos para criar um projeto](./media/code-test/create_python_project_in_vs.PNG)

    ![Explorador de Soluções](./media/code-test/solution_explorer_in_vs.PNG)

3. O código de projeto do feed no repositório de código de projeto VSTS: 

    ![Repositório de código do projeto](./media/code-test/create_repo.PNG)

4. Suponha que fez algumas trabalho de preparação de dados, tais como a ingestão de dados, engenharia da funcionalidade e criar colunas de etiqueta. Deve certificar-se de que o seu código está a gerar os resultados que pretende. Eis alguns códigos que pode utilizar para testar se o código de processamento de dados está a funcionar corretamente:

    * Verifique se os nomes de coluna estão corretos:
    
      ![Código para a correspondência de nomes de colunas](./media/code-test/check_column_names.PNG)

    * Verifique que estão corretas níveis de resposta:

      ![Código para a correspondência níveis](./media/code-test/check_response_levels.PNG)

    * Certifique-se de que a percentagem de resposta está razoável:

      ![Código de percentagem de resposta](./media/code-test/check_response_percentage.PNG)

    * Verifique a taxa de cada coluna de dados em falta:
    
      ![Código para taxa em falta](./media/code-test/check_missing_rate.PNG)


5. Depois de fez o processamento de dados e o trabalho engenharia da funcionalidade e tiver um bom modelo de preparação, certifique-se de que o modelo que preparada pode Pontuar novos conjuntos de dados corretamente. Pode utilizar os seguintes dois testes para verificar os níveis de predição e a distribuição dos valores de etiqueta:

    * Níveis de predição de verificação:
    
      ![Código para a verificação de níveis de predição](./media/code-test/check_prediction_levels.PNG)

    * Verifique a distribuição dos valores de predição:

      ![Código para a verificação de valores de predição](./media/code-test/check_prediction_values.PNG)

6. Coloque todas as testar as funções em conjunto para um script de Python chamado **test_funcs.py**:

    ![Script de Python para funções de teste](./media/code-test/create_file_test_func.PNG)


7. Depois dos códigos de teste estão preparados, pode configurar o ambiente de teste no Visual Studio.

   Crie um ficheiro de Python chamado **test1.py**. Neste ficheiro, crie uma classe que inclui todos os testes que pretende efetuar. O exemplo seguinte mostra os seis testes preparados:
    
    ![Ficheiro de Python com uma lista dos testes de uma classe](./media/code-test/create_file_test1_class.PNG)

8. Esses testes podem ser automaticamente descobertos se colocar **codetest.testCase** após o nome de classe. Abra o Explorador do teste no painel direito e selecione **executar todos os**. Todos os testes serão executados sequencialmente e informará se o teste está concluída com êxito ou não.

    ![A executar os testes](./media/code-test/run_tests.PNG)

9. Verifique no seu código para o repositório de projeto, utilizando os comandos de Git. O seu trabalho mais recente será apresentado em breve no VSTS.

    ![Comandos de Git para a verificar no código](./media/code-test/git_check_in.PNG)

    ![Trabalho mais recente no VSTS](./media/code-test/git_check_in_most_recent_work.PNG)

10. Configurar a criação automática e testar no VSTS:

    a. No repositório de projeto, selecione **compilar e versão**e, em seguida, selecione **+ novo** para criar um novo processo de compilação.

       ![Seleções para iniciar um novo processo de compilação](./media/code-test/create_new_build.PNG)

    b. Siga as instruções para selecionar a localização do código de origem, nome do projeto, repositório e informações de ramo.
    
       ![Origem, o nome, o repositório e informações de ramo](./media/code-test/fill_in_build_info.PNG)

    c. Selecione um modelo. Porque não existe nenhum modelo de projeto de Python, comece por selecionar **vazio processo**. 

       ![Lista de modelos e no botão "Vazio processo"](./media/code-test/start_empty_process_template.PNG)

    d. A compilação de nome e selecione o agente. Pode escolher aqui a predefinição se pretender utilizar um DSVM para concluir o processo de compilação. Para obter mais informações sobre os agentes de definição, consulte [compilar e versão agentes](https://docs.microsoft.com/en-us/vsts/build-release/concepts/agents/agents?view=vsts).
    
       ![Seleções de compilação e agente](./media/code-test/select_agent.PNG)

    e. Selecione **+** no painel esquerdo, para adicionar uma tarefa para esta fase de compilação. Porque vamos para executar o script de Python **test1.py** para concluir todas as verificações, esta tarefa está a utilizar um comando do PowerShell para executar código Python.
    
       ![Painel "Adicionar tarefas" com o PowerShell selecionado](./media/code-test/add_task_powershell.PNG)

    f. Nos detalhes do PowerShell, preencha as informações necessárias, tais como o nome e versão do PowerShell. Escolha **o Inline Script de** como o tipo. 
    
       Na caixa em **o Inline Script de**, pode escrever **python test1.py**. Certifique-se que a variável de ambiente está configurada corretamente para o Python. Se precisar de uma versão diferente ou kernel do Python, pode especificar explicitamente o caminho conforme mostrado na figura: 
    
       ![Detalhes do PowerShell](./media/code-test/powershell_scripts.PNG)

    g. Selecione **Guardar & fila** para concluir o processo de definição de compilação.

       ![Botão "Guardar & fila"](./media/code-test/save_and_queue_build_definition.PNG)

Agora sempre que consolidar um novo é enviada para o repositório de código, o processo de compilação é iniciado automaticamente. (Aqui, utilizamos mestre como repositório, mas pode definir quaisquer sucursais). O processo é executado o **test1.py** ficheiro no computador de agente para se certificar de que tudo definida no código é executada corretamente. 

Se alertas estão corretamente configuradas, será notificado por correio eletrónico quando a compilação for concluída. Também pode verificar o estado de compilação no VSTS. Se falhar, pode verificar os detalhes da compilação e saber que peça foi interrompida.

![E-mail de notificação de sucesso de compilação](./media/code-test/email_build_succeed.PNG)

![Notificação de VSTS de sucesso de compilação](./media/code-test/vs_online_build_succeed.PNG)

## <a name="next-steps"></a>Passos Seguintes
* Consulte o [repositório de predição de receitas UCI](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) para obter exemplos concretos de testes de unidade para cenários de ciência de dados.
* Siga o contorno e exemplos de cenário de predição de receitas UCI nos seus projetos de ciência de dados anterior.

## <a name="references"></a>Referências
* [Processo de Ciência de Dados de Equipa](https://aka.ms/tdsp)
* [Ferramentas de teste do Visual Studio](https://www.visualstudio.com/vs/features/testing-tools/)
* [VSTS testar recursos](https://www.visualstudio.com/team-services/)
* [Máquinas de virtuais de ciência de dados](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/)