# desafio-code-girls-one-ec2
Documentação e anotações do desafio de AWS EC2 da DIO. Este repositório serve como material de apoio para consolidar e demonstrar o aprendizado sobre o gerenciamento de instâncias EC2.

### **README: Projeto de Arquitetura AWS para Aplicação de Ponto de Venda (PDV)**

#### **Visão Geral**
Este projeto documenta uma arquitetura de referência para uma aplicação de Ponto de Venda (PDV) na nuvem AWS. O objetivo principal é criar uma solução que seja **altamente disponível**, **escalável** e **segura**, capaz de lidar com picos de tráfego e garantir a resiliência em caso de falhas.

A arquitetura foi construída seguindo as boas práticas do AWS Well-Architected Framework, utilizando serviços gerenciados para simplificar a operação e otimizar custos.

---

### **Diagrama da Arquitetura**
A arquitetura é representada no seguinte diagrama:

![Diagrama da Arquitetura AWS](images/DiagramadesafioEc2AWS.jpg)

---

### **Componentes da Arquitetura**

| Componente | Função |
| :--- | :--- |
| **VPC** | É a sua rede virtual privada na AWS, isolando todos os recursos do projeto em um ambiente seguro. |
| **Internet Gateway** | Permite que o tráfego da internet entre na sua VPC, atuando como o ponto de entrada principal. |
| **Load Balancer** | Recebe todo o tráfego da internet e o distribui de forma uniforme para as instâncias da aplicação. Ele também monitora a saúde das instâncias, garantindo que o tráfego seja enviado apenas para servidores saudáveis. |
| **Availability Zones (AZs)** | A arquitetura utiliza duas AZs para garantir que, em caso de falha de uma delas, o serviço continue operando na outra, proporcionando alta disponibilidade. |
| **Auto Scaling Group (ASG)** | Gerencia a quantidade de instâncias da aplicação (EC2). Ele escala horizontalmente (cria novas instâncias) para lidar com o aumento de tráfego e reduz o número de instâncias quando o tráfego diminui, otimizando os custos. |
| **Instância EC2 T3** | São os servidores virtuais que hospedam a aplicação PDV. O ASG garante que sempre haja instâncias operacionais em ambas as AZs. |
| **Sub-rede Pública** | Contém os recursos que precisam ser acessíveis pela internet, como o Load Balancer e as instâncias EC2 da aplicação. |
| **Sub-rede Privada** | Contém os recursos que não podem ser acessados pela internet, como o banco de dados. Isso adiciona uma camada extra de segurança. |
| **Banco RDS (Multi-AZ)** | É o serviço de banco de dados gerenciado pela AWS. A configuração **Multi-AZ** cria uma réplica automática em uma AZ diferente, garantindo alta disponibilidade e failover rápido em caso de falha da instância primária. |
| **Security Groups** | Agem como firewalls virtuais, controlando o tráfego de entrada e saída de cada componente. Eles garantem que apenas a aplicação possa se comunicar com o banco de dados. |

---

### **Fluxo de Dados**
1.  O tráfego dos usuários entra na **VPC** através do **Internet Gateway**.
2.  O **Load Balancer** recebe o tráfego e o distribui para as instâncias da aplicação, gerenciadas pelo **Auto Scaling Group**.
3.  O **Auto Scaling Group** garante que haja sempre um número adequado de instâncias EC2 rodando nas duas **Availability Zones**.
4.  As instâncias EC2, que estão na **sub-rede pública**, se comunicam com o **Banco RDS**, localizado na **sub-rede privada**.
5.  O **RDS (Multi-AZ)** garante a alta disponibilidade do banco de dados, protegendo-o de falhas em uma única AZ.
6.  Os **Security Groups** controlam o acesso, garantindo que apenas a aplicação possa acessar o banco de dados.

---

### **Benefícios da Arquitetura**
* **Alta Disponibilidade:** A aplicação e o banco de dados estão protegidos contra falhas de AZ.
* **Escalabilidade:** A arquitetura se ajusta automaticamente para lidar com o volume de tráfego, evitando lentidões e interrupções.
* **Segurança:** A separação de sub-redes e o uso de Security Groups protegem a camada de dados de acessos não autorizados.
* **Resiliência:** A aplicação é capaz de se recuperar de falhas sem intervenção manual.
