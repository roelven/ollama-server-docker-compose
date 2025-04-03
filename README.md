# Run Ollama Server with Docker Compose

This repository allows you to run Ollama server with docker compose.

The official [Ollama Docker image](https://hub.docker.com/r/ollama/ollama)
`ollama/ollama` is available on Docker Hub.

Ollama official [github page](https://github.com/jmorganca/ollama)

Remember you need a Docker account and Docker Desktop app installed to run the commands below.



### CPU

```bash
# CPU only
docker run -d -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama
```

### GPU

```bash
docker run -d --gpus=all -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama
```

## Docker Compose

Run using Docker Compose and the starter `docker-compose.yaml` in this folder:

```bash
docker-compose pull
docker-compose up --wait --detach
```

Here's how to execute `ollama run llama3` commands within the container:

```bash
container_id=$(docker ps | grep ollama | awk '{print $1}')
docker exec -it $container_id ollama run llama3
```

Or here is how to open a bash shell in the container:

```bash
docker exec -it $container_id bash
```
