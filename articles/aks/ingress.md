---
title: Configurar entrada com o cluster do serviço de Kubernetes do Azure (AKS)
description: Instalar e configurar um controlador de entrada NGINX num cluster do Azure Kubernetes serviço (AKS).
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/25/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: fcf0b6f3b7f6d75006d8c10aab041c25fc0d8c39
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751290"
---
# <a name="https-ingress-on-azure-kubernetes-service-aks"></a>Entrada HTTPS no serviço Kubernetes do Azure (AKS)

Um controlador de entrada é um fragmento de software que fornece proxy inverso, o encaminhamento de tráfego configurável e terminação de TLS para Kubernetes serviços. Recursos de entrada Kubernetes são utilizados para configurar as regras de entrada e as rotas para serviços Kubernetes individuais. Utilizar um controlador de entrada e regras de entrada, um único endereço externo pode ser utilizado para encaminhar o tráfego para vários serviços num Kubernetes cluster.

Este documento explica como através de uma implementação de exemplo do [controlador de entrada NGINX] [ nginx-ingress] num cluster do Azure Kubernetes serviço (AKS). Além disso, o [Gestor de certificados] [ cert-manager] projeto é utilizado para gerar e configurar automaticamente [vamos encriptar] [ lets-encrypt] certificados. Por fim, várias aplicações são executadas no cluster AKS, cada um dos quais está acessível através de um único endereço.

## <a name="install-an-ingress-controller"></a>Instalar um controlador de entrada

Utilize Helm para instalar o controlador de entrada NGINX. Consulte o controlador de entrada NGINX [documentação] [ nginx-ingress] para obter informações de implementação detalhados.

Este exemplo instala o controlador no `kube-system` espaço de nomes, isto pode ser modificado para um espaço de nomes à sua escolha. Se o cluster AKS não RBAC ativada, adicione `--set rbac.create=false` ao comando. Para obter mais informações, consulte o [gráfico de entrada nginx][nginx-ingress].

```bash
helm install stable/nginx-ingress --namespace kube-system
```

Durante a instalação, é criado um endereço IP público do Azure para o controlador de entrada. Para obter o endereço IP público, utilize o comando de serviço kubectl get. Pode demorar algum tempo para o endereço IP a ser atribuída para este serviço.

```console
$ kubectl get service -l app=nginx-ingress --namespace kube-system

NAME                                       TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
eager-crab-nginx-ingress-controller        LoadBalancer   10.0.182.160   51.145.155.210  80:30920/TCP,443:30426/TCP   20m
eager-crab-nginx-ingress-default-backend   ClusterIP      10.0.255.77    <none>          80/TCP                       20m
```

Porque não existem regras de entrada tem sido criadas, se navegar para o endereço IP público, são encaminhados para a NGINX entrada controladores 404 página predefinida.

![Back-end NGINX de predefinido](media/ingress/default-back-end.png)

## <a name="configure-dns-name"></a>Configurar nome DNS

Porque forem utilizados certificados HTTPS, terá de configurar um nome FQDN para o endereço IP de controladores de entrada. Neste exemplo, é criado um FQDN do Azure com a CLI do Azure. Atualize o script com o endereço IP do controlador de entrada e o nome que pretende utilizar no FQDN.

```bash
#!/bin/bash

# Public IP address
IP="51.145.155.210"

# Name to associate with public IP address
DNSNAME="demo-aks-ingress"

# Get the resource-id of the public ip
PUBLICIPID=$(az network public-ip list --query "[?ipAddress!=null]|[?contains(ipAddress, '$IP')].[id]" --output tsv)

# Update public ip address with dns name
az network public-ip update --ids $PUBLICIPID --dns-name $DNSNAME
```

O controlador de entrada deve ser acessível através do FQDN.

## <a name="install-cert-manager"></a>Instalar o Gestor de certificados

O controlador de entrada NGINX suporta terminação TLS. Quando existem várias formas de obter e configurar certificados para HTTPS, este documento demonstra utilizando [Gestor de certificados][cert-manager], que fornece automática [permite encriptar] [ lets-encrypt] funcionalidade geração e gestão de certificados.

Para instalar o controlador de Gestor de certificados, utilize o seguinte comando do install Helm.

```bash
helm install stable/cert-manager --set ingressShim.defaultIssuerName=letsencrypt-prod --set ingressShim.defaultIssuerKind=ClusterIssuer
```

Se o cluster não for RBAC ativada, utilize este comando.

```bash
helm install stable/cert-manager \
  --set ingressShim.defaultIssuerName=letsencrypt-prod \
  --set ingressShim.defaultIssuerKind=ClusterIssuer \
  --set rbac.create=false \
  --set serviceAccount.create=false
```

Para obter mais informações sobre a configuração do Gestor de certificados, consulte o [projeto do Gestor de certificados][cert-manager].

## <a name="create-ca-cluster-issuer"></a>Criar o emissor de cluster de AC

Antes dos certificados podem ser emitidos, o Gestor de certificados requer um [emissor] [ cert-manager-issuer] ou [ClusterIssuer] [ cert-manager-cluster-issuer] recursos. Os recursos são idênticos nas funcionalidades contudo `Issuer` funciona num único espaço de nomes onde `ClusterIssuer` funciona em todos os espaços de nomes. Para obter mais informações, consulte o [Gestor de certificados de emissor] [ cert-manager-issuer] documentação.

Crie um emissor de cluster utilizando o manifesto seguinte. Atualize o endereço de e-mail com um endereço válido da sua organização.

```yaml
apiVersion: certmanager.k8s.io/v1alpha1
kind: ClusterIssuer
metadata:
  name: letsencrypt-prod
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: user@contoso.com
    privateKeySecretRef:
      name: letsencrypt-prod
    http01: {}
```

## <a name="create-certificate-object"></a>Criar o objeto de certificado

Em seguida, tem de ser criado um recurso de certificado. O recurso de certificado define o certificado x. 509 pretendido. Para obter mais informações, consulte, [cert manager certificados][cert-manager-certificates].

Crie o recurso de certificado com o manifesto seguinte.

```yaml
apiVersion: certmanager.k8s.io/v1alpha1
kind: Certificate
metadata:
  name: tls-secret
spec:
  secretName: tls-secret
  dnsNames:
  - demo-aks-ingress.eastus.cloudapp.azure.com
  acme:
    config:
    - http01:
        ingressClass: nginx
      domains:
      - demo-aks-ingress.eastus.cloudapp.azure.com
  issuerRef:
    name: letsencrypt-prod
    kind: ClusterIssuer
```

## <a name="run-application"></a>Executar a aplicação

Neste momento, um controlador de entrada e de uma solução de gestão de certificados foram configuradas. Agora, execute algumas aplicações no seu cluster AKS.

Neste exemplo, Helm é utilizada para executar várias instâncias de uma aplicação de mundo Olá simples.

Antes de executar a aplicação, adicione o repositório de Helm exemplos do Azure no seu sistema de desenvolvimento.

```bash
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

Execute o gráfico AKS Olá mundo com o seguinte comando:

```bash
helm install azure-samples/aks-helloworld
```

Agora a instalar uma segunda instância da aplicação Olá mundo.

A segunda instância, especifique um título de novo para que as duas aplicações forem visualmente distintas. Também tem de especificar um nome exclusivo de serviço. Estas configurações podem ser vistas no comando seguinte.

```bash
helm install azure-samples/aks-helloworld --set title="AKS Ingress Demo" --set serviceName="ingress-demo"
```

## <a name="create-ingress-route"></a>Criar rota de entrada

Ambas as aplicações são agora em execução no seu cluster Kubernetes, no entanto foram configuradas com um serviço do tipo `ClusterIP`. Como tal, as aplicações não estão acessíveis através da internet. Para disponibilizar o, crie um recurso de entrada Kubernetes. O recurso de entrada configura as regras que encaminhar o tráfego para uma das duas aplicações.

Criar um nome de ficheiro `hello-world-ingress.yaml` e copie o seguinte YAML.

Tome nota que o tráfego para o endereço `https://demo-aks-ingress.eastus.cloudapp.azure.com/` é encaminhado para o serviço com o nome `aks-helloworld`. O tráfego para o endereço `https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two` é encaminhado para o `ingress-demo` serviço.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    certmanager.k8s.io/cluster-issuer: letsencrypt-prod
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  tls:
  - hosts:
    - demo-aks-ingress.eastus.cloudapp.azure.com
    secretName: tls-secret
  rules:
  - host: demo-aks-ingress.eastus.cloudapp.azure.com
    http:
      paths:
      - path: /
        backend:
          serviceName: aks-helloworld
          servicePort: 80
      - path: /hello-world-two
        backend:
          serviceName: ingress-demo
          servicePort: 80
```

Criar o recurso de entrada com o `kubectl apply` comando.

```console
kubectl apply -f hello-world-ingress.yaml
```

## <a name="test-the-ingress-configuration"></a>Testar a configuração de entrada

Navegue para o FQDN do controlador de entrada Kubernetes, deverá ver a aplicação do Olá mundo.

![Exemplo de aplicação um](media/ingress/app-one.png)

Agora navegar para o FQDN do controlador de entrada com o `/hello-world-two` caminho, deverá ver a aplicação do mundo Olá com o título personalizado.

![Exemplo de aplicação dois](media/ingress/app-two.png)

Repare também que a ligação é encriptada e que é utilizado um certificado emitido por vamos encriptar.

![Permite encriptar o certificado](media/ingress/certificate.png)

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o software demonstrado neste documento.

- [Helm CLI][helm-cli]
- [Controlador de entrada NGINX][nginx-ingress]
- [Gestor de certificados][cert-manager]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm#install-helm-cli
[cert-manager]: https://github.com/jetstack/cert-manager
[cert-manager-certificates]: https://cert-manager.readthedocs.io/en/latest/reference/certificates.html
[cert-manager-cluster-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/clusterissuers.html
[cert-manager-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/issuers.html
[lets-encrypt]: https://letsencrypt.org/
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
