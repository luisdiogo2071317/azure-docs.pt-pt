---
title: Implementar a pré-visualização de proteção de palavra-passe do Azure AD
description: Implementar a pré-visualização de proteção de palavra-passe do Azure AD para banir palavras-passe incorretas no local
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 06/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: 6fda373f832d6e24d1252587a19c88b0f464dda6
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232418"
---
# <a name="preview-deploy-azure-ad-password-protection"></a>Pré-visualização: Implementar a proteção de palavra-passe do Azure AD

|     |
| --- |
| Proteção de palavra-passe do Azure AD e a lista de palavras-passe banned personalizadas são funcionalidades de pré-visualização pública do Azure Active Directory. Para mais informações sobre pré-visualizações, consulte [suplementares os termos de utilização para a pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Agora que temos uma compreensão dos [como impor proteção de palavra-passe do Azure AD para o Windows Server Active Directory](concept-password-ban-bad-on-premises.md), o passo seguinte é planear e executar a implementação.

## <a name="deployment-strategy"></a>Estratégia de implementação

Microsoft sugere que qualquer implementação iniciar no modo de auditoria. Modo de auditoria é a definição predefinida da inicial em que as palavras-passe podem continuar a ser definido e os que seria bloqueado criar entradas no registo de eventos. Depois de servidores de proxy e agentes DC totalmente são implementados no modo de auditoria, monitorização regular deve ser feito para determinar a política de palavras-passe que impacto imposição teria no ambiente e os utilizadores se a política tivesse sido imposta.

Durante a fase de auditoria, muitas organizações encontrar:

* Que precisam para melhorar os processos operacionais existentes para utilizar as palavras-passe mais seguras.
* Os utilizadores são habituar para regularmente escolher as palavras-passe não seguros
* Precisam de informar os utilizadores sobre a alteração futura à imposição de segurança, o impacto poderá ter nos mesmos e ajudá-los melhor compreender como que possam escolher as palavras-passe mais seguras.

Depois da funcionalidade tem estado em execução no modo de auditoria durante um período de tempo razoável, a configuração de imposição pode ser flipped de **auditoria** para **impor** assim que necessitam de palavras-passe mais seguras. Monitorização focados durante este tempo é uma boa ideia.

## <a name="known-limitation"></a>Limitação conhecida

Há uma limitação conhecida na versão de pré-visualização do proxy de proteção de palavra-passe do Azure AD. Não é suportada a utilização de contas de administrador de inquilino exigir a MFA. Atualização futura do proxy de proteção de palavra-passe do Azure AD suportar proxy de registo com contas de administrador que exigir a MFA.

## <a name="single-forest-deployment"></a>Implementação de floresta única

A pré-visualização da proteção de palavra-passe do Azure AD é implementada com o serviço de proxy no até dois servidores e o serviço do agente DC pode ser implementado incremental em todos os controladores de domínio na floresta do Active Directory.

![A forma como os componentes de proteção de palavra-passe do Azure AD funcionam em conjunto](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

### <a name="download-the-software"></a>Transferir o software

Existem dois programas de instalação necessários para a proteção de palavra-passe do Azure AD que pode ser transferido a partir de [Centro de transferências da Microsoft](https://www.microsoft.com/download/details.aspx?id=57071)

### <a name="install-and-configure-the-azure-ad-password-protection-proxy-service"></a>Instalar e configurar o serviço de proxy de proteção de palavra-passe do Azure AD

1. Escolha um ou mais servidores para alojar o serviço de proxy de proteção de palavra-passe do Azure AD.
   * Cada um desses serviços pode fornecer apenas políticas de palavra-passe para uma única floresta e a máquina do anfitrião tem de ser associado a um domínio a um domínio (raiz e subordinadas são ambos igualmente suportadas) nessa floresta. Para o serviço proxy de proteção de palavra-passe do Azure AD satisfazer o missão, deve existir conectividade de rede entre a pelo menos um DC em cada domínio da floresta e o computador do anfitrião do Proxy de proteção do Azure AD palavra-passe.
   * Este é suportada para instalar e executar o serviço de proxy de proteção de palavra-passe do Azure AD num controlador de domínio para fins de teste, mas, em seguida, requer conectividade à internet.

   > [!NOTE]
   > A pré-visualização pública suporta um máximo de dois (2) servidores de proxy por floresta.

2. Instale o software do serviço de Proxy de política de palavra-passe utilizando o pacote AzureADPasswordProtectionProxy.msi MSI.
   * A instalação de software não requer um reinício. A instalação de software pode ser automatizada utilizando padrão MSI procedimentos, por exemplo: `msiexec.exe /i AzureADPasswordProtectionProxy.msi /quiet /qn`

3. Abra uma janela do PowerShell como administrador.
   * A proteção de palavra-passe do Azure AD Proxy software inclui um novo módulo do PowerShell com o nome AzureADPasswordProtection. Os seguintes passos baseiam-se no executar vários cmdlets este módulo do PowerShell e partem do princípio de que tem de abrir uma nova janela do PowerShell e tenha importado de novo módulo da seguinte forma:
      * `Import-Module AzureADPasswordProtection`

      > [!NOTE]
      > O software de instalação modifica a variável de ambiente de PSModulePath do computador anfitrião. Por ordem para que esta alteração tenha efeito, para que o módulo do powershell AzureADPasswordProtection possam ser importado e utilizado, poderá ter de abrir uma nova janela da consola do PowerShell.

   * Verifique se o serviço está a ser executado utilizando o seguinte comando do PowerShell: `Get-Service AzureADPasswordProtectionProxy | fl`.
      * O resultado deve produzir o resultado com a **estado** devolver um resultado "Em execução".

4. Registe o proxy.
   * Depois de ter sido concluído o passo 3 o serviço de proxy de proteção do Azure AD palavra-passe está em execução no computador, mas ainda não tiver as credenciais necessárias para comunicar com o Azure AD. Registo com o Azure AD é obrigatório para ativar essa capacidade, utilizando o `Register-AzureADPasswordProtectionProxy` cmdlet do PowerShell. O cmdlet requer o administrador global de credenciais para o seu inquilino do Azure, bem como no local privilégios de administrador de domínio do Active Directory no domínio de raiz de floresta. Assim que foi bem sucedida para um determinado proxy de serviço, adicionais invocações de `Register-AzureADPasswordProtectionProxy` continuam a ter êxito, mas são desnecessários.
      * O cmdlet poderá ser executado da seguinte forma:

         ```
         $tenantAdminCreds = Get-Credential
         Register-AzureADPasswordProtectionProxy -AzureCredential $tenantAdminCreds
         ```

         O exemplo só funciona se o utilizador atualmente com sessão iniciado também é um administrador de domínio do Active Directory para o domínio de raiz. Uma alternativa consiste em fornecer as credenciais de domínio necessários através de `-ForestCredential` parâmetro.

   > [!TIP]
   > Pode haver um atraso considerável (muitos segundos) na primeira vez que este cmdlet é executado para um determinado inquilino do Azure antes do cmdlet conclusão da execução. A menos que é considerada uma falha este atraso não deve ser considerado alarming.

   > [!NOTE]
   > Registo do serviço de proxy de proteção de palavra-passe do Azure AD é esperado um única passo na duração do serviço. O serviço de proxy efetuará automaticamente quaisquer outro maintainenance necessária a partir deste ponto e superior. Depois de esta é concluída com êxito para uma determinado floresta, adicionais invocações de 'Registo AzureADPasswordProtectionProxy' continuam a ter êxito, mas são desnecessárias.

5. Registe a floresta.
   * A floresta do Active Directory no local tem de ser inicializada com as credenciais necessárias para comunicar com o Azure com o `Register-AzureADPasswordProtectionForest` cmdlet do Powershell. O cmdlet requer o administrador global de credenciais para o seu inquilino do Azure, bem como no local privilégios de administrador de domínio do Active Directory no domínio de raiz de floresta. Este passo é executado uma vez por floresta.
      * O cmdlet poderá ser executado da seguinte forma:

         ```
         $tenantAdminCreds = Get-Credential
         Register-AzureADPasswordProtectionForest -AzureCredential $tenantAdminCreds
         ```

         O exemplo só funciona se o utilizador atualmente com sessão iniciado também é um administrador de domínio do Active Directory para o domínio de raiz. Uma alternativa consiste em fornecer as credenciais de domínio necessários através do parâmetro - ForestCredential.

         > [!NOTE]
         > Se vários servidores de proxy estão instalados no seu ambiente, não importa a qual o procedimento acima acima é executado após ao servidor proxy específico.

         > [!TIP]
         > Pode haver um atraso considerável (muitos segundos) na primeira vez que este cmdlet é executado para um determinado inquilino do Azure antes do cmdlet conclusão da execução. A menos que é considerada uma falha este atraso não deve ser considerado alarming.

   > [!NOTE]
   > Registo de floresta do Active Directory é esperado um única passo na duração da floresta. Os agentes de controlador de domínio em execução na floresta efetuará automaticamente quaisquer outro maintainenance necessária a partir deste ponto e superior. Depois de ser concluída com êxito para uma determinada floresta, adicionais invocações de `Register-AzureADPasswordProtectionForest` continuam a ter êxito, mas são desnecessários.

6. Opcional: Configure o serviço de proxy de proteção de palavra-passe do Azure AD para escutar numa porta específica.
   * RPC através de TCP é utilizada pela proteção de palavra-passe do Azure AD software do agente de DC nos controladores de domínio para comunicar com o serviço de proxy de proteção de palavra-passe do Azure AD. Por predefinição, a proteção de palavra-passe do Azure AD serviço Proxy de política de palavra-passe escuta em qualquer ponto de final RPC disponível dinâmico. Se necessário, devido a topologia de rede ou os requisitos da firewall, o serviço em vez disso, pode ser configurado para escutar numa porta TCP específica.
      * Para configurar o serviço a ser executado sob uma porta estática, utilize o `Set-AzureADPasswordProtectionProxyConfiguration` cmdlet.
         ```
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort <portnumber>
         ```

         > [!WARNING]
         > Tem de parar e reiniciar o serviço para estas alterações serem aplicadas.

      * Para configurar o serviço a ser executado sob uma porta dinâmica, utilize o mesmo procedimento mas definido StaticPort novamente para zero, desta forma:
         ```
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort 0
         ```

         > [!WARNING]
         > Tem de parar e reiniciar o serviço para estas alterações serem aplicadas.

   > [!NOTE]
   > O serviço de proxy de proteção de palavra-passe do Azure AD requer um reinício manual após qualquer alteração na configuração da porta. Não é necessário reiniciar o software de serviço do agente de DC em execução em controladores de domínio depois de efetuar as alterações de configuração deste natureza.

   * A configuração atual do serviço pode ser consultada utilizando o `Get-AzureADPasswordProtectionProxyConfiguration` cmdlet como o seguinte exemplo mostra:

      ```
      Get-AzureADPasswordProtectionProxyConfiguration | fl

      ServiceName : AzureADPasswordProtectionProxy
      DisplayName : Azure AD password protection Proxy
      StaticPort  : 0 
      ```

### <a name="install-the-azure-ad-password-protection-dc-agent-service"></a>Instalar o serviço de agente de proteção DC de palavra-passe do Azure AD

* Instalar o Azure AD palavra-passe proteção DC agent serviço software utilizando o `AzureADPasswordProtectionDCAgent.msi` o pacote MSI é:
   * A instalação de software exigir o reinício do sistema na instalação e desinstalação devido ao requisito de sistema operativo que dlls de filtro de palavras-passe são apenas carregados ou descarregados após um reinício.
   * É suportada para instalar o serviço de agente do DC num computador que ainda não é um controlador de domínio. Neste caso, o serviço será iniciado e executar, mas será; caso contrário, depois de estar inativo até que a máquina é promovida para ser um controlador de domínio.

   A instalação de software pode ser automatizada utilizando padrão MSI procedimentos, por exemplo:  `msiexec.exe /i AzureADPasswordProtectionDCAgent.msi /quiet /qn`

   > [!WARNING]
   > O comando de msiexec de exemplo irá resultar num reinício imediato; Isto pode ser evitado, especificando o `/norestart` sinalizador.

Depois de instalado num controlador de domínio e reiniciado, a proteção de palavra-passe do Azure AD a instalação de software do agente DC está concluída. Não é outra configuração necessária ou possíveis.

## <a name="multiple-forest-deployments"></a>Múltiplas implementações de floresta

Não existem não requisitos adicionais para implementar a proteção de palavra-passe do Azure AD em várias florestas. Cada floresta independentemente está configurada conforme descrito na secção de implementação única floresta. Cada proteção de palavra-passe do Azure AD Proxy só pode suportar os controladores de domínio da floresta que é associado ao. O software de proteção de palavra-passe do Azure AD numa floresta especificado não tem conhecimento do software de proteção de palavra-passe do Azure AD implementado na outra floresta, independentemente de configurações de confiança do Active Directory.

## <a name="read-only-domain-controllers"></a>Controladores de domínio só de leitura

Palavra-passe changes\sets nunca são processados e persistente em controladores de domínio só de leitura (RODCs); em vez disso, estes são reencaminhados para controladores de domínio gravável. Por isso não é necessário para instalar o software do agente DC em RODCs.

## <a name="high-availability"></a>Elevada disponibilidade

A principal preocupação com garantir a disponibilidade elevada de proteção de palavra-passe do Azure AD é a disponibilidade dos servidores proxy, quando os controladores de domínio numa floresta estão a tentar transferir novas políticas ou outros dados a partir do Azure. A pré-visualização pública suporta um máximo de dois servidores de proxy por floresta. Cada agente DC utiliza um algoritmo de estilo de round robin simples quando decidir qual o servidor proxy a chamada e ignora a ativação pós-falha de servidores de proxy não estão a responder.
Os habituais problemas relacionados com elevada disponibilidade são atenuados pela estrutura do software do agente DC. O agente de DC mantém uma cache local da política de palavra-passe mais recentemente transferida. Mesmo se registou todos os servidores de proxy se tornar indisponíveis por qualquer motivo, dos agentes DC continuam para impor a respetiva política de palavra-passe em cache. Uma frequência de atualização razoáveis para políticas de palavra-passe numa implementação grande é normalmente na ordem de dias, não horas ou menos.   Por conseguinte falhas breves dos servidores proxy não causam um impacto significativo para a operação da funcionalidade de proteção de palavra-passe do Azure AD ou suas vantagens de segurança.

## <a name="next-steps"></a>Passos Seguintes

Agora que instalou os serviços necessários para a proteção de palavra-passe do Azure AD nos seus servidores no local concluir o [pós-instalar configuração e recolha de informações de relatório](howto-password-ban-bad-on-premises-operations.md) para concluir a implementação.

[Descrição geral conceptual de proteção de palavra-passe do Azure AD](concept-password-ban-bad-on-premises.md)
