# Script passo a passo dos Checkpoints IaC



### Checkpoint 06 - 2º Semestre

### Passo-a-passo

1. Fazer um **Fork** do repositório [staticsite-multi-cloud](https://github.com/kledsonhugo/staticsite-multi-cloud) 

2. Clone em uma pasta o repositório 

```
git clone https://github.com/NaderSouza/staticsite-multi-cloud-nhs.git
```

3. Crie a **Branch** develop **(Não sabemos se ele vai pedir isso)**

```
git checkout - b develop 
```

4. Entrar na **AWS** e dar start o **LAB** e pegar as credencias e colocar no **GitHub Secrets**

![secrets](/images/secret.png)


5. Entrar na **Azure** e **Criar Grupo de Recursos** clicando em **+** desta forma: (se você tiver algum existente apague)


![gp](/images/gp.png)


6. Próximo passo **Criar uma conta de armazenamento** clicando em **+** desta forma: (se você tiver algum existente apague)


![storage](/images/storage.png)

7. Próximo passo **Criar o container** desta forma:



![container](/images/conta1.png)



> **Note**: Coloquei tfstate1 pois não podia - Mas sempre coloque tfstate**

![container](/images/conta2.png)


8. Entre na **AWS** e crie o **S3** e **DynamoDB** e ja altere com os nomes correspondentes que colocou no **provider.tf**

```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = ">= 5.21"
    }
  }
  backend "s3" {
    bucket         = "staticsite-multicloud-tf-v001-nadin-teste"
    key            = "terraform.tfstate"
    dynamodb_table = "staticsite-multicloud-tf-v001-nadin-teste"
    region         = "us-east-1"
  }
}

provider "aws" {
  region = "us-east-1"
  alias  = "cloud"
}
```


9. Agora via **Terminal** ou **Console** (vscode, cmd, git, fdc kkk) coloque esses comandos com a sua credencial de login da **Azure**:

```
az login -u RM86975@fiap.com.br -p Fiap@*****
```

```
az account list --query "[?user.name=='RM86975@fiap.com.br'].{Name:name, ID:id, Default:isDefault}" --output Table

```
> **Note**: Apos esse comando ele ira te dar o seu **ID**, copie e salve em algum lugar isso 

```
az ad sp create-for-rbac --name sp-terraform-nhs-3 --role Contributor --scopes /subscriptions/41ffcd55-b4c1-4b92-98e9-ab8f64c0c11a

```
> **Note**: Sempre mude nessa linha de codigo o *sp-terraform* acresente algo como eu fiz e altere o ID no final da linha do código


10. Altere os dados como fiz no código a seguir:

```
{
  "appId": "e4af7bb0-e207-47d2-b513-5a98b0cc6d31",
  "displayName": "sp-terraform-nhs-0",
  "password": "3C18Q~4WgTFZo53lMbryHgdAg67~nUFImYo3maeb",
  "tenant": "11dbbfe2-89b8-4549-be10-cec364e59551"
}

-------------------------------------------------------------

Aplicar essas credencias da Azure no Github - 

{
    "clientId": "e4af7bb0-e207-47d2-b513-5a98b0cc6d31",
    "clientSecret": "3C18Q~4WgTFZo53lMbryHgdAg67~nUFImYo3maeb",
    "subscriptionId": "41ffcd55-b4c1-4b92-98e9-ab8f64c0c11a",
    "tenantId": "11dbbfe2-89b8-4549-be10-cec364e59551"
}


No Secrets fica assim:

      ARM_CLIENT_ID - e4af7bb0-e207-47d2-b513-5a98b0cc6d31
      ARM_CLIENT_SECRET - 3C18Q~4WgTFZo53lMbryHgdAg67~nUFImYo3maeb
      ARM_SUBSCRIPTION_ID - 41ffcd55-b4c1-4b92-98e9-ab8f64c0c11a
      ARM_TENANT_ID - 11dbbfe2-89b8-4549-be10-cec364e59551
```

<br>

11. Fazer o comandos para subir o repositório

<br>

```
git add .
```
```
git commit -m "cp-3"
```
```
git push origin develop
```
> **Note**: Se não aparecer o **Actions**, suba mais um **commit** - exemplo: dar algum espaçamento em algum arquivo e salvar 


12. Veja se no **Github Actions** subiu corretamente o pipeline 
<br>
<br>
