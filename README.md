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

#### Example: Adding a Client Application to Docker Compose

You can add a client application to your docker-compose.yml file to interact with Ollama. Here's an example client that demonstrates the API:

```yaml
# Example client application that uses Ollama
example-client:
  image: curlimages/curl:latest
  container_name: ollama-example-client
  depends_on:
    ollama:
      condition: service_healthy
  restart: "no"
  command: >
    sh -c "
      # Wait to ensure Ollama is fully ready
      sleep 5
      
      # List available models
      echo '--- Available Models ---'
      curl -s ollama:11434/api/tags | grep name
      
      # Run a basic prompt with the default model
      echo '\\n--- Example Query ---'
      curl -s ollama:11434/api/generate -d '{
        \"model\": \"phi3:mini-4k\",
        \"prompt\": \"Explain how to use Docker in 3 steps\",
        \"stream\": false
      }' | grep content
      
      # Keep container running for demonstration (remove in production)
      echo '\\n--- Client finished ---'
    "
  networks:
    - ollama-network
```

This example demonstrates how to:
- Wait for Ollama to be healthy before making requests
- List available models
- Send prompts to the Ollama API
- Access Ollama by service name within the Docker network

#### Building Your Own Integration

To build your own application that uses Ollama:

```yaml
services:
  # Your Ollama setup...
  
  your-app:
    image: your-image
    environment:
      - OLLAMA_API_URL=http://ollama:11434
    networks:
      - ollama-network
    depends_on:
      ollama:
        condition: service_healthy
```

This ensures your application only starts when Ollama is ready and can access it via the internal Docker network using the service name `ollama`.

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
