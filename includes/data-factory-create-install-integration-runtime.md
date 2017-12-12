## <a name="create-a-self-hosted-ir"></a>Criar um IR autoalojado

Nesta secção, vai criar um integration runtime autoalojado e vai associá-lo a um computador no local com a base de dados do SQL Server. O runtime de integração autoalojado é o componente que copia os dados do SQL Server no seu computador para o armazenamento de blobs do Azure. 

1. Crie uma variável para o nome do runtime de integração. Utilize um nome exclusivo e aponte-o. Vai utilizá-lo mais tarde no tutorial. 

    ```powershell
   $integrationRuntimeName = "ADFTutorialIR"
    ```
1. Crie um integration runtime autoalojado. 

   ```powershell
   Set-AzureRmDataFactoryV2IntegrationRuntime -Name $integrationRuntimeName -Type SelfHosted -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName
   ```

   Segue-se o resultado do exemplo:

   ```json
    Id                : /subscriptions/<subscription ID>/resourceGroups/ADFTutorialResourceGroup/providers/Microsoft.DataFactory/factories/onpremdf0914/integrationruntimes/myonpremirsp0914
    Type              : SelfHosted
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Name              : myonpremirsp0914
    Description       :
    ```
 

2. Execute o seguinte comando para obter o estado do integration runtime criado. Confirme que o valor da propriedade **Estado** está definido como **NeedRegistration**. 

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntime -name $integrationRuntimeName -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Status
   ```

   Segue-se o resultado do exemplo:

   ```json
   Nodes                     : {}
   CreateTime                : 9/14/2017 10:01:21 AM
   InternalChannelEncryption :
   Version                   :
   Capabilities              : {}
   ScheduledUpdateDate       :
   UpdateDelayOffset         :
   LocalTimeZoneOffset       :
   AutoUpdate                :
   ServiceUrls               : {eu.frontend.clouddatahub.net, *.servicebus.windows.net}
   ResourceGroupName         : <ResourceGroup name>
   DataFactoryName           : <DataFactory name>
   Name                      : <Integration Runtime name>
   State                     : NeedRegistration
   ```

3. Execute o seguinte comando para obter as **chaves de autenticação** para registar o integration runtime autoalojado no serviço Data Factory na cloud. 

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntimeKey -Name $integrationRuntimeName -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName | ConvertTo-Json
   ```

   Segue-se o resultado do exemplo:

   ```json
   {
       "AuthKey1":  "IR@0000000000-0000-0000-0000-000000000000@xy0@xy@xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx=",
       "AuthKey2":  "IR@0000000000-0000-0000-0000-000000000000@xy0@xy@yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy="
   }
   ```    
4. Copie uma das chaves (exclua as aspas duplas) para registar o runtime de integrçaão autoalojado que vai instalar no computador no passo seguinte:  

## <a name="install-integration-runtime"></a>Instalar o runtime de integração
1. Se já tiver o **Microsoft Integration Runtime** no seu computador, desinstale-o utilizando **Adicionar ou Remover Programas**. 
2. [Transfira](https://www.microsoft.com/download/details.aspx?id=39717) o runtime de integração autoalojado num computador Windows local e execute a instalação. 
3. Em **Bem-vindo ao Assistente de Configuração do Microsoft Integration Runtime** , clique em **Seguinte**.  
4. Na página **Contrato de Licença do Utilizador Final** , aceite os termos e o contrato de licença e clique em **Seguinte**. 
5. Na página **Pasta de Destino**, clique em **Seguinte**. 
6. Em **Pronto para instalar o Microsoft Integration Runtime**, clique em **Instalar**. 
7. Se vir uma mensagem de erro que diz que o computador está a ser configurado para entrar no modo de suspensão ou hibernação quando não estiver a ser utilizado, clique em **OK**. 
8. Se vir a janela **Opções de Energia**, feche-a e mude para a janela de configuração. 
9. Na página **Concluiu o Assistente de Configuração do Microsoft Integration Runtime**, clique em **Concluir**.
10. Na página **Registar o Integration Runtime (Autoalojado)**, cole a chave que guardou na secção anterior e clique em **Registar**. 

   ![Registar o integration runtime](media/data-factory-create-install-integration-runtime/register-integration-runtime.png)
2. Quando o integration runtime autoalojado for registado com êxito, verá a mensagem seguinte:

   ![Registado com êxito](media/data-factory-create-install-integration-runtime/registered-successfully.png)

3. Na página **Novo Nó de Integration Runtime (Autoalojado)**, clique em **Seguinte**. 

    ![Página Novo Nó de Integration Runtime](media/data-factory-create-install-integration-runtime/new-integration-runtime-node-page.png)
4. No **Canal de Comunicação da Intranet**, clique em **Ignorar**. Pode selecionar a certificação TLS/SSL para proteger a comunicação intra-nós num ambiente de runtime de integração de vários nós. 

    ![Página de cala de comunicação da Intranet](media/data-factory-create-install-integration-runtime/intranet-communication-channel-page.png)
5. Na página **Registar Integration Runtime (Autoalojado)**, clique em **Configuration Manager**. 
6. Quando o nó for ligado ao serviço cloud, verá a página seguinte:

   ![O nó está ligado](media/data-factory-create-install-integration-runtime/node-is-connected.png)
7. Agora, teste a conectividade à base de dados do SQL Server.

    ![Separador Diagnóstico](media/data-factory-create-install-integration-runtime/config-manager-diagnostics-tab.png)   

    - Na janela do **Configuration Manager**, mude para o separador **Diagnósticos**.
    - Selecione **SqlServer** em **Tipo de origem de dados**.
    - Introduza o nome do **servidor**.
    - Introduza o nome da **base de dados**. 
    - Selecione o modo de **autenticação**. 
    - Introduza o nome de **utilizador**. 
    - Introduza a **palavra-passe** do nome de utilizador.
    - Clique em **Testar** para confirmar que o runtime de integração se consegue ligar ao SQL Server. Verá uma marca de verificação verde se a ligação for bem-sucedida. Caso contrário, verá uma mensagem de erro associada a uma falha. Corrija os problemas e confirme que o runtime de integração se consegue ligar ao seu SQL Server.    

    > [!NOTE]
    > Aponte estes valores (tipo de autenticação, servidor, base de dados, utilizador, palavra-passe). Vai utilizá-los mais tarde no tutorial. 
    
