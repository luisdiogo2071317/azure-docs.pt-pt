Nestas instruções, irá aprender a utilizar o **Salesforce - quando é criado um objeto** acionador para iniciar um fluxo de trabalho de aplicação lógica quando uma nova oportunidade potencial é criada no Salesforce.

> [!NOTE]
> Será solicitado que inicie sessão sua conta do Salesforce se não tiver já criado uma *ligação* ao Salesforce.  
> 
> 

1. Introduza *salesforce* na caixa de pesquisa no designer de aplicações lógicas, em seguida, selecione a **Salesforce - quando é criado um objeto** acionador.  
   ![Imagem de Acionador de Salesforce 1](./media/connectors-create-api-salesforce/trigger-1.png)   
2. O **quando é criado um objeto** controlo é apresentado.  
   ![Imagem de Acionador de Salesforce 2](./media/connectors-create-api-salesforce/trigger-2.png)   
3. Selecione o **tipo de objeto** , em seguida, selecione *originar* na lista de objetos. Neste passo está indicando que está a criar um acionador que o irão notificar a aplicação lógica, sempre que uma nova oportunidade potencial for criada no Salesforce.   
   ![Imagem de Acionador de Salesforce 3](./media/connectors-create-api-salesforce/trigger-3.png)   
4. E já está. Acabou de criar o acionador. No entanto, terá de criar pelo menos uma ação para que isso seja uma aplicação lógica válida.    
   ![Imagem de Acionador de Salesforce 4](./media/connectors-create-api-salesforce/trigger-4.png)   

Neste momento, a aplicação lógica foi configurada com um acionador que vai iniciar uma execução de outros acionadores e ações do fluxo de trabalho quando um novo item é criado no Salesforce.  

