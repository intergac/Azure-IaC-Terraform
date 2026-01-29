# ☁️ Azure-IaC-Terraform

> Infrastructure as Code (IaC) para Microsoft Azure usando Terraform

[![Terraform](https://img.shields.io/badge/Terraform-1.6+-623CE4?logo=terraform)](https://www.terraform.io/)
[![Azure](https://img.shields.io/badge/Azure-Cloud-0078D4?logo=microsoft-azure)]()  
[![License](https://img.shields.io/badge/license-MIT-green)]()  
[![Contributions](https://img.shields.io/badge/contributions-welcome-brightgreen)]()  

---

## 🎯 Visão Geral

**Azure-IaC-Terraform** é uma coleção completa de módulos e configurações Terraform para provisionamento automatizado de infraestrutura no Microsoft Azure. Este projeto demonstra as melhores práticas de Infrastructure as Code, facilitando o deployment, gerenciamento e escalonamento de recursos na nuvem.

### 🚀 O que você encontra aqui:

- ☁️ **Máquinas Virtuais (VMs)** - Provisionamento de VMs Windows e Linux
- 💾 **Storage Accounts** - Armazenamento blob, file shares e tables
- 🌐 **Networking** - VNets, Subnets, NSGs, Load Balancers
- ☸️ **Azure Kubernetes Service (AKS)** - Clusters Kubernetes gerenciados
- 📦 **App Service & Functions** - PaaS para aplicações web e serverless
- 📊 **Monitor & Log Analytics** - Observabilidade e monitoramento
- 🔐 **Key Vault** - Gerenciamento de señas criptográficas

---

## 📂 Estrutura do Projeto

```
Azure-IaC-Terraform/
├── environments/              # Ambientes (dev, staging, prod)
│   ├── dev/
│   │   ├── terraform.tfvars
│   │   └── main.tf
│   ├── staging/
│   └── prod/
│
├── modules/                  # Módulos reutilizáveis
│   ├── vm/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── outputs.tf
│   ├── networking/
│   ├── storage/
│   ├── aks/
│   ├── app_service/
│   └── monitoring/
│
├── scripts/                  # Scripts de automação
│   ├── init.sh
│   ├── deploy.sh
│   └── destroy.sh
│
├── documentation/           # Documentação
│   ├── SETUP.md
│   ├── MODULES.md
│   └── BEST_PRACTICES.md
│
├── .github/
│   └── workflows/              # CI/CD GitHub Actions
│       ├── terraform.yml
│       └── deploy.yml
│
├── provider.tf              # Configuração do Provider
├── terraform.tfvars         # Variáveis globais
├── .gitignore              # Git ignore (Terraform)
├── .terraformignore        # Terraform ignore
└── README.md               # Este arquivo
```

---

## 🚀 Começando

### Pré-requisitos

- [Terraform](https://www.terraform.io/downloads) v1.6 ou superior
- [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) v2.40+
- Conta Microsoft Azure ativa
- Git

### Instalação

#### 1. Clonar o repositório

```bash
git clone https://github.com/intergac/Azure-IaC-Terraform.git
cd Azure-IaC-Terraform
```

#### 2. Autenticar com Azure

```bash
az login
az account set --subscription "SUBSCRIPTION_ID"
```

#### 3. Inicializar Terraform

```bash
terraform init
```

#### 4. Validar configuração

```bash
terraform validate
terraform fmt -recursive
```

#### 5. Planejar deployment

```bash
terraform plan -out=tfplan
```

#### 6. Aplicar configurações

```bash
terraform apply tfplan
```

---

## 🗚 Exemplos de Código

### Exemplo 1: Máquina Virtual

```hcl
module "virtual_machine" {
  source = "./modules/vm"
  
  rg_name           = azurerm_resource_group.main.name
  location          = azurerm_resource_group.main.location
  vm_name           = "vm-prod-01"
  vm_size           = "Standard_D2s_v3"
  image_publisher   = "Canonical"
  image_offer       = "UbuntuServer"
  image_sku         = "18.04-LTS"
  subnet_id         = azurerm_subnet.internal.id
  
  tags = {
    Environment = "Production"
    Owner       = "DevOps Team"
  }
}
```

### Exemplo 2: Cluster AKS

```hcl
module "aks_cluster" {
  source = "./modules/aks"
  
  cluster_name        = "aks-prod"
  kubernetes_version  = "1.27"
  node_count          = 3
  vm_size             = "Standard_D4s_v3"
  network_plugin      = "azure"
  network_policy      = "azure"
  
  resource_group_name = azurerm_resource_group.main.name
  location            = azurerm_resource_group.main.location
}
```

---

## 📁 Melhores Práticas

- ✅ **Modularização**: Organize código em módulos reutilizáveis
- ✅ **Versionamento de Estado**: Use Azure Blob Storage para remote state
- ✅ **Variáveis & Secrets**: Nunca commitar credenciais, use Azure Key Vault
- ✅ **Testing**: Valide com `terraform plan` antes de aplicar
- ✅ **Documentation**: Documente cada módulo com README e exemplos
- ✅ **CI/CD**: Integre com GitHub Actions para deploys automáticos
- ✅ **Naming Convention**: Padrões claros para recursos

---

## 📦 Gerenciar Estado (State)

### Configurar Remote State

```hcl
terraform {
  backend "azurerm" {
    resource_group_name  = "rg-tfstate"
    storage_account_name = "tfstateaccount"
    container_name       = "terraform-state"
    key                  = "prod.tfstate"
  }
}
```

---

## 🔄 CI/CD com GitHub Actions

Este projeto inclui workflows automáticos para:

- **Plan**: Validação em pull requests
- **Apply**: Deploy automático em merge para main
- **Destroy**: Destruição controlada de recursos

---

## 💡 Comandos Úteis

```bash
# Validar sintaxe
terraform validate

# Formatar código
terraform fmt -recursive

# Mostrar plano (sem aplicar)
terraform plan

# Aplicar mudanças
terraform apply

# Mostrar estado atual
terraform show

# Listar recursos
terraform state list

# Destruir infraestrutura
terraform destroy

# Importar recurso existente
terraform import azurerm_resource_group.main /subscriptions/{id}/resourceGroups/{name}
```

---

## 🔐 Segurança & Compliance

- 🔑 Usar Azure Key Vault para secrets
- 🔑 Ativar auditoria com Azure Monitor
- 🔑 Implementar RBAC no Azure
- 🔑 Usar Network Security Groups
- 🔑 Backup & Disaster Recovery

---

## 🎓 Recursos Adicionais

- [Documentação Terraform](https://www.terraform.io/docs)
- [Azure Provider para Terraform](https://registry.terraform.io/providers/hashicorp/azurerm/latest)
- [Melhores Práticas Azure IaC](https://learn.microsoft.com/pt-br/azure/cloud-adoption-framework/)
- [Terraform Best Practices](https://www.terraform.io/cloud-docs)

---

## 🤝 Contribuindo

Contribuições são bem-vindas!

1. Fork o repositório
2. Crie uma branch (`git checkout -b feature/nova-feature`)
3. Commit suas mudanças (`git commit -m 'Add nova-feature'`)
4. Push para a branch (`git push origin feature/nova-feature`)
5. Abra um Pull Request

---

## 📋 Licença

Este projeto está licenciado sob MIT License.

---

## 👨‍💼 Autor

**Guilherme de Almeida Campos**
- 🔗 [LinkedIn](https://www.linkedin.com/in/intergac/)
- 🐙 [GitHub](https://github.com/intergac)
- 📊 [Portfólio](https://github.com/intergac)

---

**Desenvolvido com ❤️ para profissionais de Cloud & DevOps**
