---
title: Configuração de VNet de instância de gerida de base de dados SQL do Azure | Documentos da Microsoft
description: Este tópico descreve opções de configuração para uma rede virtual (VNet) com uma instância de gerida de base de dados do Azure SQL.
services: sql-database
author: srdan-bozovic-msft
manager: craigg
ms.service: sql-database
ms.custom: managed instance
ms.topic: conceptual
ms.date: 04/10/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: 0fea91fb067a6d78ef25cb0ff8014b65a8b6a916
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/26/2018
ms.locfileid: "39258105"
---
# <a name="configure-a-vnet-for-azure-sql-database-managed-instance"></a>Configurar uma VNet para a instância gerida de base de dados SQL do Azure

O Azure SQL Database Managed Instance (pré-visualização) deve ser implantado numa do Azure [rede virtual (VNet)](../virtual-network/virtual-networks-overview.md). Esta implementação permite que os seguintes cenários: 
- Ligar-se para uma instância gerida diretamente a partir de uma rede no local 
- Ligar uma instância gerida para o servidor ligado ou de outra-arquivo de dados no local 
- Ligar uma instância gerida nos recursos do Azure  

## <a name="plan"></a>Planear

Planeie como implementar uma instância gerida na rede virtual com as suas respostas às seguintes perguntas: 
- Planeja implantar únicas ou múltiplas instâncias geridas? 

  O número de instâncias geridas determina o tamanho mínimo da sub-rede para alocar para as suas instâncias geridas. Para obter mais informações, consulte [determinar o tamanho da sub-rede da instância gerida](#create-a-new-virtual-network-for-managed-instances). 
- Precisa para implementar a sua instância gerida numa rede virtual existente ou estiver a criar uma nova rede? 

   Se planeia utilizar uma rede virtual existente, terá de modificar essa configuração de rede para acomodar a sua instância gerida. Para obter mais informações, consulte [modificar a rede virtual existente para a instância gerida](#modify-an-existing-virtual-network-for-managed-instances). 

   Se planeja criar nova rede virtual, veja [criar nova rede virtual para a instância gerida](#create-a-new-virtual-network-for-managed-instances).

## <a name="requirements"></a>Requisitos

Para a criação de instância gerida precisa de dedicar sub-rede dentro da VNet que está em conformidade com os seguintes requisitos:
- **Estar em branco**: A sub-rede não pode conter qualquer outro serviço cloud associado a si, e não tem de ser sub-rede do Gateway. Não será possível criar a instância gerida na sub-rede que contém os recursos que não seja a instância gerida ou adicionar outros recursos dentro da sub-rede mais tarde.
- **Nenhum NSG**: A sub-rede não tem de ter um grupo de segurança de rede associados a ele.
- **Tem a tabela de rotas específicas**: A sub-rede tem de ter uma tabela de rota de utilizador (UDR) com Internet de salto seguinte 0.0.0.0/0 como o único caminho atribuído ao mesmo. Para obter mais informações, consulte [criar a tabela de rotas necessárias e associá-la](#create-the-required-route-table-and-associate-it)
3. **DNS personalizado opcional**: se o DNS personalizado é especificado na VNet, endereço IP de resoluções de recursiva do Azure (por exemplo, 168.63.129.16) tem de ser adicionado à lista. Para obter mais informações, consulte [configurar o DNS de personalizado](sql-database-managed-instance-custom-dns.md).
4. **Nenhum ponto de final de serviço**: A sub-rede não tem de ter um ponto final de serviço (armazenamento ou Sql) associado a ele. Certifique-se de que a opção de pontos finais de serviço é desativada durante a criação de VNet.
5. **Endereços IP suficientes**: A sub-rede tem de ter o mínimo de 16 endereços IP. Para obter mais informações, consulte [determinar o tamanho da sub-rede para instâncias geridas](#determine-the-size-of-subnet-for-managed-instances)

> [!IMPORTANT]
> Não poderá implementar a nova instância gerida, se a sub-rede de destino não é compatível com todos os requisitos anteriores. O Vnet de destino e a sub-rede devem ser mantidos em conformidade com esses requisitos de instância gerida (antes e depois da implantação), como qualquer violação pode fazer com que a instância para entrar em estado com falhas e fique indisponível. Recuperar a partir de que estado requer a criação de nova instância numa VNet com as políticas de rede em conformidade, recriar os dados de nível de instância e restaurar as bases de dados. Isto apresenta o período de indisponibilidade significativo para as suas aplicações.

##  <a name="determine-the-size-of-subnet-for-managed-instances"></a>Determinar o tamanho da sub-rede para instâncias geridas

Quando cria uma instância gerida, o Azure aloca um número de máquinas virtuais, dependendo do tamanho de escalão que selecionar durante o aprovisionamento. Uma vez que estas máquinas virtuais estão associadas a sua sub-rede, necessitam de endereços IP. Para garantir a elevada disponibilidade durante operações normais e de manutenção do serviço, o Azure pode alocar máquinas virtuais adicionais. Como resultado, o número de endereços IP necessários numa sub-rede é maior do que o número de instâncias geridas nessa sub-rede. 

Por design, uma instância gerida necessita de um mínimo de 16 endereços IP numa sub-rede e pode utilizar até 256 endereços IP. Como resultado, pode utilizar máscaras de sub-rede /28 para /24 quando definir os intervalos IP de sub-rede. 

Se planeia implementar várias instâncias geridas dentro da sub-rede e precisa para otimizar o tamanho da sub-rede, utilize estes parâmetros para formar um cálculo: 

- O Azure utiliza cinco endereços IP na sub-rede para as suas próprias necessidades 
- Cada instância de fins gerais tem dois endereços 
- Cada uma delas críticas para a empresa precisa quatro endereços

**Exemplo**: planeia ter três para fins gerais e duas empresas instâncias da críticos geridos. Que significa que terá de 5 + 3 * 2 + 2 * 4 = 19 endereços IP. Como intervalos de IP estão definidos na potência de 2, precisa do intervalo IP de 32 (2 ^ 5) endereços IP. Por conseguinte, terá de reservar a sub-rede com a máscara de sub-rede de/27. 

## <a name="create-a-new-virtual-network-for-managed-instances"></a>Criar uma nova rede virtual para instâncias geridas 

Criar uma rede virtual do Azure é um pré-requisito para a criação de uma instância gerida. Pode utilizar o portal do Azure, [PowerShell](../virtual-network/quick-create-powershell.md), ou [CLI do Azure](../virtual-network/quick-create-cli.md). A seção a seguir mostra os passos no portal do Azure. Os detalhes discutidos aqui aplicam-se para cada um desses métodos.

1. Clique em **Criar um recurso**, no canto superior esquerdo do portal do Azure.
2. Localize e, em seguida, clique em **Rede Virtual**, certifique-se de que **Resource Manager** está selecionado como o modo de implementação e, em seguida, clique em **Criar**.

   ![criação de rede virtual](./media/sql-database-managed-instance-tutorial/virtual-network-create.png)

3. Preencha o formulário de rede virtual com as informações pedidas, de forma semelhante a captura de ecrã seguinte:

   ![formulário de criação de rede virtual](./media/sql-database-managed-instance-tutorial/virtual-network-create-form.png)

4. Clique em **Criar**.

   O espaço de endereços e a sub-rede estão especificados na notação CIDR. 

   > [!IMPORTANT]
   > Os valores predefinidos crie a sub-rede que usa todo o espaço de endereço VNet. Se escolher esta opção, não pode criar outros recursos dentro da rede virtual que não seja a instância gerida. 

   A abordagem recomendada seria o seguinte: 
   - Calcular o tamanho da sub-rede seguindo [determinar o tamanho da sub-rede da instância gerida](#determine-the-size-of-subnet-for-managed-instances) secção  
   - Avaliar as necessidades para o resto da VNet 
   - Preencha a intervalos de endereços da VNet e sub-rede em conformidade 

   Certifique-se de que os pontos finais de serviço opção permanece **desativado**. 

   ![formulário de criação de rede virtual](./media/sql-database-managed-instance-tutorial/service-endpoint-disabled.png)

## <a name="create-the-required-route-table-and-associate-it"></a>Criar a tabela de rotas necessárias e associá-la

1. Iniciar sessão no portal do Azure  
2. Localize e, em seguida, clique em **Tabela de rotas** e, em seguida, clique em **Criar** na página Tabela de rotas.

   ![formulário de criação de tabela de rotas](./media/sql-database-managed-instance-tutorial/route-table-create-form.png)

3. Crie uma rota de Internet de salto seguinte 0.0.0.0/0, de forma como as capturas de ecrã seguintes:

   ![adição de tabela de rotas](./media/sql-database-managed-instance-tutorial/route-table-add.png)

   ![rota](./media/sql-database-managed-instance-tutorial/route.png)

4. Associe esta rota à sub-rede para a instância gerida, de forma como as capturas de ecrã seguintes:

    ![sub-rede](./media/sql-database-managed-instance-tutorial/subnet.png)

    ![definir tabela de rotas](./media/sql-database-managed-instance-tutorial/set-route-table.png)

    ![definir a gravação da tabela de rotas](./media/sql-database-managed-instance-tutorial/set-route-table-save.png)


Assim que tiver sido criada a sua VNet, está pronto para criar a sua instância gerida.  

## <a name="modify-an-existing-virtual-network-for-managed-instances"></a>Modificar uma rede virtual existente para as instâncias geridas 

As perguntas e respostas nesta secção mostram-lhe como adicionar uma instância gerida a rede virtual existente. 

**Está a utilizar modelo de implementação clássico ou do Resource Manager para a rede virtual existente?** 

Só pode criar uma instância gerida em redes virtuais do Resource Manager. 

**Gostaria de criar a nova sub-rede da instância gerida de SQL ou utilizar um já existente?**

Se gostaria de criar um novo: 

- Calcular o tamanho da sub-rede, seguindo as diretrizes a [determinar o tamanho da sub-rede para instâncias geridas](#determine-the-size-of-subnet-for-managed-instances) secção.
- Siga os passos em [adicionar, alterar ou eliminar uma sub-rede de rede virtual](../virtual-network/virtual-network-manage-subnet.md). 
- Criar uma tabela de rota que contém uma entrada única **0.0.0.0/0**, como o próximo salto de Internet e associá-la com a sub-rede para a instância gerida.  

No caso de gostaria de criar uma instância gerida dentro de uma sub-rede existente: 
- Verifique se a sub-rede está vazia - uma instância gerida não é possível criar uma sub-rede que contém outros recursos, incluindo a sub-rede do Gateway 
- Calcular o tamanho da sub-rede, seguindo as diretrizes a [determinar o tamanho da sub-rede para instâncias geridas](#determine-the-size-of-subnet-for-managed-instances) secção e certifique-se de que tem o tamanho adequado. 
- Verifique se os pontos finais de serviço não estão ativados na sub-rede.
- Certifique-se de que não existem grupos de segurança de rede associados à sub-rede 

**Tem o servidor DNS personalizado configurado?** 

Se Sim, veja [configurando um DNS personalizado](sql-database-managed-instance-custom-dns.md). 

- Criar a tabela de rotas necessárias e associá-la: veja [criar a tabela de rotas necessárias e associá-la](#create-the-required-route-table-and-associate-it)

## <a name="next-steps"></a>Passos Seguintes

- Para uma descrição geral, consulte [o que é uma instância gerida](sql-database-managed-instance.md)
- Para obter um tutorial que mostra como criar uma VNet, criar uma instância gerida e restaurar uma base de dados a partir de uma cópia de segurança da base de dados, consulte [criar um Azure SQL Database Managed Instance](sql-database-managed-instance-create-tutorial-portal.md).
- Para problemas DNS, consulte [configurar um DNS personalizado](sql-database-managed-instance-custom-dns.md)
