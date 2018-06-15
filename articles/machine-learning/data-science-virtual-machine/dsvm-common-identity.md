---
title: O programa de configuração identidade comum para dados de ciência de Máquina Virtual - Azure | Microsoft Docs
description: Configure uma identidade comum nos ambientes de DSVM da equipa de empresa.
keywords: ligação avançada learning AI, ferramentas de ciência de dados, máquina de virtual de ciência de dados, geoespacial análise, o processo de ciência de dados de equipa
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: gokuma
ms.openlocfilehash: 70c6b8cd147cefaa3128bc1e6a414a6fa61dba6d
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "34830899"
---
# <a name="setup-common-identity-on-the-data-science-vm"></a>Uma identidade comum na VM de ciência de dados de configuração

Por predefinição, numa VM do Azure, incluindo o utilizador local de VM de ciência de dados (DSVM) são criadas contas ao aprovisionamento da VM e autenticam os utilizadores para a VM com estas credenciais. Se tiver várias VMs que precisem de aceder, esta abordagem pode rapidamente complexa para gerir as credenciais. Contas de utilizador comum e gestão através de que um fornecedor de identidade baseada em normas permite-lhe utilizar um único conjunto de credenciais para aceder a vários recursos no Azure, incluindo DSVMs vários. 

Do Active Directory (AD) é um fornecedor de identidade populares e é suportada no Azure como um serviço, bem como no local. Pode tirar partido do AD ou Azure AD para autenticar os utilizadores no autónoma de VM de ciência de dados (DSVM) ou num cluster de DSVM um conjunto de dimensionamento de máquina virtual do Azure. Isto é feito ao associar as instâncias DSVM a um domínio do AD. Se já tiver um Active Directory para gerir as identidades, pode utilizá-lo como o fornecedor de identidade comuns. No caso de não ter um anúncio, pode executar um AD gerido no Azure através de um serviço chamado [do Azure Active Directory Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/)(Azure AD DS). 

A documentação para [do Azure Active directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/) fornece detalhadas [instruções](https://docs.microsoft.com/azure/active-directory/choose-hybrid-identity-solution#synchronized-identity) gerido do Active Directory, incluindo o Azure AD a ligar ao seu diretório no local se tiver uma. 

O resto deste artigo descreve os passos para configurar um serviço de domínio do AD completamente gerido no Azure através do Azure AD DS e associar o seu DSVMs ao domínio do AD gerido para permitir aos utilizadores aceder a um agrupamento de DSVMs (e outros recursos do Azure) utilizando uma conta de utilizador comum e as credenciais. 

##  <a name="set-up-a-fully-managed-active-directory-domain-on-azure"></a>Configurar um domínio do Active Directory completamente gerido no Azure

Azure AD DS faz com que simples gerir as identidades fornecendo um serviço completamente gerido no Azure. Este domínio do Active Directory, os utilizadores e grupos são geridos.  Os passos para configurar do Azure alojada domínio AD e contas de utilizador no seu diretório estão:

1. Adicionar es ao Active directory no portal 

    a. Iniciar sessão para o [Centro de administração do Azure Active Directory](https://aad.portal.azure.com) com uma conta que seja um administrador global do diretório.
    
    b. Selecione **do Azure Active Directory** e, em seguida, **utilizadores e grupos**.
    
    c. No **utilizadores e grupos**, selecione **todos os utilizadores**e, em seguida, selecione **novo utilizador**.
   
   ![Selecionar o comando Adicionar](./media/add-user.png)
    
    d. Introduza os detalhes para o utilizador, tais como **nome** e **nome de utilizador**. A parte do nome de domínio do nome de utilizador tem de estar ser a predefinição inicial domínio nome "[nome de domínio].onmicrosoft.com" ou um verificado não federada [nome de domínio personalizado](../../active-directory/add-custom-domain.md) , tais como "contoso.com".
    
    e. Copie ou caso contrário, tenha em atenção a palavra-passe do utilizador gerado, para que possa proporcioná-lo para o utilizador após a conclusão deste processo.
    
    f. Opcionalmente, pode abrir e preencha as informações no **perfil**, **grupos**, ou **função de diretório** para o utilizador. 
    
    g. No **utilizador**, selecione **criar**.
    
    h. Distribua em segurança a palavra-passe gerada para o novo utilizador para que o utilizador pode iniciar sessão.

2.  Criar serviços de domínio do Azure AD

    Para criar um Azure adiciona, siga as instruções no artigo "[ativar o Azure Active Directory Domain Services no portal do Azure](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started)" (tarefas 1 a tarefa 5). É importante que o utilizador palavras-passe existentes no Active directory são atualizadas para que a palavra-passe no Azure AD DS é synched. Também é importante adicionar o DNS para o Azure AD DS, tal como indicado na tarefa #4 do artigo acima. 

3.  Criar uma sub-rede separada do DSVM na rede virtual criada na tarefa #2 do passo que precede
4.  Criar uma ou mais instâncias de VM de ciência de dados na sub-rede DSVM 
5.  Siga [instruções](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-join-ubuntu-linux-vm ) adicionar DSVM ao AD. 
6.  Em seguida, Monte um ficheiros partilhados do Azure para alojar o seu diretório inicial ou o bloco de notas para ativar a montar a sua área de trabalho em qualquer máquina. (Se necessita de permissões de nível de ficheiro sólida terá NFS em execução em VMs de uma ou mais)

    a. [Criar uma partilha de ficheiros do Azure](../../storage/files/storage-how-to-create-file-share.md)
    
    b. Montá-la no DSVM Linux. Quando clica no botão "Ligar" para os ficheiros do Azure na sua conta do storage no portal do Azure, o comando na shell de deteção com o Linux DSVM será apresentado. O comando terá um aspeto semelhante isto:
```
sudo mount -t cifs //[STORAGEACCT].file.core.windows.net/workspace [Your mount point] -o vers=3.0,username=[STORAGEACCT],password=[Access Key or SAS],dir_mode=0777,file_mode=0777,sec=ntlmssp
```
7.  Diga, montado os ficheiros do Azure no /data/workspace. Agora Crie diretórios para cada um dos seus utilizadores na partilha. /Data/Workspace/User1, /data/workspace/user2 e assim sucessivamente. Criar um ```notebooks``` diretório na área de trabalho de cada utilizador. 
8. Criar ligações simbólicas para o ```notebooks``` no ```$HOME/userx/notebooks/remote```.   

Agora, tem de utilizadores no active directory alojado no Azure e conseguirão iniciar sessão qualquer DSVM (SSH, Jupyterhub) que está associado para o Azure AD DS utilizando as credenciais do AD. Uma vez que a área de trabalho de utilizador nos ficheiros partilhados do Azure, o utilizador terá acesso para os seus blocos de notas e outras tarefas a partir de qualquer DSVM quando utilizar Jupyterhub. 

Auto dimensionamento, pode utilizar o conjunto para criar um conjunto de VMs que estão associados ao domínio desta forma e com o disco partilhado montados de dimensionamento de máquina virtual. Os utilizadores podem iniciar sessão qualquer máquina disponível no conjunto de dimensionamento de máquina virtual e ter acesso ao disco partilhado, onde são guardados os seus blocos de notas. 

## <a name="next-steps"></a>Passos Seguintes

* [Armazene de maneira segura credenciais para aceder aos recursos na nuvem](dsvm-secure-access-keys.md)



