# Tiny Agent

> - Usage of Docker Model Runner on Docker CE
> - Deployment of an ADK application to a RPI 5
> - Constraint: "small machine", then SLM ... TLM (Tiny Language Model)

## Model

For this demo I packaged a model from Hugging Face
> https://huggingface.co/Qwen/Qwen2.5-0.5B-Instruct-GGUF

```bash
#!/bin/bash
HF_MODEL=qwen2.5-0.5b-instruct-q5_0.gguf
LICENSE=LICENSE
HANDLE=k33g
DMR_MODEL=qwen2.5:0.5b-instruct-q5_0

docker model package \
--gguf $(pwd)/${HF_MODEL} \
--license $(pwd)/${LICENSE} \
--push ${HANDLE}/${DMR_MODEL}

docker model pull ${HANDLE}/${DMR_MODEL}
```

## Remote deployment with Docker Compose

### `.env`

```bash
DMR_BASE_URL=http://172.17.0.1:12434
MODEL_RUNNER_CHAT_MODEL=k33g/qwen2.5:0.5b-instruct-q5_0
```

### Check and select the Docker context

```bash
docker context ls
docker context use robby-rpi-remote
```

### Deploy

```bash
docker context ls
docker context use robby-rpi-remote
cd agents
docker compose -f compose.remote.yml up --build
```
> in case of connection issue, try: `ssh-copy-id k33g@robby.local`

Go to: http://robby.local:6060/

> - Back to the default context: `docker context use desktop-linux` 
