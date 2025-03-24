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
[![Grafana](https://img.shields.io/badge/Terraform-483D8B?style=for-the-badge&logo=Terraform&logoColor=white)](https://www.terraform.io/)

---

## **Desafios**

* **Observability**
A solução proposta não conta com nenhuma pratica de Observabilidade implementada, você como SRE recém chegado ao time tem a missão de liderar esta frente.

* **Objetivos:**

    * 1) Definir as praticas de observabilidade que devem ser implementadas no cenário proposto.
    * 2) Definir o que deve ser observado em cada uma das peças que compoem a solução (cdn, object storage, bff, api, cache, banco de dados). (ex: métricas de infraestrutura como cpu, memoria e disco)
    * 3) Definir a stack de observabilidade que será usada na solução (ferramentas) explicando a escolha por cada peça e como ela iria auxiliar no operacional do time

* **Systems Design**
A aplicação `Leaderboard` possui uma defasagem de 3 horas na apresentação dos dados. 
Considerando que existe uma certa complexidade de transformação dos dados durante a execução do `Batch App` realize uma proposta arquitetural para que os dados sejam apresentados  `near-realtime`.
* **Troubleshooting/RCA**
Após o Go-Live da funcionalidade `Leaderboard` no `AppMobile` a monitoração detectou aumento no tempo de resposta durante o carregamento do `Leaderboard`.

    * SLO latência: 3 segundos  
    * SLI latência medido (através de métricas de expostas pela `API`): 7 segundos.
    * Error budget Burn Rate (1h) :  > 1  
* **Importante: Não existe contenção de CPU/Memória/Rede e o auto-scaling está configurado adequadamente em todas as camadas (aplicação, cache, banco de dados).**
Sua missão:  
    * 1) Remover impacto ASAP
    * 2) Apresentar um documento de Postmortem
* **Devops*
    * 1) Como faria a implantação desta solução proposta de forma automatizada? 
Detalhar tecnicamente.
    * 2) O time de produto precisa coletar informações de telemetria relacionadas à experiência do usuário do App para evoluir a usabilidade no futuro. 
Para isso, será necessário disponibilizar uma nova funcionalidade onde o usuário poderá realizar o opt-in/opt-out de envio destas informações. 
Como a implantação desta funcionalidade irá requerer atualização nas camadas de BFF e API, sugira uma estratégia de Rollout que não cause impacto aos usuários que estão navegando no App e que leve em consideração cenários de falha de implan

--- 

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
<summary>1.2 O que deve ser observado em cada componente</summary>

1. CDN:
   ```bash
   Métricas de infraestrutura: CPU, memória, uso de disco.
   Métricas de desempenho: Latência, taxa de transferência, taxa de erros.
   Logs: Acesso, erros, tráfego.
   ```

2. Object Storage:
   ```bash
   Métricas de infraestrutura: Uso de armazenamento, taxa de transferência.
   Métricas de desempenho: Latência de leitura/escrita, taxa de erros.
   Logs: Acesso, erros, operações de leitura/escrita.
   ```
   
3. BFF:
   ```bash
   Métricas de infraestrutura: CPU, memória, uso de disco.
   Métricas de desempenho: Tempo de resposta, taxa de erros, throughput.
   Logs: Requisições, erros, tempos de resposta.
   Tracing: Rastreamento de requisições.
   ```

4. API:
   ```bash
   Métricas de infraestrutura: CPU, memória, uso de disco.
   Métricas de desempenho: Tempo de resposta, taxa de erros, throughput.
   Logs: Requisições, erros, tempos de resposta.
   Tracing: Rastreamento de requisições.
   ```
   
5. Cache:
   ```bash
   Métricas de infraestrutura: Uso de memória, taxa de hit/miss.
   Métricas de desempenho: Latência de leitura/escrita, taxa de erros.
   Logs: Operações de cache, erros.
   ```
   
6. Banco de Dados:
   ```bash
   Métricas de infraestrutura: CPU, memória, uso de disco.
   Métricas de desempenho: Tempo de consulta, taxa de erros, throughput.
   Logs: Consultas, erros, tempos de execução.
   ```
   
</details>
              
   
<details>
<summary>1.3 Stack de Observabilidade</summary>

1. Prometheus:
   ```bash
   Para coleta e armazenamento de métricas. Escolhido por sua integração nativa com Kubernetes e sua capacidade de escalar.
   ```

2. Grafana:
   ```bash
   Para visualização de métricas e criação de dashboards. Escolhido por sua flexibilidade e integração com Prometheus.
   ```

3. ELK Stack (Elasticsearch, Logstash, Kibana):
   ```bash
   Para coleta, processamento e visualização de logs. Escolhido por sua capacidade de lidar com grandes volumes de dados e sua flexibilidade em consultas.
   ```

4. Jaeger:
   ```bash
   Para rastreamento distribuído. Escolhido por sua integração com OpenTracing e sua capacidade de fornecer visibilidade detalhada do fluxo de requisições.
   ```

5. Alertmanager:
   ```bash
   Para gerenciamento de alertas. Escolhido por sua integração com Prometheus e sua capacidade de rotear alertas para diferentes canais.
   ```

6. DataDog:
   ```bash
   Uma opção paga escolhido por oferecer integração simplificada para monitoramento de toda a stack (infraestrutura, aplicações e logs), além de alertas e visualizações.
   ```

</details>

### **2. Systems Design**
<details>
<summary>2.1 Proposta Arquitetural para Dados Near-Realtime</summary>

Para reduzir a defasagem de 3 horas na apresentação dos dados, podemos implementar uma arquitetura baseada em streaming de dados. 
A proposta é a seguinte:


1. Kafka:
   ```bash
   Utilizar o Apache Kafka como um barramento de eventos para capturar os dados de downloads em tempo real.
   ```

2. Stream Processing:
   ```bash
   Utilizar o Apache Flink ou Apache Kafka Streams para processar os dados em tempo real, realizando as transformações necessárias (agregação, classificação).
   ```

3. Banco de Dados em Tempo Real:
   ```bash
   Utilizar um banco de dados como o Apache Cassandra ou Amazon DynamoDB para armazenar os dados processados em tempo real.
   ```

4. API:
   ```bash
   Modificar a API para consultar o banco de dados em tempo real ao invés do banco de dados batch.
   ```

</details>

### **3. Troubleshooting/RCA**
<details>
<summary>3.1 Remover Impacto ASAP</summary>

1. Identificação do Gargalo:
   ```bash
   Utilizar o rastreamento distribuído (Jaeger) para identificar onde a latência está ocorrendo.
   ```

2. Cache Quente:
   ```bash
   Implementar um cache quente para os dados mais acessados, reduzindo a necessidade de consultas ao banco de dados.
   ```

3. Otimização de Consultas:
   ```bash
   Revisar e otimizar as consultas ao banco de dados para reduzir o tempo de resposta.
   ```
</details>

### **4. Documento de Postmortem**
<details>
<summary>4.1 Remover Impacto ASAP</summary>

**Título**: Aumento de Latência no Carregamento do Leaderboard
**Resumo**: Durante o período de pico de tráfego, foi observado um aumento significativo na latência do carregamento do Leaderboard, excedendo o SLO de 3 segundos.
**Causa Raiz**: O aumento na latência foi causado por consultas lentas ao banco de dados devido à falta de indexação adequada e ao aumento no volume de dados.

1. Ações Corretivas:
   ```bash
   Implementação de índices adequados no banco de dados.
   Adição de um cache quente para os dados mais acessados.
   Otimização das consultas ao banco de dados.
   ```

2. Ações Preventivas:
   ```bash
   Revisão periódica das consultas ao banco de dados.
   Implementação de alertas para monitorar a latência das consultas.
   Realização de testes de carga regulares.
   ```
</details>

### **5. SRE/DEVOPS**
<details>
<summary>5.1 Implantação Automatizada</summary>

1. CI/CD:
   ```bash
   Utilizar ferramentas como Jenkins, GitLab CI opara automatizar o pipeline de integração e entrega contínua.
   ```

2. Infraestrutura como Código
   ```bash
   Utilizar Terraform ou CloudFormation para gerenciar a infraestrutura de forma automatizada.
   ```

3. Orquestração de Containers:
   ```bash
   Utilizar Kubernetes para orquestrar a implantação e o scaling dos serviços.
   ```
</details>

<details>
<summary>5.2 Estratégia de Rollout</summary>

1. Canary Deployment:
   ```bash
   Implementar uma estratégia de Canary Deployment para liberar a nova funcionalidade 
   gradualmente para um pequeno grupo de usuários antes de liberar para todos.
   ```

2. Feature Flags:
   ```bash
   Utilizar feature flags para habilitar/desabilitar a funcionalidade de opt-in/opt-out sem a necessidade de reimplantação.
   ```

3. Rollback Automático:
   ```bash
   Configurar o pipeline de CI/CD para realizar rollback automático em caso de falha na implantação.
   ```
</details>

<details>
<summary>5.3 Detalhamento Técnico</summary>

1. Canary Deployment:
   ```bash
   Liberar a nova funcionalidade para 1% dos usuários.
   Monitorar métricas de desempenho e erros.
   Aumentar gradualmente o percentual de usuários até 100%.
   ```     
     
2. Feature Flags
   ```bash
   Utilizar uma biblioteca como LaunchDarkly ou Flagsmith para gerenciar feature flags.
   Habilitar a funcionalidade de opt-in/opt-out via feature flag.   ```
   ```      
      
3. Rollback Automático:
   ```bash
   Configurar o pipeline de CI/CD para monitorar métricas de saúde da aplicação.
   Em caso de falha, reverter automaticamente para a versão estável anterior 
   ```
</details>
Essa abordagem garante uma implantação segura e controlada, minimizando o impacto para os usuários e permitindo uma rápida resposta a eventuais problemas.


---

## **Implementações a serem consideradas**

Abaixo, descrevo algumas implementações que não foram aplicadas neste exercício, mas tem sua necessidade de execução em um cenário real.

* **Kubernetes - Nodes Multizone:**
    * Para trazer maior disponibilidade e resiliencia, aconselha-se a que o orquestrador de containers tenha seus nodes distribuidos entre as Zonas do Cloud Provider.
* **Kubernetes - Affinity / Anti-Affinity:**
    * Para tirar um maior proveito dos nodes em multizone, se faz necessário implementar regras de Affinity & Anti-Affinity para que as replicas dos componentes não sejam agendadas no mesmo node.
* **Kubernetes - HPA**
    * Para um melhor desempenho e experiencia do usuário, faz sentido refinar o scale dos componentes com HPA ou KEDA, deste modo, atendendo as sazonalidades do uso da aplicação.
