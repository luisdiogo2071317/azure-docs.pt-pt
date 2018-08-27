Agora que adicionou uma condição, é hora de fazer algo interessante com os dados que são gerados pelo acionador. Siga estes passos para adicionar o **Salesforce - obter objeto** ação. Esta ação irá obter os dados sempre que uma nova oportunidade potencial for criada. Também irá adicionar uma segunda ação que irá utilizar os dados do Salesforce - obter um objeto de ação para enviar um e-mail com o conector do Office 365.  

Para configurar esta ação, terá de fornecer as informações seguintes. Notará que ele é fácil de usar dados gerados pelo acionador como entrada para algumas das propriedades para o novo ficheiro:

| Criar propriedade de ficheiro | Descrição |
| --- | --- |
| Tipo de objeto |Este é o tipo de objeto do Salesforce que está interessado. Os exemplos são oportunidade potencial, conta, etc. |
| ID de objeto |Isso representa um identificador para o objeto. |

1. Selecione **adicionar uma ação** ligação. Ela abre a caixa de pesquisa, onde pode procurar qualquer ação gostaria de fazer. Neste exemplo, as ações de Salesforce são de interesse.      
   ![Imagem da ação de Salesforce 1](./media/connectors-create-api-salesforce/action-1.png)  
2. Introduza *salesforce* para procurar ações relacionadas com o salesforce.
3. Selecione **Salesforce - obter objeto** como a ação a tomar.   **Tenha em atenção**: será solicitado para autorizar a aplicação lógica para aceder à sua conta do Salesforce, se não o tiver feito isso anteriormente.    
   ![Imagem da ação de Salesforce 2](./media/connectors-create-api-salesforce/action-2.png)    
4. O **obter objeto** controlar abre-se.  
5. Selecione *levar* como o tipo de objeto.
6. Selecione o **ID de objeto** controle.
7. Selecione **...**  para expandir a lista de tokens que pode ser utilizado como entrada para ações.       
   ![Imagem da ação de Salesforce 3](./media/connectors-create-api-salesforce/action-3.png)    
8. Selecione **levar ID** controlar abre-se.   
   ![Imagem da ação de Salesforce 4](./media/connectors-create-api-salesforce/action-4.png)     
9. Tenha em atenção que o token de ID de levar está agora no controlo da ID de objeto, que indica que a ação de objeto de Get irá procurar uma oportunidade potencial com um ID de é igual para o ID da oportunidade potencial do cliente potencial que disparou esta aplicação lógica.  
   ![Imagem da ação de Salesforce 5](./media/connectors-create-api-salesforce/action-5.png)  
10. Guarde o seu trabalho. É isso, adicionou a ação de objeto de Get para a sua aplicação lógica. Seu controle de objeto de Get deve ter este aspeto:    
    ![Imagem da ação de Salesforce 6](./media/connectors-create-api-salesforce/action-6.png)  

Agora que adicionou uma ação para obter uma oportunidade potencial, pode querer fazer algo interessante com o líder de recém-criado. Numa empresa, pode querer enviar um e-mail para notificar uma lista de distribuição que foi criada uma nova oportunidade potencial. Vamos utilizar o conector do Office 365 para enviar um e-mail com algumas das informações relevantes do objeto nova oportunidade potencial no Salesforce.  

1. Selecione **adicionar uma ação** , em seguida, introduza *e-mail* no controle de pesquisa. Este procedimento filtra as ações para aqueles que estão relacionados para enviar e receber correio eletrónico.  
2. Selecione o **Office 365 Outlook - enviar um e-mail** item da lista. Se ainda não tiver criado uma *ligação* à sua conta do Office 365, será solicitado que introduza as credenciais do Office 365 para criá-lo agora. Depois de terminar, o **enviar um e-mail** controlar abre-se.        
   ![Imagem da ação de Salesforce 7](./media/connectors-create-api-salesforce/action-7.png)  
3. Introduza o endereço de e-mail que gostaria de enviar o e-mail na **para** controle.
4. Na **assunto** controlar, introduza *levar novo criado* –, em seguida, selecione o *empresa* token. Isso exibirá a *empresa* campo da nova oportunidade potencial criada no Salesforce.  
5. Na **corpo** controlo, pode selecionar qualquer um dos tokens a partir do novo objeto de oportunidades potenciais e também pode introduzir qualquer texto que pretende apresentar no corpo da mensagem de e-mail. Segue-se um exemplo:  
   ![Imagem da ação de Salesforce 8](./media/connectors-create-api-salesforce/action-8.png)   
6. Guarde o fluxo de trabalho.  

E já está. A aplicação lógica está agora concluída.  

Agora, pode testar a aplicação lógica: no Salesforce, criar uma nova oportunidade potencial que atenda à condição que criou.  Se seguiu totalmente nestas instruções, basta criar uma oportunidade potencial com um endereço de e-mail que contém *amazon.com* nela. Após alguns segundos a sua aplicação lógica deve ser acionada e os resultados podem ter um aspeto semelhantes a este:  
![Imagem da ação de Salesforce 9](./media/connectors-create-api-salesforce/action-9.png)  

