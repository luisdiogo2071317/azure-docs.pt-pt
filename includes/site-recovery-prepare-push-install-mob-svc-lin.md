### <a name="prepare-for-a-push-installation-on-a-linux-server"></a>Preparar uma instalação por push num servidor Linux

1. Certifique-se de que existe conectividade de rede entre o computador Linux e o servidor de processos.
2. Crie uma conta que possa ser utilizada pelo servidor de processos para aceder ao computador. A conta deve ser um utilizador **raiz** no servidor Linux de origem. Utilize esta conta apenas para a instalação de push e para as atualizações.
3. Verifique se o ficheiro /etc/hosts no servidor Linux de origem tem entradas que mapeiam o nome de anfitrião local para endereços IP associados a todos os adaptadores de rede.
4. Instale os pacotes openssh, openssh-server e openssl mais recentes no computador que quer replicar.
5. Certifique-se de que o Secure Shell (SSH) está ativado e em execução na porta 22.
6. Ative a autenticação de subsistema e a palavra-passe SFTP no ficheiro sshd_config. Siga estes passos.

    a. Inicie sessão como **raiz**.

    b. No **/etc/ssh/sshd_config** ficheiro, localize a linha que começa com **PasswordAuthentication**.

    c. Anule o comentário da linha e altere o valor para **Sim**.

    d. Localizar a linha que começa com **subsistema**, e anule o comentário da linha.

      ![Linux](./media/site-recovery-prepare-push-install-mob-svc-lin/mobility2.png)

    e. Reinicie o serviço **sshd**.

7. Adicione a conta que criou no CSPSConfigtool. Siga estes passos.

    a. Inicie sessão no servidor de configuração.

    b. Abra **cspsconfigtool.exe**. Está disponível como um atalho no ambiente de trabalho e na pasta %ProgramData%\home\svsystems\bin.

    c. No **gerir contas** separador, selecione **adicionar conta**.

    d. Adicione a conta que criou.

    d. Introduza as credenciais que utiliza para ativar a replicação para um computador.
