# MAPF-GPT: Imitation Learning for Multi-Agent Pathfinding at Scale



<div align="center" dir="auto">
   <p dir="auto"><img src="svg/puzzles.svg" alt="Follower" style="max-width: 80%;"></p>


---

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://github.com/Cognitive-AI-Systems/MAPF-GPT/blob/main/LICENSE)
[![arXiv](https://img.shields.io/badge/arXiv-2310.01207-b31b1b.svg)](https://arxiv.org/abs/2409.00134)
[![Hugging Face](https://img.shields.io/badge/Weights-MAPF--GPT-blue?logo=huggingface)](https://huggingface.co/aandreychuk/MAPF-GPT/tree/main)
[![Hugging Face](https://img.shields.io/badge/Dataset-MAPF--GPT-blue?logo=huggingface)](https://huggingface.co/datasets/aandreychuk/MAPF-GPT)
</div>

The repository consists of the following crucial parts:

- `example.py` - an example of code to run the MAPF-GPT approach.
- `benchmark.py` - a script that launches the evaluation of the MAPF-GPT model on the POGEMA benchmark set of maps.
- `generate_dataset.py` - a script that generates a 1B training dataset. The details are provided inside the script in the main() function.
- `train.py` - a script that launches the training of the MAPF-GPT model.
- `eval_configs` - a folder that contains configs from the POGEMA benchmark. Required by the `benchmark.py` script.
- `dataset_configs` - a folder that contains configs to generate training and validation datasets. Required by the `generate_dataset.py` script.

## Installation

It's recommended to utilize Docker to build the environment compatible with MAPF-GPT code. The `docker` folder contains both `Dockerfile` and `requirements.txt` files to successfully build an appropriate container.

```
cd docker & sh build.sh
```

## Running an example

To test the work of MAPF-GPT, you can simply run the `example.py` script. You can adjust the parameters, such as the number of agents or the name of the map. You can also switch between MAPF-GPT-2M, MAPF-GPT-6M and MAPF-GPT-85M models.

```
python3 example.py
```

Besides statistics about SoC, success rate, etc., you will also get an SVG file that animates the solution found by MAPF-GPT (`out.svg`).

## Running evaluation

You can run the `benchmark.py` script, which will run both MAPF-GPT-2M and MAPF-GPT-6M models on all the scenarios from the POGEMA benchmark.
You can also run the MAPF-GPT-85M model by setting `path_to_weights` to `weights/model-85M.pt`. The weights for all models will be downloaded automatically.

```
python3 benchmark.py
```

The results will be stored in the `eval_configs` folder near the corresponding configs. They can also be logged into wandb. The tables with average success rates will be displayed directly in the console.

## Generating dataset

Due to the very large size of the dataset, we are not able to upload it to the repository. However, we provide a script that can complete all the steps required to generate the dataset, including the instance generation process (via POGEMA), solving the instances via LaCAM, generating and filtering observations, shuffling the data, and saving the dataset into multiple `.arrow` files for further efficient in-memory operation.

```
python3 generate_dataset.py
```

Please note that the full training dataset for 1B observation-gt_action pairs requires 256 GB of disk space and additionally around 20 GB for temporary files. It also requires a lot of time to solve all instances via LaCAM. By modifying the config files in `dataset_configs` (adjusting the number of seeds, reducing the number of maps), you can reduce the time and space required to generate the dataset (as well as its final size).

## Running training of MAPF-GPT

To train MAPF-GPT from scratch or to fine-tune the existing models on other datasets (if you occasionally have such ones), you can use the `train.py` script. By providing it a config, you can adjust the parameters of the model and training setup. The script utilizes DDP, which allows training the model on multiple GPUs simultaneously. By adjusting the `nproc_per_node` value, you can choose the number of GPUs that are used for training.

```
torchrun --standalone --nproc_per_node=1 train.py gpt/config-6M.py
```

## Citation:

```bibtex
@article{andreychuk2024mapf-gpt,
  title={MAPF-GPT: Imitation Learning for Multi-Agent Pathfinding at Scale},
  author={Anton Andreychuk and Konstantin Yakovlev and Aleksandr Panov and Alexey Skrynnik},
  journal={arXiv preprint arXiv:2409.00134},
  year={2024},
  url={https://arxiv.org/abs/2409.00134}
}
```
