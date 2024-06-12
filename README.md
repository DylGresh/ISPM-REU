# ISPM-REU

A collection of relevant files for local LLM deployment.

## local_llm_deployment.md

Markdown note file describing the typical process for deploying a LLM locally.

## cpu_only.ipynb & cpu_only.py

A Jupyter notebook detailing the setup and running of inference using the Llama 3 8b model from Meta.

The `Requirements` section in this notebook is probably the most detailed one.

`cpu_only.py` is the same code as `cpu_only.ipynb` just in a Python script format instead of a Jupyter Notebook.

## conversations.ipynb

A Jupyter notebook detailing the setup and running of conversational inference using the Llama 3 8b model from Meta.

Very similar to `cpu_only` except the previous chats are kept track of and passed to the model at each inference generation step.

## cpu_vs_gpu.ipynb

An incomplete Jupyter notebook where I attempted to run inference on both the GPU and CPU to compare runtimes. I was able to eventually finish the file on a different device and just forgot to push the changes. I'll do so ASAP.

## llagua2-llama3.ipynb

A Jupyter notebook detailing a simple sample process for using Llama Guard 2 as a companion model for another model, in this case Llama 3 8b from Meta.

## A Note on Purple Llama

Llama Guard and Llama Guard 2 are fine-tuned models (often referred to as derived or children models) of Llama 2 7b and Llama 3 8b respectively.

CodeShield is a Python script that aims to analyze code (only a [small subset of all programming languages](https://github.com/meta-llama/PurpleLlama/blob/main/CodeShield/insecure_code_detector/README.md#languages-supported) are supported as of now) that's either generated by or passed into a LLM for vulnerabilities and potentially insecure portions of code.

- Both Llama Guard/Llama Guard 2 and CodeShield are NOT meant to be used on their own. They're meant to be used as an additional layer on top of whatever other security practices are implemented in your environment.

## A Note on LLMs in General

You'll find that in the code examples provided, I only used `llama-cpp-python` as the library for inference. This is due to its ability to run on either the CPU or GPU or on both. This allows running LLMs locally on virtually any machine no matter if they have a GPU or not.

Of course, it's very much possible to use other libraries, most others simply didn't perform as well as `llama-cpp-python` did in my testing or I was unable to get them properly setup to run with quantized models.

### Runtime

CPU only inference time will always be longer than GPU inference time. This is due to GPU's inherent capability to perform matrix/tensor computations at a faster rate than a CPU.

GPU inference time (and also CPU inference time) is heavily dependent on the amount of VRAM (GPUs) and RAM (CPUs, optionally alongside GPUs) that the host machine has available.

When inference is being done, the model parameters are loaded into the RAM/VRAM spaces while the input is cached. The cache is then taken up (for the remainder of the inference time) with the previous/current layers' outputs and the remaining parameters until there are no more parameters left. The last step is un-tokenizing the output into human-readable text for output.

Although it may not initially be clear, the primary limiting factor for inference time is the speed of data transfer between Disk -> RAM/VRAM -> cache while the secondary limiting factor is the actual speed of the CPU/GPU that is running the computations.

- More often than not, the slowdown is due to the CPU/GPU computing faster than the data can be loaded into the relevant memory.
