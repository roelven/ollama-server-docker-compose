# Ollama Docker Project Guidelines

## Description
This simple project should enable the user to run a contained Ollama server that can be instructed to pull and run LLM models, and it should expose the Ollama API endpoint so that other (dockerized) applications can query it. For example, an app I run on my server is called Linkwarden, and it expexts to be able to call Ollama like this:

```
NEXT_PUBLIC_OLLAMA_ENDPOINT_URL=http://localhost:11434
OLLAMA_MODEL=phi3:mini-4k
```


## Commands
- Start: `docker-compose up -d`
- Stop: `docker-compose down`
- View logs: `docker-compose logs -f`
- Run models: `docker exec -it ollama ollama run <model-name>`
- Pull models: `docker exec -it ollama ollama pull <model-name>`
- Shell access: `docker exec -it ollama bash`

## Code Style & Guidelines
- **Docker Compose**: Use YAML indentation with 2 spaces
- **Comments**: Document environment variables and GPU settings 
- **Naming**: Use descriptive service and volume names
- **Error Handling**: Check container health with the provided healthcheck
- **Container Access**: Use `docker exec` pattern for commands
- **Resource Management**: Configure optional GPU passthrough using deploy resources
- **Environment**: Make sure the Ollama server is kept alive and restarts automatically
- **Security**: Avoid exposing server beyond localhost unless necessary
- **Volume Management**: Use relative paths for volume mounts

## Project Structure
- Keep Docker Compose configuration minimal
- Document usage examples in README.md, including how to stop and clean up. 
- Validate configuration changes with `docker-compose config`
- Add license information to indicate this is open source for everyone to copy and use. 
- No feature creep, keep things simple. 