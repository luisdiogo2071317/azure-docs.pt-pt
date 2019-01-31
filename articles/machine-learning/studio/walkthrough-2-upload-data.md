---
title: "Passo 2: Carregar dados para uma experimentação do Machine Learning Studio' titleSuffix: Descrição do Azure Machine Learning Studio: "Passo 2 da desenvolver uma solução preditiva passo a passo: Carregamento armazenados dados públicos no Azure Machine Learning Studio. "
services: machine-learning ms.service: machine-learning ms.subservice: studio ms.topic: article

author: garyericson ms.author: garye ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro ms.date: 03/23/2017
---
# <a name="walkthrough-step-2-upload-existing-data-into-an-azure-machine-learning-studio-experiment"></a>Passo 2 das instruções: Carregar dados existentes para uma experimentação do Azure Machine Learning Studio
Este é o segundo passo de passo a passo, [desenvolver uma solução de Análise Preditiva no Azure Machine Learning](walkthrough-develop-predictive-solution.md)

1. [Criar uma área de trabalho do Machine Learning](walkthrough-1-create-ml-workspace.md)
2. **Carregar os dados existentes**
3. [Criar uma nova experimentação](walkthrough-3-create-new-experiment.md)
4. [Dar formação e avaliar os modelos](walkthrough-4-train-and-evaluate-models.md)
5. [Implementar o serviço Web](walkthrough-5-publish-web-service.md)
6. [Aceder ao serviço Web](walkthrough-6-access-web-service.md)

- - -
Para desenvolver um modelo preditivo para o risco de crédito, precisamos de dados que pode ser usada para treinar e, em seguida, testar o modelo. Nestas instruções, vamos utilizar o "UCI Statlog (dados de crédito alemães) conjunto de dados" do repositório de UC Irvine Machine Learning. Pode encontrá-lo aqui:  
<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)</a>

Vamos utilizar o ficheiro denominado **german.data**. Transferir este ficheiro para o disco rígido local.  

O **german.data** conjunto de dados contém linhas de 20 variáveis para 1000 candidatos nos últimos de crédito. Estas variáveis de 20 representam o conjunto do conjunto de dados de recursos (a *vetor de funcionalidade*), que fornece características de identificação para cada candidato de crédito. Uma coluna adicional em cada linha representa o risco de crédito calculado o candidato, com 700 candidatos identificados como um risco de crédito baixa e 300 como um risco elevado.

O site UCI fornece uma descrição dos atributos do vetor de funcionalidade para estes dados. Isto inclui informações financeiras, o histórico de crédito, o estado de emprego e informações pessoais. Para cada candidato, uma classificação binária foi determinado que indica se são uma baixa ou alta risco de crédito. 

Utilizaremos estes dados para preparar um modelo de Análise Preditiva. Quando finalizamos, o nosso modelo deve ser capaz de aceitar um vetor de funcionalidade para um novo indivíduo e prever se ele é um risco de crédito baixa ou alta.  

Aqui está uma mudança interessante. A descrição do conjunto de dados no site da UCI menciona o que custa se podemos misclassify o risco de crédito de uma pessoa.
Se o modelo prevê um risco elevado crédito para alguém que é, na verdade, um risco de crédito baixa, o modelo fez uma misclassification.
Mas o inverso misclassification é cinco vezes mais caro para a instituição financeira: se o modelo prevê um risco de crédito baixa para alguém que é, na verdade, um risco elevado crédito.

Por isso, queremos preparar o nosso modelo para que o custo deste último tipo de misclassification é cinco vezes maior do que misclassifying a outra forma.
É uma forma simples de fazê-lo ao treinar o modelo na nossa experiência ao duplicar (cinco vezes) para essas entradas que representam a alguém com um risco elevado crédito. Em seguida, se o modelo misclassifies alguém como um risco de crédito baixa quando eles são, na verdade, um risco elevado, o modelo faz essa mesma misclassification cinco vezes, uma vez para cada duplicado. Isso aumentará o custo deste erro nos resultados de treinamento.


## <a name="convert-the-dataset-format"></a>Converter o formato de conjunto de dados
O conjunto de dados original usa um formato separados em branco. O Machine Learning Studio funciona melhor com um ficheiro de valores separados por vírgulas (CSV), portanto, converteremos o conjunto de dados, substituindo espaços com vírgulas.  

Existem várias formas de converter esses dados. Uma forma é utilizando o seguinte comando Windows PowerShell:   

    cat german.data | %{$_ -replace " ",","} | sc german.csv  

Outra forma é através do comando de po Unix:  

    sed 's/ /,/g' german.data > german.csv  

Em ambos os casos, criámos uma versão separada por vírgulas dos dados num arquivo chamado **german.csv** que podemos usar na nossa experiência.

## <a name="upload-the-dataset-to-machine-learning-studio"></a>Carregar o conjunto de dados para Machine Learning Studio
Depois dos dados foi convertidos em formato CSV, precisamos carregá-lo para Machine Learning Studio. 

1. Abrir a home page do Machine Learning Studio ([https://studio.azureml.net](https://studio.azureml.net)). 

2. Clique no menu ![Menu][1] no canto superior esquerdo da janela, clique **do Azure Machine Learning**, selecione **Studio**e iniciar sessão.

3. Clique em **+ novo** na parte inferior da janela.

4. Selecione **conjunto de dados**.

5. Selecione **do ficheiro LOCAL**.

    ![Adicionar um conjunto de dados a partir de um ficheiro local][2]

6. Na **carregar um novo conjunto de dados** caixa de diálogo, clique em **procurar** e localize o **german.csv** ficheiro que criou.

7. Introduza um nome para o conjunto de dados. Nestas instruções, chamá-lo de "Dados de cartão de crédito de alemão de UCI".

8. Para o tipo de dados, selecione **arquivo CSV genérico com nenhum cabeçalho (. nh.csv)**.

9. Adicione uma descrição, se desejar.

10. Clique nas **OK** marca de verificação.  

    ![Carregar o conjunto de dados][3]

Isso carrega os dados num módulo de conjunto de dados que pode ser utilizado numa experimentação.

Pode gerir conjuntos de dados que carregar para o Studio clicando a **conjuntos de dados** separador à esquerda da janela do Studio.

![Gerir conjuntos de dados][4]

Para obter mais informações sobre a importação de outros tipos de dados para uma experimentação, consulte [importar os dados de treinamento para o Azure Machine Learning Studio](import-data.md).

**Seguinte: [Criar uma nova experimentação](walkthrough-3-create-new-experiment.md)**

[1]: media/walkthrough-2-upload-data/menu.png
[2]: media/walkthrough-2-upload-data/add-dataset.png
[3]: media/walkthrough-2-upload-data/upload-dataset.png
[4]: media/walkthrough-2-upload-data/dataset-list.png
