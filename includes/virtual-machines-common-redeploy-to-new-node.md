## <a name="use-the-azure-portal"></a>Utilizar o portal do Azure
1. Selecione a VM que pretende voltar a implementar, em seguida, selecione o *Reimplementar* botão no *definições* painel. Poderá ter de deslocar para baixo para ver os **suporte e resolução de problemas** secção que contém o botão "Reimplementar" como no exemplo seguinte:
   
    ![Painel VM do Azure](./media/virtual-machines-common-redeploy-to-new-node/vmoverview.png)
2. Para confirmar a operação, selecione o *Reimplementar* botão:
   
    ![Voltar a implementar um painel VM](./media/virtual-machines-common-redeploy-to-new-node/redeployvm.png)
3. O **Status** da VM é alterado para *atualização* como a VM se prepara para implementar novamente, conforme mostrado no exemplo a seguir:
   
    ![A atualizar a VM](./media/virtual-machines-common-redeploy-to-new-node/vmupdating.png)
4. O **Status** é alterado, em seguida, para *inicial* como a VM arranca num anfitrião do Azure novo, conforme mostrado no exemplo a seguir:
   
    ![VM a iniciar](./media/virtual-machines-common-redeploy-to-new-node/vmstarting.png)
5. Após a conclusão da VM o processo de inicialização, o **Status** , em seguida, devolve a *em execução*, que indica que a VM tiver sido implementada novamente com êxito:
   
    ![VM em execução](./media/virtual-machines-common-redeploy-to-new-node/vmrunning.png)

