
* A conversão necessita de um reinício da VM, por isso, agende a migração das VMs durante uma janela de manutenção pré-existente. 

* A conversão não é reversível. 

* Certifique-se de que testa a conversão. Migre uma máquina virtual de teste antes de efetuar a migração na produção.

* Durante a conversão, deve desalocar a VM. A VM recebe um novo endereço IP quando é iniciada após a conversão. Se necessário, pode [atribuir um endereço IP estático](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md) à VM.

* Os VHDs originais e a conta de armazenamento utilizada pela VM antes da conversão não são eliminados. Continuam a incorrer em custos. Para evitar a cobrança destes artefactos, elimine os blobs de VHD originais depois de verificar que a conversão está concluída.

* Reveja a versão mínima do agente de VM do Azure necessário para suportar o processo de conversão. Para obter informações sobre como Verifique e atualize a sua versão do agente, consulte [suporte para a versão mínima para agentes VM no Azure](https://support.microsoft.com/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support)