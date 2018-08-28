Agora que adicionou um acionador, é hora de fazer algo interessante com os dados que são gerados pelo acionador. Siga estes passos para adicionar o **SharePoint Online - criar ficheiro** ação. Esta ação irá criar um ficheiro no SharePoint Online cada vez que o novo item é acionado. 

Para configurar esta ação, terá de fornecer as informações seguintes. Como fornecer estas informações, notará que ele é fácil de usar dados gerados pelo acionador como entrada para algumas das propriedades para o novo ficheiro:

| Criar propriedade de ficheiro | Descrição |
| --- | --- |
| URL do Site |Este é o URL do site SharePoint Online onde pretende criar o novo ficheiro. Selecione o site na lista apresentada. |
| Caminho da pasta |Esta é a pasta (no URL de Site selecionados no passo anterior) onde o novo ficheiro será colocado. Procure e selecione a pasta. |
| Nome de ficheiro |Este é o nome do ficheiro a ser criado. |
| Conteúdo do ficheiro |O conteúdo que será gravado no arquivo. |

1. Selecione **+ novo passo** para adicionar a ação.  
   ![Imagem de ação online do SharePoint 1](./media/connectors-create-api-sharepointonline/action-1.png)  
2. Selecione o **adicionar uma ação** ligação. Ela abre a caixa de pesquisa, onde pode procurar qualquer ação gostaria de fazer. Neste exemplo, ações do SharePoint são de interesse.    
   ![Imagem de ação online do SharePoint 2](./media/connectors-create-api-sharepointonline/action-2.png)    
3. Introduza *sharepoint* para procurar ações relacionadas com o SharePoint.
4. Selecione **SharePoint Online - criar ficheiro** como a ação a tomar.   **Tenha em atenção**: será solicitado para autorizar a aplicação lógica para aceder à sua conta de SharePoint, se não tiver criado anteriormente uma ligação ao SharePoint Online.    
   ![Imagem de ação online do SharePoint 3](./media/connectors-create-api-sharepointonline/action-3.png)    
5. O **criar ficheiro** controlar abre-se.   
   ![Imagem de ação online do SharePoint 4](./media/connectors-create-api-sharepointonline/action-4.png)     
6. Selecione **URL do Site** e procure para localizar o site onde pretende criar o ficheiro.     
   ![Imagem de ação online do SharePoint 5](./media/connectors-create-api-sharepointonline/action-5.png)  
7. Selecione **caminho da pasta** e procure para localizar a pasta onde o novo ficheiro será colocado.  
   ![Imagem de ação online do SharePoint 6](./media/connectors-create-api-sharepointonline/action-6.png)  
8. Selecione o **nome de ficheiro** controlar e introduza o nome do ficheiro que pretende criar. Aqui, pode introduzir o nome de ficheiro diretamente ou pode utilizar qualquer uma das propriedades de Acionador que criou anteriormente. Fazê-lo ao selecionar as propriedades da lista de **saídas de quando é criado um novo item**. Esta lista é apresentar apenas depois de selecionar o **nome de ficheiro** controle. Este guia, selecionei ID (ID do novo item de lista) como o nome do ficheiro a ser criado pelos **SharePoint Online - criar ficheiro** ação.    
   ![Imagem de ação online do SharePoint 7](./media/connectors-create-api-sharepointonline/action-7.png)  
9. Selecione o **conteúdo do ficheiro** controlar e introduza o conteúdo que será escrito para o ficheiro que será criado. Para o conteúdo do ficheiro, tenha em atenção que pode utilizar qualquer uma das propriedades de Acionador que criou anteriormente. Basta Selecione as propriedades da lista apresentada. Em alternativa, pode introduzir a **conteúdo do ficheiro** texto diretamente no controlo. Neste exemplo, selecionei a algumas propriedades e adicionar espaços e um hífen entre cada propriedade.        
   ![Imagem de ação online do SharePoint 8](./media/connectors-create-api-sharepointonline/action-8.png)  
10. Guardar as alterações ao seu fluxo de trabalho  
11. Parabéns, agora tem uma aplicação lógica totalmente funcional que é acionada quando é adicionado um novo item numa lista do SharePoint Online. A aplicação, em seguida, criará um arquivo, usando algumas das propriedades do novo item de lista.  Agora pode testá-lo através da criação de um novo item na lista do SharePoint. 

