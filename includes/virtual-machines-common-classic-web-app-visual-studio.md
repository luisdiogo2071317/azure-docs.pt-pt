

Quando cria um projeto de aplicativo web para o Azure, pode aprovisionar uma máquina virtual no Azure. Em seguida, pode configurar a máquina virtual com o software adicional, ou utilizar a máquina virtual para fins de depuração ou diagnóstico.

Para criar uma máquina virtual quando cria uma aplicação web, siga estes passos:

1. No Visual Studio, clique em **arquivo** > **New** > **projeto** > **Web**e, em seguida, escolha **Aplicação Web ASP.NET** (sob a **Visual c#** ou **Visual Basic** nós).
2. Na **novo projeto ASP.NET** caixa de diálogo, selecione o tipo de aplicação web que pretende e, na secção do Azure da caixa de diálogo (no canto inferior direito), certifique-se de que o **alojar na nuvem** é a caixa de verificação selecionado (esta caixa de verificação está etiquetada **criar recursos remotos** em algumas instalações).
   
    ![][0]
3. Para este exemplo, na lista pendente, no Microsoft Azure, escolha **Máquina Virtual (v1)** e, em seguida, clique nas **OK** botão.
4. Se lhe for pedido, inicie sessão no Azure. O **criar Máquina Virtual** é apresentada a caixa de diálogo.
   
    ![][2]
5. Na **nome DNS** , introduza um nome para a máquina virtual. O nome DNS deve ser exclusivo no Azure. Se o nome que introduziu não estiver disponível, é apresentado um ponto de exclamação vermelho.
6. Na **imagem** lista, selecione a imagem que pretende basear a máquina virtual. Pode escolher qualquer uma das imagens de máquina virtual do Azure standard ou a imagem que carregou para o Azure.
7. Deixe o **ativar o IIS e o Web Deploy** caixa de verificação selecionada, a menos que planeia instalar um servidor web diferente. Não será possível publicar a partir do Visual Studio se desativar o Web Deploy. Pode adicionar o IIS e o Web Deploy a qualquer uma das imagens do Windows Server em pacote, incluindo as suas próprias imagens personalizadas.
8. Na **tamanho** lista, escolha o tamanho da máquina virtual.
9. Especifique as credenciais de início de sessão para esta máquina virtual. Tome nota dos mesmos, pois precisará-os para aceder à máquina através do ambiente de trabalho remoto.
10. Na **localização** lista, escolha a região para alojar a máquina virtual.
11. Clique nas **OK** botão para começar a criar a máquina virtual. Pode seguir o progresso da operação na **saída** janela.
    
    ![][3]
12. Quando a máquina virtual é aprovisionada, os scripts publicados são criados num **PublishScripts** nó na sua solução. O script publicado é executado e aprovisiona uma máquina virtual no Azure. O **saída** janela mostra o estado. O script realiza as seguintes ações para configurar a máquina virtual:
    
    * Cria a máquina virtual, se ainda não exista.
    * Cria uma conta de armazenamento com um nome que começa com `devtest`, mas apenas se não existir já uma conta de armazenamento na região especificada.
    * Cria um serviço em nuvem como um contêiner para a máquina virtual e cria uma função da web para a aplicação web.
    * Configura o Web Deploy na máquina virtual.
    * Configura o IIS e ASP.NET na máquina virtual.
    
    ![][4]
13. (Opcional) Pode ligar-se para a nova máquina virtual. Na **Explorador de servidores**, expanda o **máquinas virtuais** nó, escolha o nó para a máquina virtual que criou e, no seu menu de atalho, escolha **ligar com o ambiente de trabalho remoto**. Em alternativa, no **Cloud Explorer** pode escolher **abrir no Portal** no menu de atalho e ligar à máquina virtual existe.
    
    ![][5]

## <a name="next-steps"></a>Passos Seguintes
Se pretender personalizar os scripts publicados que criar, ler informações mais aprofundadas em [usando Scripts do Windows PowerShell para publicar no desenvolvimento e ambientes de teste](http://msdn.microsoft.com/library/dn642480.aspx).

[0]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_NewProject.PNG
[1]: ./media/dotnet-visual-studio-create-virtual-machine/CreateVM_SignIn.PNG
[2]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_CreateVM.PNG
[3]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_Provisioning.png
[4]: ./media/virtual-machines-common-classic-web-app-visual-studio/CreateVM_SolutionExplorer.png
[5]: ./media/virtual-machines-common-classic-web-app-visual-studio/VS_Create_VM_Connect.png
