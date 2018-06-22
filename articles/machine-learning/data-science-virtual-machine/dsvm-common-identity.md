---
title: Configurar uma identidade comum para dados de ciência de Máquina Virtual - Azure | Microsoft Docs
description: Configure uma identidade comum nos ambientes de DSVM uma equipa de empresa.
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
ms.openlocfilehash: d6235f3a425481a13e627d683bb4c3943b473b40
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309828"
---
# <a name="set-up-a-common-identity-on-the-data-science-virtual-machine"></a>Configurar uma identidade comum na máquina de Virtual de ciência de dados

Uma máquina virtual do Azure (VM), incluindo os dados de ciência de Máquina Virtual (DSVM), crie contas de utilizador local ao aprovisionamento de VM. Os utilizadores, em seguida, autenticar-se a VM ao utilizar estas credenciais. Se tiver várias VMs que precisem de aceder, esta abordagem pode obter rapidamente complexa à medida que gere credenciais. Contas de utilizador comum e gestão através de um fornecedor de identidade baseada em normas permitem-lhe utilizar um único conjunto de credenciais para aceder a vários recursos no Azure, incluindo DSVMs vários. 

Active Directory é um fornecedor de identidade populares e é suportada no Azure como um serviço e no local. Pode utilizar o Azure Active Directory (Azure AD) ou no local do Active Directory para autenticar os utilizadores no DSVM autónomo ou num cluster de DSVMs um conjunto de dimensionamento de máquina virtual do Azure. Fazê-lo ao associar as instâncias DSVM a um domínio do Active Directory. 

Se já tiver o Active Directory para gerir as identidades, pode utilizá-lo como o fornecedor de identidade comuns. Se não tiver o Active Directory, pode executar uma instância do Active Directory gerida no Azure através de um serviço chamado [do Azure Active Directory Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/) (Azure AD DS). 

A documentação para [do Azure AD](https://docs.microsoft.com/azure/active-directory/) fornece detalhadas [instruções de gestão](https://docs.microsoft.com/azure/active-directory/choose-hybrid-identity-solution#synchronized-identity), incluindo a ligação do Azure AD para o seu diretório no local, se tiver uma. 

Este artigo descreve os passos para configurar um serviço de domínio do Active Directory completamente gerido no Azure através da utilização do Azure AD DS. Pode, depois, junte os DSVMs ao domínio do Active Directory gerido para permitir aos utilizadores aceder a um agrupamento de DSVMs (e outros recursos do Azure), utilizando uma conta de utilizador comum e as credenciais. 

## <a name="set-up-a-fully-managed-active-directory-domain-on-azure"></a>Configurar um domínio do Active Directory completamente gerido no Azure

Azure AD DS faz com que simples gerir as identidades fornecendo um serviço completamente gerido no Azure. Este domínio do Active Directory, gerir utilizadores e grupos. Os passos para configurar um contas de utilizador e domínio alojado no Azure Active Directory no seu diretório são:

1. No portal do Azure, adicione o utilizador do Active Directory: 

   a. Iniciar sessão para o [Centro de administração do Azure Active Directory](https://aad.portal.azure.com) com uma conta que seja um administrador global do diretório.
    
   b. Selecione **do Azure Active Directory** e, em seguida, **utilizadores e grupos**.
    
   c. No **utilizadores e grupos**, selecione **todos os utilizadores**e, em seguida, selecione **novo utilizador**.
   
      O **utilizador** abre o painel.
      
      ![O painel "Utilizador"](./media/add-user.png)
    
   d. Introduza os detalhes para o utilizador, tais como **nome** e **nome de utilizador**. A parte do nome de domínio do nome de utilizador tem de ser a predefinição inicial domínio nome "[nome de domínio].onmicrosoft.com" ou um verificado não federada [nome de domínio personalizado](../../active-directory/add-custom-domain.md) , tais como "contoso.com".
    
   e. Copie ou caso contrário, tenha em atenção a palavra-passe do utilizador gerado, para que possa proporcioná-lo para o utilizador após a conclusão deste processo.
    
   f. Opcionalmente, pode abrir e preencha as informações no **perfil**, **grupos**, ou **função de diretório** para o utilizador. 
    
   g. No **utilizador**, selecione **criar**.
    
   h. Distribua em segurança a palavra-passe gerada para o novo utilizador para que o utilizador pode iniciar sessão.

2. Crie uma instância do Azure AD DS. Siga as instruções no artigo [ativar o Azure Active Directory Domain Services no portal do Azure](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started) (tarefas 1 a 5). É importante atualizar as utilizador palavras-passe existentes no Active Directory para que a palavra-passe no Azure AD DS está sincronizada. Também é importante adicionar o DNS para o Azure AD DS, conforme descrito na tarefa 4 do artigo. 

3. Crie uma sub-rede separada do DSVM na rede virtual criada na tarefa 2 do passo anterior.
4. Crie uma ou mais instâncias de VM de ciência de dados na sub-rede DSVM. 
5. Siga o [instruções](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-join-ubuntu-linux-vm ) para adicionar DSVM ao Active Directory. 
6. Monte uma partilha de ficheiros do Azure para alojar o seu diretório inicial ou o bloco de notas para ativar a montar a sua área de trabalho em qualquer máquina. (Se necessita de permissões de nível de ficheiro sólidas, terá de NFS em execução num ou mais VMs.)

   a. [Criar uma partilha de ficheiros do Azure](../../storage/files/storage-how-to-create-file-share.md).
    
   b. Montá-la no DSVM Linux. Quando seleciona o **Connect** botão para partilhar os ficheiros do Azure na sua conta do storage no portal do Azure, o comando a executar no Bash shell no DSVM Linux é apresentada. O comando tem o seguinte aspeto:
   
   ```
   sudo mount -t cifs //[STORAGEACCT].file.core.windows.net/workspace [Your mount point] -o vers=3.0,username=[STORAGEACCT],password=[Access Key or SAS],dir_mode=0777,file_mode=0777,sec=ntlmssp
   ```
7. Partem do princípio que montar a partilha de ficheiros do Azure no /data/workspace, por exemplo. Agora criar diretórios para cada um dos seus utilizadores na partilha: /data/workspace/user1, /data/workspace/user2 e assim sucessivamente. Criar um `notebooks` diretório na área de trabalho de cada utilizador. 
8. Criar ligações simbólicas para `notebooks` no `$HOME/userx/notebooks/remote`.   

Agora, tem de utilizadores na sua instância do Active Directory alojada no Azure. Utilizando as credenciais do Active Directory, os utilizadores podem iniciar sessão qualquer DSVM (SSH ou JupyterHub) que está associado ao Azure AD DS. Porque a área de trabalho do utilizador está numa partilha de ficheiros do Azure, os utilizadores têm acesso para os seus blocos de notas e outras tarefas a partir de qualquer DSVM quando estiver a utilizar JupyterHub. 

Para efetuar o dimensionamento automático, pode utilizar um conjunto para criar um conjunto de VMs que estão associados ao domínio desta forma e com o disco partilhado montado de dimensionamento de máquina virtual. Os utilizadores podem iniciar sessão qualquer máquina disponível no conjunto de dimensionamento de máquina virtual e ter acesso para o disco partilhado, onde são guardados os seus blocos de notas. 

## <a name="next-steps"></a>Passos Seguintes

* [Armazene de maneira segura credenciais para aceder aos recursos na nuvem](dsvm-secure-access-keys.md)



