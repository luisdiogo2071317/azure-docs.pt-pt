<!--author=SharS last changed: 1/7/2016-->

#### <a name="to-add-a-volume-container"></a>Para adicionar um contentor de volume
1. Sobre o **dispositivos** página, selecione o dispositivo, clique duas vezes nele e, em seguida, clique nas **contentores de volumes** separador.
2. Clique em **adicionar** na parte inferior da página. Na **criar contentor de volumes** diálogo caixa, faça o seguinte:
   
   1. Fornecer um exclusivo **nome** para o contentor de volume. Este nome pode conter um máximo de 32 carateres.
   2. Selecione um **conta de armazenamento** a ser associado este contentor de volume. Pode escolher entre uma conta de armazenamento existente dentro da mesma subscrição ou selecione **adicionar mais** para selecionar uma conta de armazenamento a partir de outra subscrição. Também pode escolher a conta de armazenamento que foi gerada pela primeira vez em que o serviço foi criado.
   3. Especificar a largura de banda conforme **ilimitado** se quiser consumir largura de banda disponível, ou **personalizado** para utilizar controlos de largura de banda. Para uma largura de banda personalizada, fornece um valor entre 1 e 1000 Mbps. Para alocar a largura de banda com base numa agenda, pode **selecionar um modelo de largura de banda**.
   4. Recomendamos que mantenha **ativar a encriptação de armazenamento na Cloud** selecionado para encriptar os dados que vão para a cloud. Desative a encriptação apenas se estão empregando outros meios para encriptar os dados. Não é possível modificar a definição de encriptação quando o contentor de volume tiver sido criado.
   5. Fornecer um **chave de encriptação de armazenamento na nuvem** que contenha entre 8 e 32 carateres. O dispositivo utiliza esta chave para acessar os dados criptografados. Na **Confirmar chave de encriptação de armazenamento na nuvem** , insira a chave de encriptação de armazenamento na cloud novamente para confirmar que este. 
   6. Clique na seta para avançar para a página seguinte.
      
      ![Criar contentor de volumes com o modelo de largura de banda 1](./media/storsimple-add-volume-container/HCS_CreateVCBT1-include.png) 
3. Se tiver especificado **selecionar um modelo de largura de banda**, selecione na lista pendente de modelos de largura de banda existentes. Reveja as definições de agendamento e clique no ícone de verificação ![ícone de verificação](./media/storsimple-configure-new-storage-account/HCS_CheckIcon-include.png).
   
    ![Criar contentor de volumes com o modelo de largura de banda 2](./media/storsimple-add-volume-container/HCS_CreateVCBT2-include.png) 

O contentor de volumes será guardado e o contentor de volumes recentemente criado será incluído nos **contentor de volumes** página.

