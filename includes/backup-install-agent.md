## <a name="download-install-and-register-the-azure-backup-agent"></a>Transferir, instalar e registar o agente de cópia de segurança do Azure
Depois de criar o Cofre dos serviços de recuperação, instale o agente de cópia de segurança em cada uma das suas máquinas do Windows (Windows Server, cliente Windows, o servidor do System Center Data Protection Manager ou máquina do servidor de cópia de segurança do Azure) utilizado para fazer cópias de segurança de dados do Azure.

1. Abra a sua subscrição a [Portal do Azure](https://ms.portal.azure.com/).
2. No menu da esquerda, selecione **todos os serviços** e na lista de serviços, escreva **dos serviços de recuperação**. Clique em **cofres dos serviços de recuperação**.

   ![Abra o Cofre dos serviços de recuperação](../articles/backup/media/tutorial-backup-windows-server-to-azure/full-browser-open-rs-vault_2.png)
3. Na página início rápido, clique o **cliente para o Windows Server ou o System Center Data Protection Manager ou o Windows** opção em **Transferir agente**. Clique em **guardar** para copiá-lo para o computador local.
   
    ![Guardar o agente](./media/backup-install-agent/agent.png)
4. Assim que o agente estiver instalado, faça duplo clique MARSAgentInstaller.exe para iniciar a instalação do agente do Backup do Azure. Escolha a pasta de instalação e a pasta scratch necessário para o agente. A localização da cache especificado deve ser, pelo menos, 5% dos dados de cópia de segurança como espaço livre.
5. Se utilizar um servidor proxy para ligar à internet, o **configuração de Proxy** ecrã, introduza os detalhes do servidor proxy. Se utilizar um proxy autenticado, introduza os detalhes de nome e palavra-passe do utilizador neste ecrã.
6. O agente do Backup do Azure instala .NET Framework 4.5 e o Windows PowerShell (se ainda não estiver disponível) para concluir a instalação.
7. Assim que o agente estiver instalado, clique em de **avançar para o registo** botão para continuar com o fluxo de trabalho.
   
   ![Registar](./media/backup-install-agent/register.png)
8. No ecrã de credenciais de cofre, selecione o ficheiro de credenciais do cofre transferidas.
   
    ![Credenciais do Cofre](./media/backup-install-agent/vc.png)
   
    O ficheiro de credenciais do cofre só é válido para 48 horas (após a transferência do portal). Se encontrar qualquer erro este ecrã (por exemplo "ficheiro de credenciais de cofre fornecido expirou"), início de sessão no portal do Azure e transfira o ficheiro de credenciais do cofre novamente.
   
    Certifique-se de que a aplicação de configuração pode aceder ao ficheiro de credenciais de cofre. Se ocorrerem erros relacionados com o acesso, copie o ficheiro de credenciais do cofre para uma localização temporária no computador local e repita a operação.
   
    Se tiver um cofre inválidas credencial erros, tais como "credenciais de cofre inválidas fornecidas", o Cofre de ficheiro de credenciais está danificado ou não tem as credenciais mais recentes associado o serviço de recuperação. Repita a operação depois de transferir um novo ficheiro de credenciais do cofre a partir do portal. Este erro geralmente é visto quando um utilizador clica o **credenciais do Cofre de transferência** opção no portal do Azure, sucessivamente rápida. Quando isto acontecer, apenas o ficheiro de credenciais de cofre segundo é válido.
9. No **definição de encriptação** ecrã, pode gerar uma frase de acesso ou fornecer uma frase de acesso (mínimo de 16 carateres). Lembre-se guardar o frase de acesso numa localização segura.
   
    ![Encriptação](./media/backup-install-agent/encryption.png)
   
   > [!WARNING]
   > Se o frase de acesso de perda ou esquecimento; Não pode ajudar a Microsoft recuperar os dados de cópia de segurança. O utilizador final possui a frase de acesso de encriptação e a Microsoft não têm visibilidade para a frase de acesso utilizada pelo utilizador final. Guarde o ficheiro numa localização segura, conforme necessário durante uma operação de recuperação.
   > 
   > 
10. Assim que clicar no **concluir** botão, a máquina está registada com êxito para o Cofre e está agora pronto para iniciar a cópia de segurança em Microsoft Azure.
11. Ao utilizar o Microsoft Azure Backup autónomo, pode modificar as definições especificadas durante o fluxo de trabalho de registo clicando no **alterar propriedades** opção no snap MMC de cópia de segurança do Azure.
    
    ![Alterar propriedades](./media/backup-install-agent/change.png)
    
    Em alternativa, ao utilizar o Data Protection Manager, pode modificar as definições especificadas durante o fluxo de trabalho de registo clicando a **configurar** opção selecionando **Online** sob o **Gestão** separador.
    
    ![Configurar o Azure Backup](./media/backup-install-agent/configure.png)

