Siga estes passos para instalar e executar o MongoDB numa máquina virtual com o Windows Server.

> [!IMPORTANT]
> Recursos de segurança do MongoDB, como a autenticação e o enlace de endereço IP, não estão ativados por predefinição. Recursos de segurança devem ser ativados antes de implantar o MongoDB num ambiente de produção.  Para obter mais informações, consulte [segurança e autenticação](http://www.mongodb.org/display/DOCS/Security+and+Authentication).
>
>

1. Depois de se ligar à máquina virtual com o ambiente de trabalho remoto, abra o Internet Explorer a partir da **iniciar** menu na máquina virtual.
2. Selecione o **ferramentas** botão no canto superior direito.  Na **opções da Internet**, selecione a **Security** separador e, em seguida, selecione o **Sites confiáveis** ícone e, finalmente, clique o **Sites** botão. Adicione *https://\*. mongodb.org* à lista de sites fidedignos.
3. Aceda a [Downloads - MongoDB](https://www.mongodb.com/download-center#community).
4. Encontrar o **versão estável atual** dos **Community Server**, selecione a versão mais recente **64-bit** versão na coluna Windows. Transferir e, em seguida, execute o Instalador MSI.
5. MongoDB é normalmente instalado em C:\Program Files\MongoDB. Procurar as variáveis de ambiente no ambiente de trabalho e adicionar o caminho de binários do MongoDB à variável PATH. Por exemplo, talvez os binários em C:\Program Files\MongoDB\Server\3.4\bin no seu computador.
6. Criar diretórios de dados e de registo do MongoDB no disco de dados (por exemplo, a unidade **f:**) que criou nos passos anteriores. Partir **começar**, selecione **linha de comandos** para abrir uma janela de linha de comandos.  Escreva:

        C:\> F:
        F:\> mkdir \MongoData
        F:\> mkdir \MongoLogs
7. Para executar a base de dados, execute:

        F:\> C:
        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log

    Todas as mensagens de registo são direcionadas para o *F:\MongoLogs\mongolog.log* de ficheiros como mongod.exe servidor é iniciado e preallocates ficheiros de diário. Pode demorar vários minutos para o MongoDB pré-alocar os ficheiros de diário e começar a escutar ligações. A linha de comandos permanece focada nesta tarefa enquanto a instância do MongoDB está em execução.
8. Para iniciar o shell do MongoDB administrativo, abra outra janela de comando da **iniciar** e escreva os seguintes comandos:

        C:\> cd \my_mongo_dir\bin  
        C:\my_mongo_dir\bin> mongo  
        >db  
        test
        > db.foo.insert( { a : 1 } )  
        > db.foo.find()  
        { _id : ..., a : 1 }  
        > show dbs  
        ...  
        > show collections  
        ...  
        > help  

    A base de dados é criado pela inserção.
9. Em alternativa, pode instalar mongod.exe como um serviço:

        C:\> mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log --logappend  --install

    Um serviço é instalado com o nome MongoDB com uma descrição de "Mongo DB". O `--logpath` opção tem de ser utilizada para especificar um ficheiro de registo, uma vez que o serviço em execução não tenha uma janela de comando para apresentar os resultados.  O `--logappend` opção especifica que um reinício do serviço faz com que a saída acrescentar ao ficheiro de registo existente.  O `--dbpath` opção especifica a localização do diretório de dados. Para opções de linha de comando mais com o serviço, veja [relacionadas com o serviço opções da linha de comandos][MongoWindowsSvcOptions].

    Para iniciar o serviço, execute este comando:

        C:\> net start MongoDB
10. Agora que MongoDB está instalado e em execução, precisa abrir uma porta na Firewall do Windows para que possa ligar remotamente ao MongoDB.  Do **começar** menu, selecione **ferramentas administrativas** e, em seguida **Firewall do Windows com segurança avançada**.
11. a) no painel esquerdo, selecione **regras de entrada**.  Na **ações** painel à direita, selecione **nova regra...** .

    ![Firewall do Windows][Image1]

    b) além da **Assistente de nova regra de entrada**, selecione **porta** e, em seguida, clique em **seguinte**.

    ![Firewall do Windows][Image2]

    c) selecione **TCP** e, em seguida **portas locais específicas**.  Especifique uma porta de "27017" (a porta padrão escuta a MongoDB) e clique em **seguinte**.

    ![Firewall do Windows][Image3]

    d) selecione **permitir a conexão** e clique em **próxima**.

    ![Firewall do Windows][Image4]

    e) clique **seguinte** novamente.

    ![Firewall do Windows][Image5]

    f) Especifique um nome para a regra, tal como "MongoPort" e clique em **concluir**.

    ![Firewall do Windows][Image6]

12. Se não configurar um ponto final para o MongoDB quando criou a máquina virtual, pode fazê-lo agora. Terá da regra de firewall e o ponto final para conseguir ligar remotamente ao MongoDB.

  No portal do Azure, clique em **máquinas virtuais (clássicas)**, clique no nome da sua nova máquina virtual e, em seguida, clique em **pontos de extremidade**.

    ![Pontos Finais][Image7]

13. Clique em **Adicionar**.

14. Adicionar um ponto final com o nome "Mongo", protocolo **TCP**e ambos **público** e **privada** portas definido como "27017". Abrir essa porta permite que o MongoDB ser acessado remotamente.

    ![Pontos Finais][Image9]

> [!NOTE]
> A porta 27017 é a porta predefinida utilizada por MongoDB. Pode alterar esta porta predefinida, especificando o `--port` parâmetro ao iniciar o servidor de mongod.exe. Certifique-se fornecer o mesmo número de porta na firewall e o ponto final de "Mongo" nas instruções anteriores.
>
>

[MongoDownloads]: http://www.mongodb.org/downloads

[MongoWindowsSvcOptions]: http://www.mongodb.org/display/DOCS/Windows+Service


[Image1]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall1.png
[Image2]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall2.png
[Image3]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall3.png
[Image4]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall4.png
[Image5]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall5.png
[Image6]: ./media/install-and-run-mongo-on-win2k8-vm/WinFirewall6.png
[Image7]: ./media/install-and-run-mongo-on-win2k8-vm/menusendpointadd.png
<!-- Removed 03/08/2017. Not in new portal. -->
<!-- [Image8]: ./media/install-and-run-mongo-on-win2k8-vm/WinVmAddEndpoint2.png
-->
[Image9]: ./media/install-and-run-mongo-on-win2k8-vm/newendpointdetails.png
