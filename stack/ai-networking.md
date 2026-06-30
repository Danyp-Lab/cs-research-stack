# 🤖 AI & Networking Frameworks

> Domain-specific research in applied AI and network engineering requires specialized tools beyond the standard development stack. This page catalogs the frameworks and platforms that define modern research in these areas.

---

## 1. Deep Learning Frameworks

### PyTorch (Primary Recommendation)

[PyTorch](https://pytorch.org/) is the dominant framework in academic ML research. Its define-by-run (dynamic) computation graph is more intuitive for research than TensorFlow's static graph model.

```bash
# Install with CUDA support (always get exact command from pytorch.org)
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121
```

**Key PyTorch research libraries:**

| Library | Purpose |
| :--- | :--- |
| **[PyTorch Lightning](https://lightning.ai/)** | Training loop abstraction; removes boilerplate |
| **[Hugging Face Transformers](https://huggingface.co/docs/transformers)** | Pre-trained models for NLP, vision, audio |
| **[timm](https://github.com/huggingface/pytorch-image-models)** | 1000+ pre-trained vision models |
| **[TorchMetrics](https://lightning.ai/docs/torchmetrics/)** | Standardized evaluation metrics |
| **[einops](https://github.com/arogozhnikov/einops)** | Tensor manipulation with readable notation |
| **[torchinfo](https://github.com/TylerYep/torchinfo)** | Model summary and parameter counting |

### TensorFlow / JAX

| Framework | Best For |
| :--- | :--- |
| **TensorFlow 2** | Production deployment, TensorFlow Serving, TFLite |
| **JAX** | High-performance research needing XLA compilation, functional transforms |
| **Flax** | Neural networks in JAX (Google Brain style) |

---

## 2. Large Language Models

### Local Inference

Running LLMs locally preserves data privacy and eliminates API costs:

* **[Ollama](https://ollama.com/)** — The easiest way to run LLMs locally. Supports Llama, Mistral, Gemma, Phi, and more.

```bash
# Install Ollama
curl -fsSL https://ollama.com/install.sh | sh

# Pull and run a model
ollama pull llama3.2
ollama run llama3.2

# OpenAI-compatible API (localhost:11434)
curl http://localhost:11434/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{"model": "llama3.2", "messages": [{"role": "user", "content": "Hello"}]}'
```

* **[llama.cpp](https://github.com/ggerganov/llama.cpp)** — Efficient C++ inference for quantized LLMs. Runs 7B models on a MacBook; 70B models on a single consumer GPU.

```bash
git clone https://github.com/ggerganov/llama.cpp
cd llama.cpp && make -j $(nproc)
./llama-cli -m models/llama-3.2-7b-q4_K_M.gguf -p "Explain transformers:"
```

### High-Throughput Serving

* **[vLLM](https://github.com/vllm-project/vllm)** — PagedAttention-based serving for GPU clusters. Best throughput for multi-user research API deployments.

```bash
pip install vllm
python -m vllm.entrypoints.openai.api_server \
  --model meta-llama/Llama-3.1-8B-Instruct \
  --tensor-parallel-size 2
```

### Fine-Tuning and PEFT

| Tool | Purpose |
| :--- | :--- |
| **[PEFT](https://github.com/huggingface/peft)** | LoRA, QLoRA, prefix tuning, adapters |
| **[TRL](https://github.com/huggingface/trl)** | RLHF, DPO, SFT training |
| **[LLaMA-Factory](https://github.com/hiyouga/LLaMA-Factory)** | Unified fine-tuning for 100+ LLMs |
| **[Unsloth](https://github.com/unslothai/unsloth)** | 2× faster LoRA fine-tuning, 50% less VRAM |

---

## 3. Computer Vision

### Object Detection and Segmentation

* **[Ultralytics YOLO](https://github.com/ultralytics/ultralytics)** — State-of-the-art real-time detection, segmentation, pose estimation, and classification. The industry standard for applied CV research.

```python
from ultralytics import YOLO

model = YOLO("yolo11n.pt")           # Load nano model
results = model("image.jpg")         # Inference
model.train(data="coco8.yaml", epochs=100)  # Training
```

* **[Detectron2](https://github.com/facebookresearch/detectron2)** — Meta's research platform for object detection and segmentation. Reference implementation for Mask R-CNN, Panoptic FPN.
* **[MMDetection](https://github.com/open-mmlab/mmdetection)** — OpenMMLab's modular detection framework with 50+ architectures.

### Vision-Language Models

| Model | Organization | Best For |
| :--- | :--- | :--- |
| **[LLaVA](https://github.com/haotian-liu/LLaVA)** | UW Madison | Visual question answering, fine-tuning |
| **[CLIP](https://github.com/openai/CLIP)** | OpenAI | Zero-shot image classification, retrieval |
| **[Florence-2](https://huggingface.co/microsoft/Florence-2-large)** | Microsoft | Multi-task vision understanding |

---

## 4. Network Research Tools

### Traffic Analysis and Capture

```bash
# Capture traffic on an interface
tcpdump -i eth0 -w capture.pcap

# Analyze capture in Wireshark
wireshark capture.pcap

# Generate synthetic traffic for benchmarks
iperf3 -s                                    # Server
iperf3 -c server_ip -t 60 -P 4 -u -b 10G   # Client (UDP, 4 parallel streams)
```

### Network Emulation

* **[Mininet](http://mininet.org/)** — Software-defined network emulator. Create arbitrary network topologies for SDN and networking research.

```python
from mininet.net import Mininet
from mininet.topo import SingleSwitchTopo

net = Mininet(topo=SingleSwitchTopo(4))
net.start()
net.pingAll()
net.stop()
```

* **[ns-3](https://www.nsnam.org/)** — Discrete-event network simulator. The gold standard for academic network protocol simulation.
* **[GNS3](https://www.gns3.com/)** — Graphical network simulator integrating real OS images (Cisco IOS, Linux).

### Proxy and Tunnel Infrastructure

For research involving censorship, geo-restrictions, or university firewall bypass:

| Tool | Protocol | Use Case |
| :--- | :--- | :--- |
| **[frp](https://github.com/fatedier/frp)** | TCP/UDP multiplexing | Expose local dev server behind NAT/firewall |
| **[mihomo (MetaCubeX)](https://github.com/MetaCubeX/mihomo)** | Multi-protocol rule-based | Advanced routing with TUN mode, DNS hijacking |
| **[sing-box](https://github.com/SagerNet/sing-box)** | Universal proxy platform | Lightweight Go-based tunnel for custom networks |
| **[WireGuard](https://www.wireguard.com/)** | Modern VPN | Low-overhead research VPN between lab nodes |
| **[OpenWrt](https://openwrt.org/)** | Linux router firmware | Network engineering research at gateway level |

### frp Setup Example

```ini
# frps.ini (server, needs public IP)
[common]
bind_port = 7000
token = your_secret_token

# frpc.ini (client, your local machine)
[common]
server_addr = your.server.com
server_port = 7000
token = your_secret_token

[jupyter]
type = tcp
local_ip = 127.0.0.1
local_port = 8888
remote_port = 18888
```

---

## 5. MLOps and Model Deployment

### Experiment Tracking

See [research/experiment-tracking.md](../research/experiment-tracking.md) for a detailed comparison.

### Model Serving

| Tool | Scale | Best For |
| :--- | :--- | :--- |
| **[Gradio](https://www.gradio.app/)** | Prototype | Research demos, rapid UI for models |
| **[Streamlit](https://streamlit.io/)** | Prototype | Data dashboards, interactive demos |
| **[FastAPI](https://fastapi.tiangolo.com/)** | Production | High-performance REST API for model serving |
| **[BentoML](https://www.bentoml.com/)** | Production | ML model packaging and deployment |
| **[Triton Inference Server](https://github.com/triton-inference-server/server)** | GPU cluster | NVIDIA's high-throughput GPU serving |

---

## Further Reading

* [PyTorch Tutorials](https://pytorch.org/tutorials/) — Official comprehensive tutorials
* [Hugging Face Course](https://huggingface.co/learn/nlp-course/) — Free NLP course
* [Computer Networking: A Top-Down Approach](https://gaia.cs.umass.edu/kurose_ross/online_lectures.htm) — Kurose & Ross (free lectures)
