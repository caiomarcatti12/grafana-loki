# Configurando o promtail e o loki com docker
Vai ser mamão com açucar a instalção com docker!

# Pré Requisitos
Certifique-se de ter tudo instalado, se não, não irá funcionar.

- Docker
- Docker Compose

# Configurando
Não tem segredo! Vamos criar 2 arquivinhos. Pode ser em qualquer pasta do pc!

- Copie e cole o conteudo abaixo em um arquivo com nome de **docker-compose.yaml**
```
version: "3.9"

networks:
  loki:
    driver: bridge

services:
  loki:
    image: grafana/loki:2.7.1
    ports:
      - "3100:3100"
    command: -config.file=/etc/loki/local-config.yaml
    networks:
      - loki

  promtail:
    image: grafana/promtail:2.7.1
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - ./promtail-custom-config.yaml:/etc/promtail/promtail-custom-config.yml
    command: -config.file=/etc/promtail/promtail-custom-config.yml
    networks:
      - loki
```

- Copie e cole o conteudo abaixo em um arquivo com nome de **promtail-custom-config.yml**
```
server:
  http_listen_port: 9080
  grpc_listen_port: 0

clients:
  - url: http://loki:3100/loki/api/v1/push

scrape_configs:
  - job_name: docker
    # use docker.sock to filter containers
    docker_sd_configs:
      - host: "unix:///var/run/docker.sock"
        refresh_interval: 15s
    # use container name to create a loki label
    relabel_configs:
      - source_labels: ['__meta_docker_container_name']
        regex: '/(.*)'
        target_label: 'container'
```

- Agora no terminal, acesse a apsta que você gravou esses dois arquivos e execute o comando

```
docker-compose up -d
```

Pronto, instalado e rodando.