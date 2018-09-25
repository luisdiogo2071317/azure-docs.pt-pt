


Este artigo descreve como:

* Injete dados numa máquina virtual do Azure (VM) quando ele está a ser aprovisionado.
* Recuperá-la para Windows e Linux.
* Utilize ferramentas especiais disponíveis em alguns sistemas para detetar e lidar com dados personalizados automaticamente.

> [!NOTE]
> Este artigo descreve como CustomData pode ser injetado através de uma VM criada com o modelo de implementação clássica. Para ver como utilizar a API de gestão de recursos do Azure, consulte [o modelo de exemplo](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-customdata).
> 
> 

## <a name="injecting-custom-data-into-your-azure-virtual-machine"></a>Injetar dados personalizados à sua máquina virtual do Azure

[!INCLUDE [classic-cli](contains-classic-cli-content.md)]

Esta funcionalidade é atualmente suportada apenas na [CLI do Azure](https://github.com/Azure/azure-xplat-cli). Aqui criamos uma `custom-data.txt` ficheiro que contém nossos dados, em seguida, inserir isso em à VM durante o aprovisionamento. Embora possa utilizar qualquer uma das opções para o `azure vm create` de comando, o exemplo seguinte demonstra uma abordagem básica:

```
    azure vm create <vmname> <vmimage> <username> <password> \  
    --location "West US" --ssh 22 \  
    --custom-data ./custom-data.txt  
```


## <a name="using-custom-data-in-the-virtual-machine"></a>Usando dados personalizados na máquina virtual
* Se a sua VM do Azure é uma VM baseada no Windows, em seguida, o ficheiro de dados personalizado é guardado para `%SYSTEMDRIVE%\AzureData\CustomData.bin`. Embora tenha sido codificado em base64 para transferir a partir do computador local para a nova VM, ela é automaticamente decodificada e pode ser aberta ou utilizada imediatamente.
  
  > [!NOTE]
  > Se o ficheiro existir, será substituído. A segurança no diretório está definida como **controle de sistema: completo** e **administradores: Full Control**.
  > 
  > 
* Se a sua VM do Azure é uma VM baseada em Linux, em seguida, o ficheiro de dados personalizado está localizado em um dos seguintes locais, dependendo de sua distribuição. Os dados podem ser codificada em base64, por isso terá de decodificar os dados pela primeira vez:
  
  * `/var/lib/waagent/ovf-env.xml`
  * `/var/lib/waagent/CustomData`
  * `/var/lib/cloud/instance/user-data.txt` 

## <a name="cloud-init-on-azure"></a>Inicialização da cloud no Azure
Se a sua VM do Azure é a partir de uma imagem de Ubuntu ou CoreOS, em seguida, pode utilizar CustomData para enviar uma configuração de cloud para o cloud-init. Ou, se seu arquivo de dados personalizados é um script, em seguida, na cloud-init pode executá-lo.

### <a name="ubuntu-cloud-images"></a>Imagens de nuvem do Ubuntu
Na maioria das imagens do Linux do Azure, seria a editar "/ etc/waagent.conf" para configurar o disco de recursos temporário e trocar o ficheiro. Ver [Guia do usuário do agente Linux do Azure](../articles/virtual-machines/extensions/agent-linux.md) para obter mais informações.

No entanto, em imagens de Cloud Ubuntu, tem de utilizar cloud-init para configurar o disco de recurso (ou seja, o disco "efémeros") e a partição de troca. Consulte a página seguinte no wiki do Ubuntu para obter mais detalhes: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps-using-cloud-init"></a>Próximos passos: utilizar o cloud-init
Para obter mais informações, consulte a [documentação de cloud-init para Ubuntu](https://help.ubuntu.com/community/CloudInit).

<!--Link references-->
[Adicionar referência de API de REST de gestão de serviço de função](http://msdn.microsoft.com/library/azure/jj157186.aspx)

[Interface de linha de comandos do Azure](https://github.com/Azure/azure-xplat-cli)

