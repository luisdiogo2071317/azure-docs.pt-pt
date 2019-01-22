---
title: Resolver problemas com o servidor de configuração durante a recuperação após desastre de VMs de VMware e servidores físicos para o Azure com o Azure Site Recovery | Documentos da Microsoft
description: Este artigo fornece informações de resolução de problemas para implementar o servidor de configuração para recuperação após desastre de VMs de VMware e servidores físicos para o Azure ao utilizar o Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 01/14/2019
ms.author: ramamill
ms.openlocfilehash: 0eebfd8b75f428d3b8f6024ed6ee71c18c1309f6
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2019
ms.locfileid: "54435979"
---
# <a name="troubleshoot-configuration-server-issues"></a>Resolver problemas do servidor de configuração

Este artigo ajuda-o a resolver problemas ao implementar e gerir o [do Azure Site Recovery](site-recovery-overview.md) servidor de configuração. O servidor de configuração atua como um servidor de gestão. Utilize o servidor de configuração para configurar a recuperação após desastre de VMs de VMware no local e servidores físicos no Azure utilizando a recuperação de Site. As secções seguintes abordam as falhas mais comuns que podem ocorrer quando adiciona um novo servidor de configuração e na gestão de um servidor de configuração.

## <a name="registration-failures"></a>Falhas de registo

A máquina de origem regista com o servidor de configuração quando instalar o agente de mobilidade. Pode depurar quaisquer falhas durante este passo ao seguir estas diretrizes:

1. Abra o ficheiro de C:\ProgramData\ASR\home\svsystems\var\configurator_register_host_static_info.log. (A pasta ProgramData pode ser uma pasta oculta. Se não vir a pasta ProgramData, no Explorador de ficheiros, no **View** separador a **Mostrar/ocultar** secção, selecione o **oculto itens** caixa de verificação.) Falhas podem ser causadas por vários problemas.

2. Procure a cadeia de caracteres **endereço IP válido não foram encontrados**. Se for encontrada a cadeia de caracteres:
    1. Certifique-se de que o ID de anfitrião pedido é o mesmo que o ID do anfitrião da máquina de origem.
    2. Certifique-se de que a máquina de origem tem, pelo menos, um endereço IP atribuído para o NIC físico. Para o registo do agente com o servidor de configuração com êxito, a máquina de origem tem de ter pelo menos um endereço de v4 IP válido atribuído para o NIC físico.
    3. Execute um dos seguintes comandos no computador de origem para obter todos os endereços IP da máquina de origem:
      - Para Windows: `> ipconfig /all`
      - Para Linux: `# ifconfig -a`

3. Se a cadeia de caracteres **endereço IP válido não encontrado** não for encontrada, procure a cadeia **motivo = > nulo**. Este erro ocorre se o computador de origem utilizar um anfitrião está vazio para registar com o servidor de configuração. Se for encontrada a cadeia de caracteres:
    - Depois de resolver os problemas, siga as diretrizes [registar a máquina de origem com o servidor de configuração](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server) para repetir o registo manualmente.

4. Se a cadeia de caracteres **motivo = > nulo** não for encontrada, na máquina de origem, abra o ficheiro de C:\ProgramData\ASRSetupLogs\UploadedLogs\ASRUnifiedAgentInstaller.log. (A pasta ProgramData pode ser uma pasta oculta. Se não vir a pasta ProgramData, no Explorador de ficheiros, no **View** separador a **Mostrar/ocultar** secção, selecione o **oculto itens** caixa de verificação.) Falhas podem ser causadas por vários problemas. 

5. Procure a cadeia **publicar pedido: (7) – não foi possível ligar ao servidor**. Se for encontrada a cadeia de caracteres:
    1. Resolva os problemas de rede entre o computador de origem e o servidor de configuração. Certifique-se de que o servidor de configuração está acessível a partir do computador de origem, utilizando ferramentas de rede, como o ping, traceroute ou um navegador da web. Certifique-se de que a máquina de origem pode contactar o servidor de configuração através da porta 443.
    2. Verifique se qualquer de regras de firewall no bloco de máquina de origem a ligação entre a máquina de origem e o servidor de configuração. Trabalhar com os administradores de rede para desbloquear os problemas de ligação.
    3. Certifique-se de que as pastas listadas na [exclusões de pastas de recuperação de sites de programas de antivírus](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) não inclui o software antivírus.
    4. Quando resolver problemas de rede, repita o registo ao seguir as diretrizes [registar a máquina de origem com o servidor de configuração](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server).

6. Se a cadeia de caracteres **publicar pedido: (7) – não foi possível ligar ao servidor** não for encontrada, no mesmo ficheiro de registo, procure a cadeia **pedido: (60) - certificado de ponto a ponto não pode ser autenticado com fornecido certificados de AC**. Este erro pode ocorrer porque o certificado de servidor de configuração expirou ou a máquina de origem não suporta o TLS 1.0 ou posteriores protocolos SSL. Também poderá ocorrer se um firewall bloquear a comunicação SSL entre o computador de origem e o servidor de configuração. Se for encontrada a cadeia de caracteres: 
    1. Para resolver, ligue ao servidor de configuração endereço IP com um navegador da web na máquina de origem. Utilizar o URI https:\/\/< endereço IP do servidor de configuração\>: 443 /. Certifique-se de que a máquina de origem pode contactar o servidor de configuração através da porta 443.
    2. Verifique se quaisquer regras de firewall no computador de origem tem de ser adicionado ou removido máquina de origem comunicar com o servidor de configuração. Devido à variedade de software de firewall que poderá estar em utilização, nós não é possível listar todas as configurações de firewall necessárias. Trabalhar com os administradores de rede para desbloquear os problemas de ligação.
    3. Certifique-se de que as pastas listadas na [exclusões de pastas de recuperação de sites de programas de antivírus](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program) não inclui o software antivírus.  
    4. Depois de resolver os problemas, e repita o registo seguintes diretrizes [registar a máquina de origem com o servidor de configuração](vmware-azure-troubleshoot-configuration-server.md#register-source-machine-with-configuration-server).

7. No Linux, se o valor da plataforma no < INSTALLATION_DIR\>/etc/drscout.conf está danificado, registo falha. Para identificar este problema, abra o ficheiro de /var/log/ua_install.log. Procure a cadeia de caracteres **abortar a configuração como valor VM_PLATFORM é nulo ou não é o VmWare/Azure**. A plataforma deve ser definida para o **VmWare** ou **Azure**. Se o ficheiro drscout está danificado, recomendamos que [desinstalar o agente de mobilidade](vmware-physical-mobility-service-overview.md#uninstall-the-mobility-service) e, em seguida, reinstale o agente de mobilidade. Se a desinstalação falhar, conclua os seguintes passos:
    1. Abra o ficheiro de Installation_Directory/uninstall.sh e comente a chamada para o **StopServices** função.
    2. Abra o ficheiro de Installation_Directory/Vx/bin/uninstall.sh e comente a chamada para o **stop_services** função.
    3. Abra o ficheiro de Installation_Directory/Fx/uninstall.sh e comente a secção de inteira que está a tentar parar o serviço de Fx.
    4. [Desinstalar](vmware-physical-mobility-service-overview.md#uninstall-the-mobility-service) o agente de mobilidade. Após a desinstalação com êxito, reinicialize o sistema e, em seguida, tente reinstalar o agente de mobilidade.

## <a name="installation-failure-failed-to-load-accounts"></a>Falha na instalação: Falha ao carregar contas

Este erro ocorre quando o serviço não é possível ler dados a partir da ligação de transporte, quando está a instalar o agente de mobilidade e registar com o servidor de configuração. Para resolver o problema, certifique-se de que o TLS 1.0 está ativado no seu computador de origem.

## <a name="vcenter-discovery-failures"></a>falhas de deteção do vCenter

Para resolver falhas de deteção do vCenter, certifique-se de que o servidor vCenter é adicionado as ignorar lista as definições de proxy. Para executar esta atividade

- Transferir a ferramenta PsExec da [aqui](https://aka.ms/PsExec) para aceder a conteúdo de utilizador do sistema.
- Abra o Internet Explorer no conteúdo de utilizador do sistema, executando a seguinte linha de comandos psexec -s -i "%programfiles%\Internet Explorer\iexplore.exe"
- Adicionar definições de proxy no IE e reinicie o serviço de tmanssvc.
- Para configurar as definições de proxy do DRA, execute cd C:\Program Files\Microsoft Azure Site Recovery Provider
- Em seguida, execute DRCONFIGURATOR. EXE / configurar /AddBypassUrls [adicionar o endereço de IP/FQDN do vCenter Server fornecido durante **configurar servidor vCenter Server/vSphere ESXi** passo [implementação de servidor de configuração](vmware-azure-deploy-configuration-server.md#configure-settings)]

## <a name="change-the-ip-address-of-the-configuration-server"></a>Alterar o endereço IP do servidor de configuração

Recomendamos vivamente que não altere o endereço IP de um servidor de configuração. Certifique-se de que todos os endereços IP que estão atribuídos ao servidor de configuração são endereços IP estáticos. Não utilize endereços IP de DHCP.

## <a name="acs50008-saml-token-is-invalid"></a>ACS50008: SAML token é inválido

Para evitar este erro, certifique-se de que a hora no relógio do sistema não é diferente de na hora local em mais de 15 minutos. Volte a executar o instalador para concluir o registo.

## <a name="failed-to-create-a-certificate"></a>Falha ao criar um certificado

Não é possível criar um certificado necessário para autenticar o Site Recovery. Execute novamente a configuração depois de assegurar que está a executar o programa de configuração como administrador local.

## <a name="register-source-machine-with-configuration-server"></a>Registar a máquina de origem com o servidor de configuração

### <a name="if-the-source-machine-runs-windows"></a>Se a máquina de origem executar o Windows

Execute o seguinte comando na máquina de origem:

```
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe  /CSEndPoint <configuration server IP address> /PassphraseFilePath <passphrase file path>
  ```

Definição | Detalhes
--- | ---
Utilização | UnifiedAgentConfigurator.exe /CSEndPoint < endereço IP do servidor de configuração\> /PassphraseFilePath < caminho de ficheiro da frase de acesso\>
Registos de configuração do agente | Localizado em % ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log.
/CSEndPoint | Parâmetro obrigatório. Especifica o endereço IP do servidor de configuração. Utilize qualquer endereço IP válido.
/PassphraseFilePath |  Obrigatório. A localização da frase de acesso. Utilize qualquer válido UNC ou caminho de ficheiro local.

### <a name="if-the-source-machine-runs-linux"></a>Se a máquina de origem executar o Linux

Execute o seguinte comando na máquina de origem:

```
  /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <configuration server IP address> -P /var/passphrase.txt
  ```

Definição | Detalhes
--- | ---
Utilização | CD /usr/local/ASR/Vx/bin<br /><br /> UnifiedAgentConfigurator.sh -i < endereço IP do servidor de configuração\> - P < caminho de ficheiro da frase de acesso\>
-i | Parâmetro obrigatório. Especifica o endereço IP do servidor de configuração. Utilize qualquer endereço IP válido.
-P |  Obrigatório. O caminho completo do ficheiro em que a frase de acesso é guardado. Utilize qualquer pasta válido.

