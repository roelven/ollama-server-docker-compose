# Run Ollama Server with Docker Compose

This repository allows you to run an Ollama server with Docker Compose. It exposes the Ollama API endpoint so that other applications can integrate with it.

The official [Ollama Docker image](https://hub.docker.com/r/ollama/ollama) (`ollama/ollama`) is available on Docker Hub.

Ollama official [GitHub page](https://github.com/jmorganca/ollama)

## Requirements

- Docker and Docker Compose installed
- Docker Desktop app (for macOS and Windows users)


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

Run using Docker Compose:

```bash
docker-compose pull
docker-compose up -d
```

### Interacting with Ollama

Pull models from within the container:

```bash
docker exec -it ollama ollama pull phi3:mini-4k
```

Run models interactively:

```bash
docker exec -it ollama ollama run llama3
```

Access the container shell:

```bash
docker exec -it ollama bash
```

### Integration with Other Applications

The Ollama API is available at `http://localhost:11434`. You can configure other applications to use this endpoint:

```
NEXT_PUBLIC_OLLAMA_ENDPOINT_URL=http://localhost:11434
OLLAMA_MODEL=phi3:mini-4k
```

### Stopping and Cleanup

Stop the container:

```bash
docker-compose down
```

Remove all data (models and configuration):

```bash
docker-compose down
rm -rf ./ollama
```
