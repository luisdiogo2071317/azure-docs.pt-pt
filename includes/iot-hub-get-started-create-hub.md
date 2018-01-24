## <a name="create-an-iot-hub"></a>Criar um hub IoT
Crie um hub IoT para ligação da aplicação do dispositivo simulado. Os passos que se seguem mostram como concluir esta tarefa com o Portal do Azure.

1. Inicie sessão no [Portal do Azure][lnk-portal].

1. Selecione **Novo** > **Internet das Coisas** > **Hub IoT**.
   
    ![Barra de atalhos do portal do Azure][1]

1. No painel **Hub IoT**, introduza as seguintes informações para o hub IoT:

   * **Nome**: crie um nome para o seu hub IoT. Se o nome introduzido for válido, aparece uma marca de verificação verde.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

   * **Escalão de preços e dimensionamento**: para este tutorial, selecione o escalão **F1 - Gratuito**. Para obter mais informações, veja [Escalão de preços e dimensionamento][lnk-pricing].

   * **Grupo de recursos**: crie um grupo de recursos para utilizar o hub IoT ou utilize um já existente. Para obter mais informações, veja [Utilizar grupos de recursos para gerir os recursos do Azure][lnk-resource-groups]

   * **Localização**: selecione a localização mais próxima de si.

   * **Afixar ao dashboard**: marque esta opção para facilitar o acesso ao hub IoT a partir do dashboard.

    ![Janela doe hub IoT][2]

1. Clique em **Criar**. A criação do hub IoT pode demorar alguns minutos. Pode monitorizar o progresso no painel **Notificações**.

1. Quando o hub IoT tiver sido criado com êxito, clique no novo mosaico do mesmo no portal do Azure para abrir a janela das propriedades do hub IoT novo. Agora que criou um hub IoT, localize as informações importantes que utilizar para ligar dispositivos e aplicações para o seu IoT hub. Anote o **Nome de anfitrião** e, em seguida, clique em **Políticas de acesso partilhado**.
   
    ![Janela do hub IoT novo][4]

1. Em **Políticas de acesso partilhado**, clique na política **iothubowner** e copie e anote a cadeia de ligação do Hub IoT na janela **iothubowner**. Para obter mais informações, veja [Controlo de acesso][lnk-access-control] no “Guia do programador do Hub IoT.”
   
    ![Políticas de acesso partilhado][5]

<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-resource-groups]: ../articles/azure-resource-manager/resource-group-portal.md
