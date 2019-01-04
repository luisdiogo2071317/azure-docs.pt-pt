---
title: Resolver problemas com o servidor de configuração durante a recuperação após desastre de VMs de VMware e servidores físicos para o Azure com o Azure Site Recovery | Documentos da Microsoft
description: Este artigo fornece informações de resolução de problemas para implementar o servidor de configuração para recuperação após desastre de VMs de VMware e servidores físicos para o Azure com o Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 12/17/2018
ms.author: ramamill
ms.openlocfilehash: 6c8f4fa1fdfdb18d57f001308a6b2105acf9a08d
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/27/2018
ms.locfileid: "53788859"
---
# <a name="troubleshoot-configuration-server-issues"></a>Resolver problemas do servidor de configuração

Este artigo ajuda-o a resolver problemas quando implementar e gerir o [do Azure Site Recovery](site-recovery-overview.md) servidor de configuração. O servidor de configuração atua como um servidor de gestão e é utilizado para definir a recuperação após desastre para VMs de VMware no local e servidores físicos para o Azure com o Site Recovery. Seções a seguir discute as falhas mais comuns vistas ao adicionar um novo servidor de configuração e gerir um servidor de configuração.

## <a name="registration-failures"></a>Falhas de registo

Regista a máquina de origem com o servidor de configuração durante a instalação do agente de mobilidade. Quaisquer falhas durante este passo podem ser depuradas, seguindo as diretrizes indicadas abaixo:

1. Aceda ao ficheiro de C:\ProgramData\ASR\home\svsystems\var\configurator_register_host_static_info.log. ProgramData pode ser uma pasta oculta. Se não conseguir localizar, tente anular ocultar a pasta. As falhas podem ser devido a vários problemas.
2. Procure a cadeia de caracteres "Nenhum endereço IP válido encontrado". Se a cadeia for encontrada,
    - Valide se o id de anfitrião pedido for igual a máquina de origem.
    - A máquina de origem deve ter, pelo menos, um endereço IP atribuído ao NIC físico para o registo do agente com o CS tenha êxito.
    - Execute o comando na máquina de origem `> ipconfig /all` (para o SO do Windows) e `# ifconfig -a` (para o SO Linux) para obter todos os endereços IP da máquina de origem.
    - Tenha em atenção que o registo do agente requer um endereço válido IP v4 atribuído para o NIC físico.
3. Se não for encontrada a cadeia acima, procure a cadeia de caracteres "Motivo" = > "NULL". Se encontrar,
    - Quando o computador de origem utilizar um anfitrião está vazio se registar com o servidor de configuração, este erro ocorrer.
    - Depois de resolver os problemas, siga as diretrizes dado [aqui](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server) para repetir o registo manualmente.
4. Se não for encontrada a cadeia acima, vá para a máquina de origem e verifique o registo C:\ProgramData\ASRSetupLogs\UploadedLogs\* ASRUnifiedAgentInstaller.log ProgramData pode ser uma pasta oculta. Se não conseguir localizar, tente anular ocultar a pasta. As falhas podem ser devido a vários problemas. Procure a cadeia de caracteres "publicar pedido: (7) - não foi possível ligar ao servidor". Se encontrar,
    - Resolva os problemas de rede entre o servidor de configuração e de máquina de origem. Certifique-se de que esse servidor de configuração está acessível a partir do computador de origem com as ferramentas de rede, como o ping, traceroute, navegador da web etc., certifique-se de que essa máquina de origem é capaz de alcançar o servidor de configuração através da porta 443.
    - Verifique se existem quaisquer regras de firewall na máquina de origem estiver a bloquear a ligação entre o servidor de configuração e de máquina de origem. Trabalhar com os seus administradores de rede para desbloquear os problemas de ligação.
    - Certifique-se as pastas mencionadas [aqui](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) não inclui o software antivírus.
    - Depois de resolver os problemas de rede, e repita o registo seguintes diretrizes dadas [aqui](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server).
5. Se não for encontrado, em que a mesma aparência de registo para a cadeia "pedido: (60) - certificado de ponto a ponto não pode ser autenticado com fornecido certificados de AC. ". Se encontrar, 
    - Este erro pode ser porque o certificado de servidor de configuração expirou ou computador de origem não suporta o TLS 1.0 e acima SSL protocolos ou existe uma firewall que está a bloquear a comunicação SSL entre o servidor de configuração e de máquina de origem.
    - Para resolver, ligue ao endereço IP de servidor de configuração usando um navegador da web na máquina de origem com a ajuda de URI https://<CSIPADDRESS>: 443 /. Certifique-se de que essa máquina de origem é capaz de alcançar o servidor de configuração através da porta 443.
    - Verifique se existem quaisquer regras de firewall no computador de origem a ser adicionadas/removidas da máquina de origem comunicar com o CS. Uma vez que poderia haver muitos software de firewall diferente, não é possível listar as configurações necessárias, trabalhe em conjunto com os administradores de rede do cliente.
    - Certifique-se as pastas mencionadas [aqui](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) não inclui o software antivírus.  
    - Depois de resolver os problemas, e repita o registo seguintes diretrizes dadas [aqui](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server).
6. No Linux, se o valor de plataforma a partir de < INSTALLATION_DIR > /etc/drscout.conf está danificado, em seguida, registo falhará. Para identificar, vá para o registo /var/log/ua_install.log. Encontrará a cadeia de caracteres "Abortar a configuração como valor VM_PLATFORM é nulo ou não é VmWare/Azure". A plataforma deve ser definida como "VmWare" ou "Azure". Como o drscout Conf está danificado, é recomendado [desinstalar](vmware-physical-mobility-service-overview.md#uninstall-the-mobility-service) o agente de mobilidade e, em seguida, instalar novamente. Se a desinstalação falhar, siga os passos abaixo:
    - Abra o ficheiro Installation_Directory/uninstall.sh e comente a chamada para a função *StopServices*
    - Abra o ficheiro Installation_Directory/Vx/bin/desinstalação e a chamada para a função `stop_services`
    - Abra o ficheiro Installation_Directory/Fx/desinstalação e comente a seção completa que está a tentar parar o serviço de Fx.
    - Agora tente [desinstalar](vmware-physical-mobility-service-overview.md#uninstall-the-mobility-service) o agente de mobilidade. Depois de concluída com êxito a desinstalação, reinicialize o sistema e tente instalar novamente o agente.

## <a name="installation-failure---failed-to-load-accounts"></a>Falha de instalação – falha ao carregar contas

Este erro ocorre quando o serviço não consegue ler dados a partir da ligação de transporte ao instalar o agente de mobilidade e registar com o servidor de configuração. Para resolver, certifique-se de que TLS 1.0 é ativado no seu computador de origem.

## <a name="change-ip-address-of-configuration-server"></a>Alterar o endereço IP do servidor de configuração

Recomenda-se vivamente que não altere o endereço IP de um servidor de configuração. Certifique-se de que todos os IPs atribuídos ao servidor de configuração são IPs estáticos e não os IPs de DHCP.

## <a name="acs50008-saml-token-is-invalid"></a>ACS50008: SAML token é inválido

Para evitar este erro, certifique-se de que a hora no relógio do sistema não é mais de 15 minutos da hora local. Volte a executar o instalador para concluir o registo.

## <a name="failed-to-create-certificate"></a>Falha ao criar certificado

Não é possível criar um certificado necessário para autenticar o Site Recovery. Execute novamente a configuração depois de assegurar que está a executar o programa de configuração como administrador local.

## <a name="register-source-machine-with-configuration-server"></a>Registar a máquina de origem com o servidor de configuração

### <a name="if-source-machine-has-windows-os"></a>Se a máquina de origem tiver o SO Windows

Execute o seguinte comando na máquina de origem

```
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
  ```
**Definição** | **Detalhes**
--- | ---
Utilização | UnifiedAgentConfigurator.exe /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
Registos de configuração do agente | Em % ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log.
/CSEndPoint | Parâmetro obrigatório. Especifica o endereço IP do servidor de configuração. Utilize qualquer endereço IP válido.
/PassphraseFilePath |  Obrigatório. Localização da frase de acesso. Utilize qualquer válido UNC ou caminho de ficheiro local.

### <a name="if-source-machine-has-linux-os"></a>Se a máquina de origem tiver o SO Linux

Execute o seguinte comando na máquina de origem

```
  /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
  ```
**Definição** | **Detalhes**
--- | ---
Utilização | CD /usr/local/ASR/Vx/bin<br/><br/> UnifiedAgentConfigurator.sh -i <CSIP> - P <PassphraseFilePath>
-i | Parâmetro obrigatório. Especifica o endereço IP do servidor de configuração. Utilize qualquer endereço IP válido.
-P |  Obrigatório. Caminho completo do ficheiro em que a frase de acesso é guardado. Utilize qualquer pasta válido