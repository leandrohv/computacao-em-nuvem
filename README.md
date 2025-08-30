# Estudo de Caso – Migração de E-commerce para AWS

## 📑 Sumário
- [🎯 Objetivo](#-objetivo)
- [1) Desafio](#1-desafio)
  - [1.1 Contexto da empresa](#11-contexto-da-empresa)
  - [1.2 Situação atual](#12-situação-atual)
  - [1.3 Objetivos de negócio e técnicos](#13-objetivos-de-negócio-e-técnicos)
  - [1.4 Escopo da migração](#14-escopo-da-migração)
  - [1.5 Restrições e requisitos adicionais](#15-restrições-e-requisitos-adicionais)
  - [1.6 Entregáveis esperados](#16-entregáveis-esperados)
- [📌 Observações](#-observações)

---

## 🎯 Objetivo
Desenhar e implementar uma arquitetura de soluções em nuvem na **AWS** para migrar um e-commerce de médio porte com **alta disponibilidade**, **segurança** e **custos sob controle**.

---

## 1) Desafio

### 1.1 Contexto da empresa
A **Loja Alpha** é um e-commerce de médio porte em forte crescimento. Atualmente roda **on-premises** em uma VM monolítica (Linux) com **Nginx + aplicação** (linguagem à sua escolha: Java Spring, PHP/Laravel ou Python/Django) e **PostgreSQL único**.  

O sistema integra com:
- Gateway de pagamento  
- Transportadoras  
- ERP SaaS  

⚠️ Há picos de tráfego em campanhas e na **Black Friday**.

---

### 1.2 Situação atual
- **Infraestrutura**:  
  - 1 VM x86 (8 vCPU / 16 GB RAM)  
  - Banco PostgreSQL 1 TB  
  - Storage NFS para imagens  

- **Disponibilidade**:  
  - Interrupções durante manutenções  
  - Sem autoscaling  
  - Sem CDN  

- **Segurança**:  
  - Segredos em arquivos `.env`  
  - TLS terminado no Nginx  
  - Sem WAF  

- **Observabilidade**:  
  - Logs locais  
  - Métricas manuais  

---

### 1.3 Objetivos de negócio e técnicos
- **Disponibilidade**: ≥ 99,9% em produção; RTO ≤ 1h, RPO ≤ 5min  
- **Escalabilidade**: escalar horizontalmente API/Frontend nos picos  
- **Desempenho**:  
  - TTFB p95 < 400ms para páginas dinâmicas  
  - Cache de estáticos globalmente  
- **Segurança/Compliance**:  
  - LGPD, OWASP ASVS L2  
  - Segregação de ambientes (dev/stg/prod)  
  - Segredos fora do código  
  - Criptografia em repouso e em trânsito  
  - WAF  
- **Custos**:  
  - Modelo **pay-as-you-go**  
  - Visibilidade por tags e budgets  
- **Ciclo de entrega**:  
  - CI/CD com blue/green  
  - IaC 100%  

---

### 1.4 Escopo da migração
- **Replatform** do monólito para containers  
- Banco → **Amazon Aurora PostgreSQL (Multi-AZ)** com **DMS (CDC)** para migração contínua  
- Armazenar mídias em **S3** + distribuição via **CloudFront**  
- **Autoscaling** sem gestão de servidores (preferir **ECS Fargate**; **EKS** como desafio opcional)  
- Cache (sessões/dados quentes) → **ElastiCache Redis**  
- Borda segura com **AWS WAF + Shield**, **Route 53** e **ALB**  

---

### 1.5 Restrições e requisitos adicionais
- Zero ou quase zero downtime no cutover  
- 3 Zonas de Disponibilidade na região principal (ex.: `sa-east-1` ou `us-east-1`)  
- Pelo menos 2 ambientes: **staging** e **prod** (dev é opcional)  
- Integrações externas devem continuar operando após a migração  

---

### 1.6 Entregáveis esperados
- **Arquitetura lógica e de rede**  
  - Diagramas  
  - Endereçamento CIDR  
  - Fluxo de tráfego  

- **Lista de serviços AWS escolhidos e justificativas**  

- **Pipeline CI/CD**  
  - Stages e gatilhos  
  - Estratégia **blue/green**  

- **Plano de migração de dados**  
  - AWS DMS (full load + CDC)  
  - Estratégia de cutover  

- **Plano de segurança**  
  - IAM  
  - KMS  
  - WAF  
  - SG/NACL  
  - Secrets Manager  
  - LGPD  
  - Backup/Restore  

- **Plano de observabilidade**  
  - Logs, métricas e traces  
  - Alarmes  
  - SLOs/SLIs  

- **Plano de DR/Backup**  
  - RTO/RPO definidos  
  - Testes de restauração  

- **Boas práticas de custos**  
  - Tags, budgets  
  - Políticas de ASG (target tracking)  
  - Right-sizing  

- **IaC**  
  - Skeleton de **Terraform** ou **CloudFormation**  
  - Estrutura modular e com variáveis  
  - (Não precisa estar 100% funcional, mas coerente e modular)  

---

## 📌 Observações
Este estudo de caso pode servir como **prova de conceito** ou **laboratório prático** para arquitetos de nuvem e engenheiros DevOps que desejam validar uma migração realista de e-commerce para AWS.  

📄 Licença. 
MIT License – use/adapte com créditos.  
Última atualização: 30/08/2025.  
Elaborado por: Leandro Venâncio. 
Me segue nas redes sociais: https://linktr.ee/leandro.venancio
