Tem de criar uma VNet e uma sub-rede de gateway em primeiro lugar, antes de trabalhar as seguintes tarefas.

> [!NOTE]
> Estes exemplos não se aplicam ao S2S/ExpressRoute coexistentes configurações.
> Para obter mais informações sobre como trabalhar com gateways numa configuração coexist, consulte [configurar ligações coexistentes](../articles/expressroute/expressroute-howto-coexist-classic.md#gw)

## <a name="add-a-gateway"></a>Adicionar um gateway

Utilize o comando abaixo para criar um gateway. Não se esqueça de substituir quaisquer valores para os seus próprios.

```powershell
New-AzureVNetGateway -VNetName "MyAzureVNET" -GatewayName "ERGateway" -GatewayType DynamicRouting -GatewaySKU  Standard
```

## <a name="verify-the-gateway-was-created"></a>Certifique-se de que a criação do gateway

Utilize o comando abaixo para verificar se o gateway foi criado. Este comando também obtém o ID de gateway, o que precisa para outras operações.

```powershell
Get-AzureVNetGateway
```

## <a name="resize-a-gateway"></a>Redimensionar um gateway

Há uma série de [SKUs de Gateway](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Pode utilizar o seguinte comando para alterar o SKU de Gateway em qualquer altura.

> [!IMPORTANT]
> Este comando não funciona para UltraPerformance gateway. Para alterar o gateway para um gateway UltraPerformance, remova primeiro o gateway do ExpressRoute existente e, em seguida, crie um novo gateway de UltraPerformance. Para mudar o gateway de um gateway UltraPerformance, remova primeiro o gateway de UltraPerformance e, em seguida, crie um novo gateway. 
>
>

```powershell
Resize-AzureVNetGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance
```

## <a name="remove-a-gateway"></a>Remover um gateway

Utilize o comando abaixo para remover um gateway

```powershell
Remove-AzureVnetGateway -GatewayId <Gateway ID>
```