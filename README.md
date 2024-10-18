# Projeto IOT (Sensores DHT e LDR - Vinheria Agnello)

Esse projeto foi desenvolvido para a entrega do Checkpoint 5 de Edge Computing - 2024 1ES da FIAP.

- Guilherme Oliveira Santana de Almeida 555180
- Pedro Camacho 556831
- Rafael Duarte de Freitas 558644
- Rafael Gaspar Bragança Martins 557228

## Introdução

A Internet das Coisas (IoT) refere-se à interconexão de dispositivos físicos, como eletrodomésticos, veículos e sensores, através da internet. Esses dispositivos coletam e compartilham dados, permitindo automação e controle remoto.

#### Conceitos Básicos:
- **Sensores e Atuadores:** Dispositivos que coletam dados do ambiente e executam ações.
- **Conectividade:** Uso de redes (Wi-Fi, Bluetooth, etc.) para comunicação entre dispositivos.
- **Plataformas de Nuvem:** Armazenamento e processamento de dados coletados.
- **Análise de Dados:** Extração de insights a partir dos dados para tomada de decisões.

#### Importância da IoT:
- **Eficiência Operacional:** Automação de processos e otimização de recursos.
- **Tomada de Decisões Informadas:** Dados em tempo real ajudam na identificação de padrões e anomalias.
- **Qualidade de Vida:** Aplicações em saúde, segurança e conforto doméstico.

## Descrição do Projeto

O objetivo deste projeto é utilizar sensores avançados para medir a temperatura, umidade e luminosidade de um ambiente. Esse monitoramento é importante para garantir a correta conservação e mantimento dos vinhos da Vinheria Agnello.

### Como Funciona?

Os sensores abaixo são responsáveis por medir temperatura, umidade e nível de luminosidade em ambientes específicos. Eles são conectados e possuem suas informações coletadas em tempo real para visualização dos usuários interessados.

- **Sensor de Temperatura e Umidade (DHT22):** Identifica a temperatura e a umidade relativa do ar.
- **Sensor de Luminosidade (LDR):** Mede o nível de luminosidade do ambiente.

**Componentes utilizados no projeto:**
- 1 ESP32
- 1 Sensor de temperatura e umidade DHT22
- 1 Sensor LDR de luminosidade

## Arquitetura

O projeto utiliza uma máquina virtual Linux na Azure, onde foi instalado o [Fiware Descomplicado](https://github.com/fabiocabrini/fiware/tree/main), uma ferramenta simples que facilita a configuração dos principais GEs (Generic Enablers) oferecidos pela FIWARE Foundation. Utilizando containers Docker, o Fiware Descomplicado adapta-se facilmente a várias plataformas.

## Desenvolvimento

### Criação de uma VM na Azure

1. Crie uma conta na Azure e uma VM (máquina virtual). Consulte o [tutorial da Azure](https://portal.azure.com/#view/Microsoft_Azure_Resources/QuickstartTutorialBlade/checklistId/get-started-with-azure/sectionId/get-started-deploy-resource/lessonId/get-started-deploy-azure-virtual-machines).
2. Acesse as configurações de rede da VM e libere as portas 4041, 8666, 1026 e 1883 (necessárias para conexão com o broker).

---

### Instalação do Docker

Siga o tutorial oficial da Digital Ocean para instalar o Docker na VM: [Como instalar e utilizar o Docker no Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04-pt).

---

### Instalação do Fiware Descomplicado

1. Clone o repositório do Fiware Descomplicado:

   ```bash
   git clone https://github.com/fabiocabrini/fiware
   ```

2. Acesse a pasta do Fiware:

   ```bash
   cd fiware
   ```

3. Inicie os containers:

   ```bash
   sudo docker compose up -d
   ```

4. Verifique o status dos containers:

   ```bash
   sudo docker stats
   ```

---

### Verificação dos Serviços (Health Check)

Utilize o Postman para realizar o “Health Check” dos componentes “IoT Agent MQTT”, “Orion Context Broker” e “STH-Comet” usando a collection.json do repositório [Fiware Descomplicado](https://github.com/fabiocabrini/fiware/blob/main/FIWARE%20Descomplicado.postman_collection.json).

---

### Instalação do Node-RED

Para exibir os dados coletados pelos sensores IoT em um dashboard visual no **Node-RED**, siga as instruções abaixo:

#### 1. Atualização dos Pacotes da VM

Atualize os pacotes da VM:

```bash
sudo apt update
sudo apt upgrade
```

#### 2. Instalação do Node.js

O Node-RED é baseado em Node.js. Para instalar o Node.js:

```bash
curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -
sudo apt install -y nodejs
```

Verifique a instalação:

```bash
node -v
npm -v
```

#### 3. Instalação do Node-RED

Instale o Node-RED:

```bash
sudo npm install -g --unsafe-perm node-red
```

#### 4. Inicialização do Node-RED

Inicie o Node-RED:

```bash
node-red
```

O Node-RED estará disponível no endereço:

```
http://<IP_DA_VM>:1880
```

#### 5. Instalação do Dashboard no Node-RED

Instale o **node-red-dashboard** para criar gráficos e interfaces:

1. Acesse o menu no Node-RED e selecione “Manage Palette”.
2. Na aba **Install**, procure por **node-red-dashboard** e clique em **Install**.

#### 6. Configuração dos Sensores no Node-RED

Para integrar os dados dos sensores:

1. Adicione um **nó MQTT** no fluxo do Node-RED.
2. Configure-o com o broker MQTT usado pelo Fiware.
3. Utilize tópicos como `sensor/temperatura`, `sensor/umidade`, e `sensor/luminosidade` para cada sensor.

#### 7. Criação do Dashboard

Configure o dashboard para exibir os dados em tempo real:

1. Adicione nós como **gauge**, **chart** ou **text**.
2. Conecte-os aos nós MQTT que estão recebendo os dados dos sensores.
3. Personalize as opções visuais para cada sensor.

Acesse o dashboard no endereço:

```
http://<IP_DA_VM>:1880/ui
```

Segue abaixo o fluxo do Node-red utilizado:

![Fluxo do Node-red](fluxo.png)

---

Segue abaixo a imagem dos dashboards funcionando com os IoTs devidamente conectados nesse processo.

![image](https://github.com/user-attachments/assets/8934fc47-5038-4f2e-ba6d-39530f99091d)

---

### Configuração dos Componentes no Wokwi

Você pode testar o projeto de forma virtual utilizando a plataforma **Wokwi**. Acesse o projeto no [link do Wokwi](https://wokwi.com/projects/410681368165061633).

