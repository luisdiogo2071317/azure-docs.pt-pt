---
title: Implementar a pré-visualização de proteção de palavra-passe do Azure AD
description: Implementar a pré-visualização de proteção de palavra-passe do Azure AD proibir o uso de palavras-passe incorretas no local
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 10/30/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.openlocfilehash: efa684d75cd30dcbfc971d0ef0a3717cc15bd0e4
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/26/2019
ms.locfileid: "55081284"
---
# <a name="preview-deploy-azure-ad-password-protection"></a>Pré-visualização: Implementar proteção de palavras-passe do Azure AD

|     |
| --- |
| Proteção de palavra-passe do Azure AD é uma funcionalidade de pré-visualização pública do Azure Active Directory. Para obter mais informações sobre pré-visualizações, consulte [termos de utilização suplementares para pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Agora que temos uma compreensão dos [como impor proteção de palavra-passe do Azure AD para o Windows Server Active Directory](concept-password-ban-bad-on-premises.md), a próxima etapa é planejar e executar a implantação.

## <a name="deployment-strategy"></a>Estratégia de implantação

A Microsoft sugere que qualquer implementação iniciar no modo de auditoria. Modo de auditoria é a configuração inicial do padrão onde as palavras-passe podem continuar a ser definido e qualquer um que seria bloqueado criar entradas no registo de eventos. Assim que o servidor ou servidores proxy e agentes do DC estão completamente implantadas no modo de auditoria, deve ser feita a monitorização regular para determinar a política de palavra-passe que impacto imposição teria em utilizadores e o ambiente se a política foi aplicada.

Durante a fase de auditoria, muitas organizações consideram:

* Eles precisam para melhorar os processos operacionais existentes para utilizar palavras-passe mais seguras.
* Os usuários estão acostumados a escolher regularmente as palavras-passe não seguros
* Terá de informar os utilizadores sobre a alteração futura à imposição de segurança, o impacto pode ter nos mesmos e ajudá-los melhor compreendam como podem escolher senhas mais seguras.

Assim que a funcionalidade está em execução no modo de auditoria de tempo razoável, a configuração de imposição pode ser invertida partir **auditar** para **impor** requerendo portanto que as palavras-passe mais seguras. O monitoramento concentrado durante este período é uma boa idéia.

## <a name="deployment-requirements"></a>Requisitos de implementação

* Todos os controladores de domínio onde será instalado o serviço de agente de proteção DC de palavra-passe do Azure AD têm de ser o Windows Server 2012 ou posterior.

   > [!NOTE]
   > Sistemas de operativos baseados no núcleo de servidor não são atualmente suportados. Este suporte está previsto para GA.

* Todas as máquinas onde será instalada a proteção de palavra-passe do Azure AD service de Proxy têm de executar Windows Server 2012 R2 ou posterior.

   > [!NOTE]
   > Sistemas de operativos baseados no núcleo de servidor não são atualmente suportados. Este suporte está previsto para GA.

* Todas as máquinas em que componentes de proteção de palavra-passe do Azure AD estão instaladas, incluindo controladores de domínio tem de ter o tempo de execução do Universal C instalado.
Preferencialmente, isto é conseguido ao corrigir totalmente a máquina através do Windows Update. Caso contrário, pode ser um pacote de atualizações específicas do sistema operacional adequado instalado - veja [atualizar para o tempo de execução do C Universal no Windows](https://support.microsoft.com/help/2999226/update-for-universal-c-runtime-in-windows)
* Conectividade de rede tem de existir pelo menos um controlador de domínio em cada domínio e pelo menos um servidor que aloja o serviço de proxy de proteção de palavra-passe do Azure AD. Essa conectividade tem de permitir o controlador de domínio aceder a porta de mapeador de ponto de extremidade RPC (135) e a porta do servidor RPC no serviço de proxy.  A porta do servidor RPC é, por predefinição, uma porta dinâmica da RPC, mas pode ser configurada (consulte abaixo) para utilizar uma porta estática.
* Todas as máquinas que aloja o serviço de proxy de proteção de palavra-passe do Azure AD tem de ter acesso à rede para os seguintes pontos finais:

    |Ponto Final |Objetivo|
    | --- | --- |
    |`https://login.microsoftonline.com`|Pedidos de autenticação|
    |`https://enterpriseregistration.windows.net`|Funcionalidade de proteção de palavra-passe do Azure AD|

* Uma conta de administrador global para registar o serviço de proxy de proteção de palavra-passe do Azure AD e a floresta com o Azure AD.
* Uma conta com privilégios de administrador de domínio do Active Directory no domínio de raiz de floresta para registar a floresta do Active Directory do Windows Server com o Azure AD.
* Qualquer domínio do Active Directory, o controlador de domínio a executar software do serviço de agente tem de utilizar DFSR para a replicação de sysvol.

## <a name="single-forest-deployment"></a>Implementação de floresta única

A pré-visualização da proteção de palavra-passe do Azure AD é implementada com o serviço de proxy no máximo, dois servidores e o serviço de agente do controlador de domínio pode ser implementado incrementalmente a todos os controladores de domínio na floresta do Active Directory.

![Como os componentes de proteção de palavra-passe do Azure AD funcionam em conjunto](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

### <a name="download-the-software"></a>Transferir o software

Existem dois programas de instalação necessários para proteção de palavra-passe do Azure AD que pode ser transferido a partir do [Centro de transferências da Microsoft](https://www.microsoft.com/download/details.aspx?id=57071)

### <a name="install-and-configure-the-azure-ad-password-protection-proxy-service"></a>Instalar e configurar o serviço de proxy de proteção de palavra-passe do Azure AD

1. Escolha um ou mais servidores para hospedar o serviço de proxy de proteção de palavra-passe do Azure AD.
   * Cada serviço como esse pode fornecer apenas as políticas de palavra-passe para uma única floresta e a máquina de anfitrião tem de estar associado a um domínio a um domínio (raiz e subordinadas são ambos igualmente suportadas) nessa floresta. Para o serviço proxy de proteção de palavra-passe do Azure AD atender a sua missão, deve existir conectividade de rede entre a pelo menos um controlador de domínio em cada domínio da floresta e a máquina de host de Proxy de proteção de palavra-passe do Azure AD.
   * Ele é suportado para instalar e executar o serviço de proxy de proteção de palavra-passe do Azure AD num controlador de domínio para fins, mas o controlador de domínio de teste, em seguida, precisa de conectividade de internet.

   > [!NOTE]
   > A pré-visualização pública suporta um máximo de dois (2) servidores de proxy por floresta.

2. Instale o software do serviço de Proxy de política de palavra-passe através do pacote AzureADPasswordProtectionProxy.msi MSI.
   * A instalação de software não requer um reinício. A instalação de software pode ser automatizada com procedimentos MSI padrão, por exemplo: `msiexec.exe /i AzureADPasswordProtectionProxy.msi /quiet /qn`

      > [!NOTE]
      > O serviço de Firewall do Windows tem de ser executado antes de instalar o pacote AzureADPasswordProtectionProxy.msi MSI, caso contrário, ocorrerá um erro de instalação. Se a Firewall do Windows está configurada para não executar, a solução alternativa é temporariamente ativar e iniciar o serviço de Firewall do Windows durante o processo de instalação. O software de proxy não tem nenhuma dependência específica sobre o software de Firewall do Windows após a instalação. Se estiver a utilizar uma firewall de terceiros, ele ainda deve ser configurado para satisfazer os requisitos de implementação (permitir acesso de entrada para a porta 135 e o proxy RPC porta do servidor se dinâmico ou estático). [Consulte os requisitos de implementação](howto-password-ban-bad-on-premises-deploy.md#deployment-requirements)

3. Abra uma janela do PowerShell como administrador.
   * A proteção de palavra-passe do Azure AD software de Proxy inclui um novo módulo do PowerShell com o nome AzureADPasswordProtection. Os seguintes passos baseiam-se sobre como executar vários cmdlets deste módulo do PowerShell e partem do princípio de que abriu uma nova janela do PowerShell e importou o novo módulo da seguinte forma:
      * `Import-Module AzureADPasswordProtection`

      > [!NOTE]
      > O software de instalação modifica a variável de ambiente de PSModulePath da máquina host. Por ordem para que esta alteração tenha efeito, para que o módulo do powershell AzureADPasswordProtection pode ser importado e utilizado, terá de abrir uma nova janela de consola do PowerShell.

   * Verifique se o serviço está em execução com o seguinte comando do PowerShell: `Get-Service AzureADPasswordProtectionProxy | fl`.
      * O resultado deve produzir um resultado com o **estado** devolver um resultado de "Em execução".

4. Registre-se o proxy.
   * Depois de concluído o passo 3 o serviço de proxy de proteção de palavra-passe do Azure AD está em execução na máquina, mas ainda não tem as credenciais necessárias para comunicar com o Azure AD. É necessário Registro com o Azure AD para ativar essa capacidade a utilizar o `Register-AzureADPasswordProtectionProxy` cmdlet do PowerShell. O cmdlet requer o administrador global de credenciais para o seu inquilino do Azure, bem como no local privilégios de administrador de domínio do Active Directory no domínio raiz de floresta. Uma vez que foi bem sucedida para um determinado proxy de serviço, invocações adicionais de `Register-AzureADPasswordProtectionProxy` continuar a ter êxito, mas são desnecessários.
      * O cmdlet pode ser executado da seguinte forma:

         ```
         $tenantAdminCreds = Get-Credential
         Register-AzureADPasswordProtectionProxy -AzureCredential $tenantAdminCreds
         ```

         O exemplo só funciona se o utilizador atualmente com sessão iniciada também for um administrador de domínio do Active Directory para o domínio de raiz. Uma alternativa é fornecer as credenciais de domínio necessários através de `-ForestCredential` parâmetro.

   > [!NOTE]
   > Esta operação poderá falhar se a configuração de segurança avançada do Internet Explorer estiver ativada. A solução é desativar IESC, registe-se o proxy, em seguida, IESC de voltar a ativar.

   > [!NOTE]
   > Registo do serviço de proxy de proteção de palavra-passe do Azure AD é esperado uma única etapa no tempo de vida do serviço. O serviço de proxy efetuará automaticamente quaisquer outra maintainenance necessário a partir deste ponto em diante. Assim que ele foi concluída com êxito para uma determinada floresta, invocações adicionais de "Register-AzureADPasswordProtectionProxy" continuam a ter êxito, mas são desnecessárias.

   > [!TIP]
   > Pode haver um atraso considerável (vários segundos) na primeira vez que este cmdlet é executado para um determinado inquilino do Azure antes do cmdlet conclui a execução. A menos que uma falha é comunicada este atraso não deve ser considerado alarmante.

5. Registre-se a floresta.
   * Floresta do Active Directory no local tem de ser inicializada com as credenciais necessárias para comunicar com o Azure com o `Register-AzureADPasswordProtectionForest` cmdlet do Powershell. O cmdlet requer o administrador global de credenciais para o seu inquilino do Azure, bem como no local privilégios de administrador de domínio do Active Directory no domínio raiz de floresta. Este passo é executado uma vez por floresta.
      * O cmdlet pode ser executado da seguinte forma:

         ```
         $tenantAdminCreds = Get-Credential
         Register-AzureADPasswordProtectionForest -AzureCredential $tenantAdminCreds
         ```

         O exemplo só funciona se o utilizador atualmente com sessão iniciada também for um administrador de domínio do Active Directory para o domínio de raiz. Uma alternativa é fornecer as credenciais de domínio necessários por meio do parâmetro - ForestCredential.

         > [!NOTE]
         > Esta operação poderá falhar se a configuração de segurança avançada do Internet Explorer estiver ativada. A solução é desativar IESC, registe-se o proxy, em seguida, IESC de voltar a ativar.

         > [!NOTE]
         > Se vários servidores de proxy estão instalados no seu ambiente, não importa qual o servidor proxy está especificado no procedimento acima.

         > [!TIP]
         > Pode haver um atraso considerável (vários segundos) na primeira vez que este cmdlet é executado para um determinado inquilino do Azure antes do cmdlet conclui a execução. A menos que uma falha é comunicada este atraso não deve ser considerado alarmante.

   > [!NOTE]
   > Registo de floresta do Active Directory deve ser um único passo durante a vida útil da floresta. Os agentes do controlador de domínio em execução na floresta efetuará automaticamente quaisquer outra maintainenance necessário a partir deste ponto em diante. Uma vez que foi concluída com êxito para uma determinada floresta, invocações adicionais de `Register-AzureADPasswordProtectionForest` continuar a ter êxito, mas são desnecessários.

   > [!NOTE]
   > Para que `Register-AzureADPasswordProtectionForest` tenha êxito, pelo menos, um Windows Server 2012 ou posterior domínio controlador tem de estar disponível no domínio do servidor de proxy. No entanto, não existe nenhum requisito que o software do agente DC ser instalado em quaisquer controladores de domínio antes deste passo.

6. Opcional: Configure o serviço de proxy de proteção de palavra-passe do Azure AD para escutar numa porta específica.
   * RPC sobre TCP é utilizada pela proteção de palavra-passe do Azure AD software do agente de DC nos controladores de domínio para comunicar com o serviço de proxy de proteção de palavra-passe do Azure AD. Por predefinição, a proteção de palavra-passe do Azure AD service de Proxy de política de palavra-passe escuta em qualquer ponto de extremidade RPC disponível dinâmico. Se for necessário devido a topologia de redes ou requisitos de firewall, o serviço em vez disso, pode ser configurado para escutar numa porta TCP específica.
      * Para configurar o serviço para ser executado sob uma porta estática, utilize o `Set-AzureADPasswordProtectionProxyConfiguration` cmdlet.
         ```
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort <portnumber>
         ```

         > [!WARNING]
         > Tem de parar e reiniciar o serviço para que essas alterações entrem em vigor.

      * Para configurar o serviço para ser executado sob uma porta dinâmica, utilize o mesmo procedimento, mas definido StaticPort novamente para zero, da seguinte forma:
         ```
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort 0
         ```

         > [!WARNING]
         > Tem de parar e reiniciar o serviço para que essas alterações entrem em vigor.

   > [!NOTE]
   > O serviço de proxy de proteção de palavra-passe do Azure AD requer um reinício manual após qualquer alteração na configuração da porta. Não é necessário reiniciar o software de serviço do agente de controlador de domínio em execução em controladores de domínio depois de efetuar alterações de configuração dessa natureza.

   * A configuração atual do serviço pode ser consultada com o `Get-AzureADPasswordProtectionProxyConfiguration` cmdlet como mostrado no exemplo seguinte:

      ```
      Get-AzureADPasswordProtectionProxyConfiguration | fl

      ServiceName : AzureADPasswordProtectionProxy
      DisplayName : Azure AD password protection Proxy
      StaticPort  : 0 
      ```

### <a name="install-the-azure-ad-password-protection-dc-agent-service"></a>Instalar o serviço de agente de proteção de DC de palavra-passe do Azure AD

* Instalar o Azure AD palavra-passe proteção DC agente serviço software com o `AzureADPasswordProtectionDCAgent.msi` pacote MSI:
   * A instalação de software requerem um reinício na instalação e desinstalação devido ao requisito de sistema operativo que as dlls de filtro de palavras-passe são apenas carregados ou descarregados após um reinício.
   * É suportada para instalar o serviço de agente do controlador de domínio numa máquina que ainda não é um controlador de domínio. Neste caso, o serviço será iniciado e executado, mas irá; caso contrário, estar inativo até depois da máquina é promovida para ser um controlador de domínio.

   A instalação de software pode ser automatizada com procedimentos MSI padrão, por exemplo:  `msiexec.exe /i AzureADPasswordProtectionDCAgent.msi /quiet /qn`

   > [!WARNING]
   > O exemplo de comando msiexec irá resultar num reinício imediato; Isso pode ser evitado especificando o `/norestart` sinalizador.

Depois de instalado num controlador de domínio e reiniciado, a proteção de palavra-passe do Azure AD a instalação de software do agente de controlador de domínio está concluída. Nenhuma outra configuração é necessária ou possíveis.

## <a name="multiple-forest-deployments"></a>Várias implementações de floresta

Não existem não requisitos adicionais para implementar a proteção de palavra-passe do Azure AD em várias florestas. Cada floresta independentemente está configurada conforme descrito na secção de implementação única floresta. Cada proteção de palavra-passe do Azure AD Proxy só pode suportar os controladores de domínio da floresta que é associado ao. O software de proteção de palavra-passe do Azure AD de determinada floresta tem conhecimento do software de proteção de palavra-passe do Azure AD implementado na outra floresta, independentemente de configurações de confiança do Active Directory.

## <a name="read-only-domain-controllers"></a>Controladores de domínio só de leitura

Changes\sets de palavra-passe nunca são processados e mantidos em controladores de domínio só de leitura (RODCs); em vez disso, eles são reencaminhados para controladores de domínio gravável. Portanto, não há nenhuma necessidade de instalar o software do agente DC em RODCs.

## <a name="high-availability"></a>Elevada disponibilidade

A principal preocupação com garantindo a alta disponibilidade de proteção de palavra-passe do Azure AD é a disponibilidade dos servidores proxy, quando os controladores de domínio numa floresta estão a tentar transferir novas políticas ou outros dados do Azure. A pré-visualização pública suporta um máximo de dois servidores de proxy por floresta. Cada agente de controlador de domínio utiliza um algoritmo simples de round robin de estilo ao decidir qual o servidor proxy a chamada e ignora a através de servidores proxy que não estão a responder.
Problemas comuns associados à elevada disponibilidade são atenuados pelo design de software do agente de controlador de domínio. O agente de DC mantém um cache local da política de palavra-passe mais recentemente transferida. Mesmo se registrado todos os servidores de proxy se tornar indisponíveis por qualquer motivo, os agentes do DC continuam para impor a política de palavra-passe em cache. Uma frequência de atualização razoável para políticas de palavra-passe numa grande implantação normalmente é sobre a ordem de dias, não horas ou menos.   Por conseguinte breves falhas dos servidores proxy não causam um impacto significativo para a operação da funcionalidade de proteção de palavra-passe do Azure AD ou seus benefícios de segurança.

## <a name="next-steps"></a>Passos Seguintes

Agora que instalou os serviços necessários para proteção de palavra-passe do Azure AD nos seus servidores no local concluir a [pós-instalar configuração e coleta informações de relatórios](howto-password-ban-bad-on-premises-operations.md) para concluir a implementação.

[Descrição geral conceptual de proteção de palavra-passe do Azure AD](concept-password-ban-bad-on-premises.md)
