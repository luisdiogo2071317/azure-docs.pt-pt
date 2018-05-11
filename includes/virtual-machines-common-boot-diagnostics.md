Suporte para duas funcionalidades de depuração está agora disponível no Azure: resultado da consola e captura de ecrã suporte para o modelo de implementação do Gestor de recursos de máquinas virtuais do Azure. 

Quando colocar a sua própria imagem para o Azure ou mesmo arrancar uma das imagens da plataforma, pode ser muitos motivos por que motivo uma máquina virtual obtém num Estado não arranque. Estas funcionalidades permitem-lhe facilmente diagnosticar e recuperar máquinas virtuais de falhas de arranque.

Para máquinas virtuais do Linux, pode visualizar facilmente a saída do seu registo de consola do Portal:

![Portal do Azure](./media/virtual-machines-common-boot-diagnostics/screenshot1.png)
 
No entanto, para máquinas virtuais Windows e Linux, Azure também permite-lhe ver uma captura de ecrã da VM a partir do hipervisor:

![Erro](./media/virtual-machines-common-boot-diagnostics/screenshot2.png)

Ambas estas funcionalidades são suportadas máquinas virtuais do Azure em todas as regiões. Tenha em atenção que as capturas de ecrã e a saída podem demorar até 10 minutos a aparecer na sua conta de armazenamento.

## <a name="common-boot-errors"></a>Erros de arranque comuns

- [0xC000000E](https://support.microsoft.com/help/4010129)
- [0xC000000F](https://support.microsoft.com/help/4010130)
- [0xC0000011](https://support.microsoft.com/help/4010134)
- [0xC0000034](https://support.microsoft.com/help/4010140)
- [0xC0000098](https://support.microsoft.com/help/4010137)
- [0xC00000BA](https://support.microsoft.com/help/4010136)
- [0xC000014C](https://support.microsoft.com/help/4010141)
- [0xC0000221](https://support.microsoft.com/help/4010132)
- [0xC0000225](https://support.microsoft.com/help/4010138)
- [0xC0000359](https://support.microsoft.com/help/4010135)
- [0xC0000605](https://support.microsoft.com/help/4010131)
- [Não foi encontrado nenhum sistema operativo](https://support.microsoft.com/help/4010142)
- [Falha de arranque ou INACCESSIBLE_BOOT_DEVICE](https://support.microsoft.com/help/4010143)

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a>Ativar o diagnóstico numa máquina virtual nova
1. Ao criar uma nova máquina virtual do Portal do Azure, selecione o **do Azure Resource Manager** na lista pendente de modelo de implementação:
 
    ![Resource Manager](./media/virtual-machines-common-boot-diagnostics/screenshot3.jpg)

2. No **definições**, ativar o **diagnóstico de arranque**e, em seguida, selecione uma conta de armazenamento que pretende colocar estes ficheiros de diagnóstico.
 
    ![Criar VM](./media/virtual-machines-common-boot-diagnostics/create-storage-account.png)

    > [!NOTE]
    > A funcionalidade de diagnóstico de arranque não suporta a conta de armazenamento premium. Se utilizar a conta de armazenamento premium para diagnóstico de arranque, poderá receber o erro StorageAccountTypeNotSupported quando iniciar a VM.
    >
    > 

3. Se estiver a implementar a partir de um modelo Azure Resource Manager, navegue para o recurso de máquina virtual e acrescentar a secção de perfil de diagnóstico. Não se esqueça de utilizar o cabeçalho da versão da API "2015-06-15".

    ```json
    {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          … 
    ```

4. O perfil de diagnóstico permite-lhe selecionar a conta de armazenamento onde quer colocar estes registos.

    ```json
            "diagnosticsProfile": {
                "bootDiagnostics": {
                "enabled": true,
                "storageUri": "[concat('http://', parameters('newStorageAccountName'), '.blob.core.windows.net')]"
                }
            }
            }
        }
    ```

Para implementar uma máquina virtual de exemplo com o diagnóstico de arranque ativado, consulte nosso repositório aqui.

## <a name="enable-boot-diagnostics-on-existing-virtual-machine"></a>Ativar o diagnóstico de arranque na máquina virtual existente 

Para ativar o diagnóstico de arranque numa máquina virtual existente, siga estes passos:

1. Inicie sessão no [portal do Azure](https://portal.azure.com)e, em seguida, selecione a máquina virtual.
2. No **suporte + resolução de problemas**, selecione **diagnóstico de arranque** > **definições**, alterar o estado para **no**e, em seguida, Selecione uma conta de armazenamento. 
4. Certifique-se de que a opção de diagnóstico de arranque está selecionada e, em seguida, guarde a alteração.

    ![Atualizar VM Existente](./media/virtual-machines-common-boot-diagnostics/enable-for-existing-vm.png)

3. Reinicie a VM para entrar em vigor.


