# BrazenLab SDK

Fine-tune models on AWS Trainium, in your own AWS account.

```bash
pip install https://github.com/Brazen-Lab/sdk/releases/latest/download/brazenlab_sdk-0.9.3-py3-none-any.whl

brazenlab login --token <your-token>
brazenlab connect
brazenlab train --model allenai/OLMoE-1B-7B-0924 --output s3://your-bucket/ckpts
```

Instances run in **your** account with **your** credentials, and are billed to you.
BrazenLab never receives your keys, model weights, or data.

## What it does for you

- **Runs models Neuron rejects.** Wide mixture-of-experts models fail on the stock path —
  the router hits an unsupported integer type, expert dispatch reads out of bounds on the
  device, and the parameter count exceeds what Neuron accepts. The SDK handles all three.
- **Picks the machine and the parallelism.** A model whose experts cannot fit on one
  NeuronCore is automatically split across cores on a larger instance.
- **Compiles inside a Nitro Enclave.** BrazenLab's compiler never lands on your training
  machine. This is the default; there is no flag to turn it on.

## Commands

| command | what it does |
|---|---|
| `brazenlab login` | cache your token (`--token` for CI) |
| `brazenlab connect` | verify AWS access and show the account |
| `brazenlab train` | launch a fine-tune and stream progress |
| `brazenlab run job.yaml` | same, from a job file |
| `brazenlab profile` | hardware trace proving the custom kernel executed |

## Requirements

Python 3.9+, AWS credentials with EC2/SSM/S3 permissions, and Trainium capacity in your
region.

This package contains the client only. The training runtime is fetched at launch against
your token and runs on the instances in your account.
