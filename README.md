
# 🧪 CI/CD - Java Libraries & Applications

Este repositório contém os workflows reutilizáveis e pipelines padronizados para build, deploy e release de bibliotecas e aplicações Java com Maven, Docker e Terraform.

---

## 📦 Workflows Reutilizáveis

| Workflow | Função |
|----------|--------|
| `build-java-maven.yaml` | Build & Test de projetos Java com Maven |
| `deploy-java-maven-snapshot.yaml` | Deploy de versões `-SNAPSHOT` no Nexus |
| `deploy-java-maven-release-candidate.yaml` | Gera versão `RC`, publica no Nexus e cria tag |
| `deploy-java-maven-release.yaml` | Publica versão final, cria GitHub Release e faz bump da `develop` |
| `create-docker-image.yaml` | Gera imagem Docker e publica no ECR |
| `apply-terraform-infra.yaml` | Aplica infraestrutura com Terraform baseado no ambiente |
| `open-pr.yaml` | Cria Pull Request automático de `feature/*` para `develop` |

---

## 🚀 Fluxos principais

### 🔹 Feature (`feature/*`)
- Build & Test
- Validação da infraestrutura (sem aplicar)
- Criação automática de Pull Request para `develop`

### 🔹 Develop
- Build & Test
- Deploy Maven com `-SNAPSHOT`
- Build de imagem Docker
- Aplicação da infraestrutura no ambiente dev

### 🔹 Pre-Release (`release/**`)
- Build & Test
- Cálculo automático da próxima versão RC (`1.0.0-RC1`, `-RC2`, ...)
- Deploy Maven com RC
- Docker build e Terraform em ambiente homologação

### 🔹 Release (Final)
- Remove sufixo RC, define versão `1.0.0`
- Deploy Maven + GitHub Release + Tag
- Docker image com tag final
- Bump automático da branch `develop` para próxima `-SNAPSHOT`

---

## ⚙️ Inputs comuns

| Campo | Descrição | Exemplo |
|-------|-----------|---------|
| `java-version` | Versão do Java | `21` |
| `aws-region` | Região da AWS para deploy | `us-east-1` |
| `environment` | Ambiente (`dev`, `hom`, `prod`) | `dev` |
| `base_version` | Versão propagada entre workflows | `1.2.0-RC1` |

---

## 🔐 Secrets necessários

| Nome | Utilização |
|------|------------|
| `GITHUB_TOKEN` | Criação de PRs e GitHub Releases |
| `NEXUS_USERNAME` / `NEXUS_PASSWORD` | Deploy Maven |
| `AWS_ACCESS_KEY_ID` / `AWS_SECRET_ACCESS_KEY` | Deploy Docker e Terraform |

---

## ✅ Boas práticas adotadas

- Workflows reutilizáveis com `workflow_call`
- Modularização entre build, deploy, release e infra
- Controle de versão automático (RC, final, snapshot)
- Cache de dependências Maven via `pom.xml` hash
- Tags e GitHub Releases automatizados
- Integração completa com AWS (ECR + Terraform)

---
