---
title: "Passo 3: Criar uma nova experimentação de Machine Learning Studio' titleSuffix: Descrição do Azure Machine Learning Studio: "A desenvolver uma solução preditiva passo a passo, o passo 3: Criar uma nova experimentação de formação no Azure Machine Learning Studio. "
services: machine-learning ms.service: machine-learning ms.subservice: studio ms.topic: article

author: garyericson ms.author: garye ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro ms.date: 03/23/2017
---
# <a name="walkthrough-step-3-create-a-new-azure-machine-learning-studio-experiment"></a>Passo 3 das instruções: Criar uma nova experimentação do Azure Machine Learning Studio
Este é o terceiro passo de passo a passo, [desenvolver uma solução de Análise Preditiva no Azure Machine Learning](walkthrough-develop-predictive-solution.md)

1. [Criar uma área de trabalho do Machine Learning](walkthrough-1-create-ml-workspace.md)
2. [Carregar os dados existentes](walkthrough-2-upload-data.md)
3. **Criar uma nova experimentação**
4. [Dar formação e avaliar os modelos](walkthrough-4-train-and-evaluate-models.md)
5. [Implementar o serviço Web](walkthrough-5-publish-web-service.md)
6. [Aceder ao serviço Web](walkthrough-6-access-web-service.md)

- - -
Nestas instruções, a próxima etapa é criar uma experimentação no Machine Learning Studio, que utiliza o conjunto de dados que carregou.  

1. No Studio, clique em **+ novo** na parte inferior da janela.
2. Selecione **experimentação**e, em seguida, selecione "Experimentação em branco". 

    ![Criar uma nova experimentação][0]

2. Selecione o nome predefinido da experimentação na parte superior da tela e renomeá-lo para algo significativo.

    ![Mudar o nome de experimentação][5]
   
   > [!TIP]
   > É uma boa prática para preencher **resumo** e **Descrição** para a experimentação no **propriedades** painel. Estas propriedades dão-lhe a oportunidade de se documentar a experimentação, para que qualquer pessoa que olha para ela mais tarde será compreender suas metas e metodologia.
   > 
   > ![Propriedades de experimentação][6]
   > 
3. Na paleta do módulo para a esquerda da tela de experimentação, expanda **conjuntos de dados guardado**.
4. Localize o conjunto de dados que criou em **conjuntos de dados de meu** e arraste-o para a tela. Também pode encontrar o conjunto de dados ao introduzir o nome na **pesquisa** caixa acima a paleta.  

    ![Adicionar o conjunto de dados para a experimentação][7]

## <a name="prepare-the-data"></a>Preparar os dados
Pode ver as primeiras 100 linhas dos dados e algumas informações estatísticas para todo o conjunto de dados: Clique na porta de saída do conjunto de dados (o pequeno círculo na parte inferior) e selecione **Visualize**.  

Uma vez que o ficheiro de dados não vinha com cabeçalhos de coluna, Studio forneceu cabeçalhos genéricos (Col1, Col2, *etc.*). Boa cabeçalhos não são essenciais para a criação de um modelo, mas eles tornam mais fácil trabalhar com os dados na experimentação. Além disso, quando está a publicar, eventualmente, esse modelo num serviço da web, os títulos ajudam a identificar as colunas para o utilizador do serviço.  

Podemos adicionar cabeçalhos de coluna com o [Editar metadados] [ edit-metadata] módulo.
Utilizar o [Editar metadados] [ edit-metadata] módulo para alterar os metadados associados um conjunto de dados. Neste caso, é usada para fornecer nomes amigáveis mais para títulos de coluna. 

Para utilizar [Editar metadados][edit-metadata], primeiro tem de especificar as colunas que pretende modificar (no caso, todos eles.) Em seguida, especificar a ação a ser executada nessas colunas (no caso, alterar os cabeçalhos de coluna.)

1. Na paleta do módulo, escreva "metadados" a **pesquisa** caixa. O [Editar metadados] [ edit-metadata] aparece na lista de módulos.

2. Clique e arraste a [Editar metadados] [ edit-metadata] módulo para a tela e largue-o abaixo o conjunto de dados que adicionamos anteriormente.

3. Ligar o conjunto de dados para o [Editar metadados][edit-metadata]: clique na porta de saída do conjunto de dados (o pequeno círculo na parte inferior do conjunto de dados), arraste para a porta de entrada de [Editar metadados] [ edit-metadata] (o pequeno círculo na parte superior do módulo), em seguida, largue o botão do mouse. O módulo e o conjunto de dados permanecem conectados, mesmo se move qualquer um na tela.
   
   A experimentação deverá agora ser semelhante ao seguinte:  
   
   ![Adicionar metadados de edição][1]
   
   O ponto de exclamação vermelho indica que ainda não definimos as propriedades para este módulo ainda. Vamos fazer isso a seguir.
   
   > [!TIP]
   > Pode adicionar um comentário a um módulo, fazendo duplo clique no módulo e introduzindo o texto. Isto pode ajudá-lo a ver rapidamente o que o módulo está a fazer na sua experimentação. Neste caso, faça duplo clique nas [Editar metadados] [ edit-metadata] módulo e escreva os comentário "Adicionar cabeçalhos de coluna". Clique em qualquer outra coisa na tela para fechar a caixa de texto. Para apresentar o comentário, clique na seta para baixo no módulo.
   > 
   > ![Editar o módulo de metadados com o comentário adicionado][8]
   > 
4. Selecione [Editar metadados][edit-metadata]e, no **propriedades** painel à direita da tela, clique em **iniciar Seletor de colunas**.

5. Na **selecionar colunas** caixa de diálogo, selecione todas as linhas na **colunas disponíveis** e clique em > para movê-los para **Selected Columns**.
   A caixa de diálogo deve ter este aspeto:

   ![Seletor de colunas com todas as colunas selecionadas][2]

6. Clique nas **OK** marca de verificação.

7. De volta a **propriedades** painel, procure o **novos nomes das colunas** parâmetro. Neste campo, introduza uma lista de nomes para a 21 de colunas no conjunto de dados, separados por vírgulas e a ordem da coluna. Pode obter os nomes de colunas na documentação do conjunto de dados no site da UCI, ou para sua comodidade pode copiar e colar a lista seguinte:  
   
       Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  
   
   O painel de propriedades tem esta aparência:
   
   ![Propriedades de metadados de edição][3]

> [!TIP]
> Se pretender verificar os cabeçalhos de coluna, execute a experimentação (clique em **executar** abaixo a tela da experimentação). Quando ele for concluída, em execução (aparece uma marca de verificação verde [Editar metadados][edit-metadata]), clique na porta de saída do [Editar metadados] [ edit-metadata] módulo, e selecione **Visualize**. Pode ver a saída de qualquer módulo da mesma forma para ver o progresso dos dados através da experimentação.
> 
> 

## <a name="create-training-and-test-datasets"></a>A criação de treinamento e teste de conjuntos de dados
Precisamos de alguns dados para preparar o modelo e alguns para testá-lo.
Para que a próxima etapa da experimentação, dividimos o conjunto de dados em dois conjuntos de dados separados: um para o nosso modelo e outra para testá-lo de treinamento.

Para fazer isso, usamos o [Split Data] [ split] módulo.  

1. Encontrar o [Split Data] [ split] módulo, arraste-o para a tela e ligá-la para o [Editar metadados] [ edit-metadata] módulo.

2. Por predefinição, a proporção de divisão é 0,5 e o **Randomized split** parâmetro estiver definido. Isso significa que um meio aleatório dos dados é a saída por meio de uma porta do [Split Data] [ split] módulo e metade através de outro. Pode ajustar esses parâmetros, bem como o **seed aleatório** parâmetro, para alterar a divisão entre preparação e teste de dados. Neste exemplo, vamos deixá-los como-é.
   
   > [!TIP]
   > A propriedade **fração de linhas no primeiro conjunto de dados de saída** determina quanto dos dados de saída através da *esquerdo* porta de saída. Por exemplo, se definir a proporção de 0,7, em seguida, 70% dos dados é saída pela porta esquerda e 30% por meio da porta à direita.  
   > 
   > 

3. Clique duas vezes o [Split Data] [ split] módulo e introduza o comentário, "dados de treinamento/teste dividir 50%". 

Podemos usar as saídas da [Split Data] [ split] módulo no entanto, como, mas vamos optar por utilizar a saída à esquerda, como teste de dados de saída de dados de treinamento e à direita.  

Conforme mencionado na [passo anterior](walkthrough-2-upload-data.md), o custo de misclassifying um risco elevado crédito como baixa é cinco vezes maior do que o custo de misclassifying um risco de crédito baixa como elevado. Para justificar isso, podemos gerar um novo conjunto de dados reflete essa função de custo. No novo conjunto de dados, cada exemplo de alto risco é replicado cinco vezes, enquanto cada exemplo de baixo risco não é replicado.   

Podemos fazer essa replicação utilizam código R:  

1. Localize e arraste a [executar Script R] [ execute-r-script] módulo para a tela de experimentação. 

2. Ligue a porta de saída à esquerda do [Split Data] [ split] módulo para a primeira porta de entrada ("Dataset1") da [executar Script do R] [ execute-r-script] módulo.

3. Faça duplo clique o [executar Script R] [ execute-r-script] módulo e introduza o comentário "Conjunto de ajuste de custo".

4. Na **propriedades** painel, elimine o texto predefinido no **R Script** parâmetro e introduza este script:
   
       dataset1 <- maml.mapInputPort(1)
       data.set<-dataset1[dataset1[,21]==1,]
       pos<-dataset1[dataset1[,21]==2,]
       for (i in 1:5) data.set<-rbind(data.set,pos)
       maml.mapOutputPort("data.set")

    ![Script do R no módulo executar Script R][9]

Precisamos fazer essa mesma operação de replicação para cada resultado do [Split Data] [ split] módulo para que os dados de treinamento e testes tenham o mesma ajuste de custo. A maneira mais fácil de fazê-lo é ao duplicar o [executar Script R] [ execute-r-script] módulo que acabou de criar e ligar a outra porta de saída a [Split Data] [ split] módulo.

1. Com o botão direito a [executar Script R] [ execute-r-script] módulo e selecione **cópia**.

2. A tela de experimentação com o botão direito e selecione **colar**.

3. Arraste o novo módulo para a posição pretendida e, em seguida, ligue a porta de saída de direito do [Split Data] [ split] módulo para a primeira porta de entrada deste novo [executar Script do R] [ execute-r-script] módulo. 

4. Na parte inferior da tela, clique em **executar**. 

> [!TIP]
> A cópia do módulo executar Script R contém o mesmo script como o módulo original. Quando copiar e colar um módulo na tela, a cópia retém todas as propriedades do original.  
> 
> 

Nosso experimentação agora terá um aspeto semelhante ao seguinte:

![Adicionar o módulo de divisão e R scripts][4]

Para obter mais informações sobre como utilizar os R scripts no suas experimentações, consulte [expandir a sua experiência com R](extend-your-experiment-with-r.md).

**Seguinte: [Dar formação e avaliar os modelos](walkthrough-4-train-and-evaluate-models.md)**

[0]: ./media/walkthrough-3-create-new-experiment/create-new-experiment.png
[5]: ./media/walkthrough-3-create-new-experiment/rename-experiment.png
[6]: ./media/walkthrough-3-create-new-experiment/experiment-properties.png
[7]: ./media/walkthrough-3-create-new-experiment/add-dataset-to-experiment.png
[8]: ./media/walkthrough-3-create-new-experiment/edit-metadata-with-comment.png
[9]: ./media/walkthrough-3-create-new-experiment/execute-r-script.png
[1]: ./media/walkthrough-3-create-new-experiment/experiment-with-edit-metadata-module.png
[2]: ./media/walkthrough-3-create-new-experiment/select-columns.png
[3]: ./media/walkthrough-3-create-new-experiment/edit-metadata-properties.png
[4]: ./media/walkthrough-3-create-new-experiment/experiment.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
