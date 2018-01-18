| **Componente** | **Requisito** |
| --- |---|
| Núcleos de CPU| 8 |
| RAM | 12 GB|
| Número de discos | 3, incluindo o disco de SO, disco de cache do servidor de processo e unidade de retenção para reativação pós-falha |
| Espaço livre no disco (cache do servidor de processos) | 600 GB
| Espaço livre no disco (disco de retenção) | 600 GB|
| Sistema operativo  | Windows Server 2012 R2 <br> Windows Server 2016 |
| Região do sistema operativo | Inglês (en-us)|
| Versão do VMware vSphere PowerCLI | [PowerCLI 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1 "PowerCLI 6.0")|
| Funções do Windows Server | Não ative estas funções: <br> - Active Directory Domain Services <br>- Serviços de Informação da Internet <br> - Hyper-V |
| Políticas de grupo| Não ative estas políticas de grupo: <br> -Impedir o acesso à linha de comandos <br> -Impedir o acesso ao registo ferramentas de edição <br> -Confiar lógica para anexos de ficheiros <br> -Ativar a execução do Script <br> [Saiba mais](https://technet.microsoft.com/en-us/library/gg176671(v=ws.10).aspx)|
| IIS | -Qualquer Web site predefinido do pré-existente <br> -Ativar [autenticação anónima](https://technet.microsoft.com/en-us/library/cc731244(v=ws.10).aspx) <br> -Ativar [FastCGI](https://technet.microsoft.com/en-us/library/cc753077(v=ws.10).aspx) definição  <br> -Não pré-existente Web site/aplicação está a escutar na porta 443<br>|
| Tipo NIC | VMXNET3 (quando implementado como uma VM de VMware) |
| Tipo de endereço IP | Estático |
| Acesso à Internet | O servidor precisa de aceder a estes URLs: <br> - \*.accesscontrol.windows.net<br> - \*.backup.windowsazure.com <br>- \*.store.core.windows.net<br> - \*.blob.core.windows.net<br> - \*.hypervrecoverymanager.windowsazure.com <br> - https://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi (não é necessário para Servidores de Processos de Aumento Horizontal) <br> - time.nist.gov <br> - time.windows.com |
| Portas | 443 (Canal de controlo e orquestração)<br>9443 (Transporte de dados)|
