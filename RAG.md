```markdown
# 环境配置与模型下载指南

Linux 环境（ NVIDIA GeForce RTX 4090 D）下搭建 RAG 基础运行环境及通过国内镜像稳定下载模型的标准流程。

---

## 1. 创建并激活 Conda 环境

推荐使用 Python 3.10 构建独立虚拟环境：

```bash
conda create -n rag python=3.10 -y
conda activate rag

```

---

## 2. 安装 PyTorch (CUDA 12.1)

安装支持 GPU 加速的 PyTorch 运行时：

```bash
pip install torch torchvision --index-url [https://download.pytorch.org/whl/cu121](https://download.pytorch.org/whl/cu121)

```

---

## 3. 安装 RAG 与大模型核心依赖

包含文档解析、向量数据库、Embedding 处理及模型加载工具链：

```bash
# 安装 PDF 解析与轻量向量检索库
pip install pymupdf sentence-transformers chromadb tiktoken

# 升级 Hugging Face 核心工具链
pip install -U transformers accelerate huggingface_hub

```

---

## 4. 模型权重下载 (国内镜像稳定通道)

针对国内网络环境，配置镜像源并禁用容易引发网络断流和 `401 Unauthorized` 认证报错的新版底层传输协议（`hf_transfer` 与 `XetHub CAS`）。

```bash
# 1. 设置国内 HF 镜像端点
export HF_ENDPOINT=[https://hf-mirror.com](https://hf-mirror.com)

# 2. 禁用易导致连接中断的 hf_transfer 高并发加速器
export HF_HUB_ENABLE_HF_TRANSFER=0

# 3. 禁用 XetHub CAS 分块去重协议（避免绕过镜像直连海外节点触发 401 报错）
export HF_HUB_DISABLE_XET=1

# 4. 下载 Embedding 模型
python -m huggingface_hub.cli.hf download Qwen/Qwen3-Embedding-4B --local-dir Qwen3-Embedding-4B

# 5. 下载 Reranker 模型
python -m huggingface_hub.cli.hf download Qwen/Qwen3-Reranker-4B --local-dir Qwen3-Reranker-4B

```

