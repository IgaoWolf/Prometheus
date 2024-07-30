# Prometheus
# Instalação Simplificada do Prometheus

Este guia fornece um passo a passo para instalar e configurar o Prometheus em um servidor Ubuntu. O Prometheus é um sistema de monitoramento e alerta de código aberto.

## 1. Atualizar o Sistema

Primeiro, atualize os pacotes do sistema:

```bash
sudo apt update
sudo apt upgrade -y
```

## 2. Criar Usuário para o Prometheus

Crie um usuário sem diretório home e sem shell para o Prometheus:

```bash

sudo useradd --no-create-home --shell /bin/false prometheus
```

## 3. Baixar e Instalar o Prometheus

Baixe a versão mais recente do Prometheus:

```bash

wget https://github.com/prometheus/prometheus/releases/download/v2.53.1/prometheus-2.53.1.linux-amd64.tar.gz
```
Extraia o arquivo baixado:

```bash

tar -xvf prometheus-2.53.1.linux-amd64.tar.gz
cd prometheus-2.53.1.linux-amd64
```
Mova os binários para o diretório /usr/local/bin/:

```bash

sudo mv prometheus /usr/local/bin/
sudo mv promtool /usr/local/bin/
```

## 4. Configurar Diretórios e Arquivos do Prometheus

Crie os diretórios necessários e ajuste as permissões:

```bash

sudo mkdir /etc/prometheus
sudo mkdir /var/lib/prometheus
sudo chown -R prometheus:prometheus /etc/prometheus
sudo chown -R prometheus:prometheus /var/lib/prometheus
```

Mova os arquivos de configuração e de consola para o diretório apropriado:

```bash

sudo mv consoles /etc/prometheus
sudo mv console_libraries /etc/prometheus
sudo mv prometheus.yml /etc/prometheus
```

## 5. Configurar o Serviço do Prometheus

Crie o arquivo de configuração do sistema para o Prometheus:

```bash

sudo vim /etc/systemd/system/prometheus.service
```

Adicione o seguinte conteúdo ao arquivo:


```bash 

[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
  --config.file /etc/prometheus/prometheus.yml \
  --storage.tsdb.path /var/lib/prometheus/ \
  --web.console.templates=/etc/prometheus/consoles \
  --web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target
```

## 6. Iniciar e Habilitar o Prometheus

Recarregue o daemon do systemd para reconhecer o novo serviço:

```bash

sudo systemctl daemon-reload
```

Inicie o serviço do Prometheus e configure-o para iniciar automaticamente com o sistema:

``` bash

sudo systemctl start prometheus
sudo systemctl enable prometheus
```
## 7. Acesso via browser

http://<IP-SERVER>:9090
