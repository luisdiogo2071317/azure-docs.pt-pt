Agora que adicionou um acionador, é hora de fazer algo interessante com os dados que são gerados pelo acionador. Siga estes passos para adicionar um a **SFTP - extrair pasta** ação. Esta ação irá extrair o conteúdo de um ficheiro, se estiverem reunidas as condições definidas. 

Para configurar esta ação, terá de fornecer as informações seguintes. Notará que ele é fácil de usar dados gerados pelo acionador como entrada para algumas das propriedades para o novo ficheiro:

| SFTP - propriedade da pasta de extração | Descrição |
| --- | --- |
| Caminho do ficheiro de arquivo de origem |Este é o caminho para o arquivo sendo extraído. Pode selecionar um dos tokens de uma ação anterior ou procurar o servidor SFTP para localizar o caminho do ficheiro. |
| Caminho da pasta de destino |Este é o caminho onde serão colocados os ficheiros extraídos. Pode selecionar um dos tokens a partir de uma ação anterior como o caminho de destino ou procurar o servidor SFTP e selecione um caminho. |
| Substituir? |Indica se um ficheiro com o mesmo nome que o arquivo extraído for encontrado no caminho da pasta de destino, se o ficheiro existente deve ser substituído ou não. |

Vamos começar ao adicionar a ação para extrair os ficheiros, se a condição definida anteriormente é avaliada como *True*. 

1. Selecione **adicionar uma ação**.        
   ![Imagem de condição de ação do SFTP 6](./media/connectors-create-api-sftp/condition-6.png)   
2. Selecione o **SFTP - extrair pasta** ação      
   ![Imagem de condição de ação do SFTP 7](./media/connectors-create-api-sftp/condition-7.png)   
3. Selecione **caminho de ficheiro de arquivo de origem**              
   ![Imagem de condição de ação do SFTP 9](./media/connectors-create-api-sftp/condition-9.png)   
4. Selecione o **caminho do ficheiro** token. Isto indica que irá utilizar o caminho do ficheiro do ficheiro que o acionador foi encontrado como o caminho de ficheiro de arquivo de origem.           
   ![Imagem de condição de ação do SFTP 10](./media/connectors-create-api-sftp/condition-10.png)   
5. Selecione **caminho da pasta de destino**           
   ![Imagem de condição de ação do SFTP 11](./media/connectors-create-api-sftp/condition-11.png)   
6. Selecione o **caminho do ficheiro** token. Isto indica que irá utilizar o caminho do ficheiro do ficheiro que o acionador foi encontrado como o caminho de destino para os ficheiros extraídos.   
7. Introduza *\ExtractedFile* no **caminho da pasta de destino** controle. Faça-o apenas após o token de caminho de arquivo no controle de caminho de pasta de destino.         
   ![Imagem de condição de ação do SFTP 12](./media/connectors-create-api-sftp/condition-12.png)   
8. Introduza *True* no **substituir?* controle para indicar que os ficheiros existentes devem ser substituídos se tiverem o mesmo nome que os ficheiros extraídos.      
   ![Imagem de condição de ação do SFTP 13](./media/connectors-create-api-sftp/condition-13.png)   
9. Guardar as alterações ao seu fluxo de trabalho  

