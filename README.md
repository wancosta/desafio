<p align="center">
<img src="./img/itau.png" style="width:247px;height:143px;" />
</p>

# Case - SRE
#### Executor: Wanderson Costa (wan.costa@gmail.com)
---

Este repositório contém a solução para o desafio de SRE Interview.

---

## **Pré-requisitos**

Pré-requisitos necessário para execução do desafio:

[![Grafana](https://img.shields.io/badge/Grafana-D2691E?style=for-the-badge&logo=Grafana&logoColor=white)](https://grafana.com/)
[![Prometheus](https://img.shields.io/badge/Prometheus-CD853F?style=for-the-badge&logo=Prometheus&logoColor=white)](https://prometheus.io/)
[![OpenTelemetry](https://img.shields.io/badge/OpenTelemetry-1E90FF?style=for-the-badge&logo=OpenTelemetry&logoColor=white)](https://opentelemetry.io/)
[![Elasticsearch](https://img.shields.io/badge/Elasticsearch-A9A9A9?style=for-the-badge&logo=Elasticsearch&logoColor=white)](https://www.elastic.co/)
[![Datadog](https://img.shields.io/badge/Datadog-A020F0?style=for-the-badge&logo=Datadog&logoColor=white)](https://www.datadoghq.com/)

---

## **Estratégias Avaliadas para Execução**

Abaixo, descrevo os itens levados em consideração para uma melhor execução do exercício e detalho o porque de cada decisão:

* **Aplicação:**
    * Upgrade para uma versão mais recente do Klotin e Java (17), corrigindo vulnerabilidades de segurança.
    * Inclusão de endpoint para expor as métricas para o Prometheus.
* **Container:**
    * Build Docker em multistage para ter uma imagem  mais enxuta.
    * Adoção de boas práticas no Dockerfile, como o não uso do usuário root. 
* **Makefile:**
    * Criacao de um Makefile para dar agilidade e celeridade nos testes para construção e destruição do ambiente.
* **Helm Chart:**
    * Criação do Helm Chart com boas práticas de construção de manifestos (readnessProbe, livenessProbe, resource request, resource limit).
* **Terraform:**
    * Criação de estrutura enxuta para deploy da aplicação.
* **Observabilidade:**
    * Análise da perspectiva da infraestrutura e da perspectiva da aplicação.

--- 

## **Observability**

### **1. Práticas de Observabilidade a serem Implementadas**
<details>
<summary>1.1 Para garantir a visibilidade e a capacidade de diagnóstico do sistema, as seguintes práticas de observabilidade devem ser implementadas:</summary>

1. Logging:
   ```bash
   Coleta e armazenamento de logs estruturados para todas as camadas da aplicação.
   ```

2. Métricas:
   ```bash
   Coleta de métricas de infraestrutura e de aplicação para monitoramento de desempenho e saúde do sistema.
   ```
   
3. Tracing:
   ```bash
   Rastreamento distribuído para entender o fluxo das requisições através dos diferentes serviços.
   ```

4. Alerting:
   ```bash
   Configuração de alertas baseados em métricas e logs para detectar anomalias e problemas em tempo real.
   ```

5. Dashboards:
   ```bash
   Criação de painéis visuais para monitoramento em tempo real e análise histórica.
   ```
</details>

<details>
<summary>1.3 Stack de Observabilidade</summary>

1. Prometheus:
   ```bash
   Para coleta e armazenamento de métricas.
   Escolhido por sua integração nativa com Kubernetes e sua capacidade de escalar.
   ```

2. Grafana:
   ```bash
   Para visualização de métricas e criação de dashboards.
   Escolhido por sua flexibilidade e integração com Prometheus.
   ```

3. ELK Stack (Elasticsearch, Logstash, Kibana):
   ```bash
   Para coleta, processamento e visualização de logs.
   Escolhido por sua capacidade de lidar com grandes volumes de dados e sua flexibilidade em consultas.
   ```

4. Jaeger:
   ```bash
   Para rastreamento distribuído.
   Escolhido por sua integração com OpenTracing e sua capacidade de fornecer visibilidade detalhada do fluxo de requisições.
   ```

5. Alertmanager:
   ```bash
   Para gerenciamento de alertas.
   Escolhido por sua integração com Prometheus e sua capacidade de rotear alertas para diferentes canais.
   ```
</details>


<details>
<summary>1.3 Scan Vulnerabilidades Container</summary>

1. Scan da Imagem usando trivy:
   ```bash
   make docker-scan
   ```

2. Resultado:
![Scan Container](img/make_docker_scan.jpg)
</details>

### **2. Deploy no Cluster Kubernetes**
<details>
<summary>2.1 Criar Cluster com Kind</summary>

1. Crie o cluster Kubernetes Local:
    ```bash
    make kind-create-cluster
    ```
2. Carregue a imagem Docker no Cluster Kind:
    ```bash
    make kind-load-image
    ```
</details>

<details>
<summary>2.2 Deploy via Helm Chart</summary>

1. Instale o Helm Chart no Cluster:
    ```bash
    make helm-install
    ```
2. Validar a criação dos recursos:
    ```bash
    kubectl get all
    ```
3. Exposição dos recursos localmente:
    ```bash
    make kind-export-app
    ```   
4. Acessar e validar a aplicação via browser:
    ```bash
    http://localhost:8080/
    ```
5. Desinstalar o Helm Chart do Cluster:
    ```bash
    make helm-uninstall
    ```
</details>

<details>
<summary>2.3 Deploy via Terraform</summary>

1. Instale o Helm Chart no Cluster via Terraform:
    ```bash
    make terraform-apply
    ```
    ![Terraform Apply](img/make_terraform_apply.jpg)
2. Validar a criação dos recursos:
    ```bash
    kubectl get all
    ```
    ![kubectl get all](img/make_get_all.jpg)
    
3. Exposição dos recursos localmente:
    ```bash
    make kind-export-app
    ```
    
4. Acessar e validar a aplicação via browser:
    ```bash
    http://localhost:8080/
    ```
    ![kubectl get all](img/make_expose_app.jpg)
    
5. Desinstalar o Helm Chart no Cluster via Terraform:
    ```bash
    make terraform-destroy
    ```
</details>

### **3. Observabilidade**
<details>
<summary>3.1 Instalação do Prometheus</summary>

1. Instalação do Prometheus:
    ```bash
    make prometheus-install
    ```
2. Exposição do Prometheus:
    ```bash
    make prometheus-access
    ```
3. Acesso via browser:
![Prometheus](img/prometheus.jpg)
</details>

<details>
<summary>3.2 Instalação do Grafana</summary>

1. Instalação do Grafana:
    ```bash
    make grafana-install
    ```
2. Exposição do Grafana:
    ```bash
    make grafana-access
    ```
3. Acesso via browser:
![Grafana](img/grafana.jpg)
</details>

---

## **Implementações a serem consideradas**

Abaixo, descrevo algumas implementações que não foram aplicadas neste exercício, mas tem sua necessidade de execução em um cenário real.

* **Kubernetes - Nodes Multizone:**
    * Para trazer maior disponibilidade e resiliencia, aconselha-se a que o orquestrador de containers tenha seus nodes distribuidos entre as Zonas do Cloud Provider.
* **Kubernetes - Affinity / Anti-Affinity:**
    * Para tirar um maior proveito dos nodes em multizone, se faz necessário implementar regras de Affinity & Anti-Affinity para que as replicas dos componentes não sejam agendadas no mesmo node.
* **Kubernetes - HPA**
    * Para um melhor desempenho e experiencia do usuário, faz sentido refinar o scale dos componentes com HPA ou KEDA, deste modo, atendendo as sazonalidades do uso da aplicação.
