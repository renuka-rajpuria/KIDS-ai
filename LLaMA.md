### 1. Download the LLaMA weights

Reference: [shawwn/llama-dl](https://github.com/shawwn/llama-dl) 

To download all model weights, cd into the directory you want them, then run this:
```
curl -o- https://raw.githubusercontent.com/shawwn/llama-dl/56f50b96072f42fb2520b1ad5a1d6ef30351f23c/llama.sh | bash
```
Extra: use ```free -g``` and ```nvidia-smi``` on NVIDIA system for memory related checks.

### 2. Run the model

Reference: [How to Run Your Own LLaMA by Dr. Mandar Karhade](https://levelup.gitconnected.com/how-to-run-your-own-llama-550cd69b1bc9)

#### 2.1 Set up Conda and create an environment for LLaMA (as recommended by Meta)

- Open a terminal and run: ```wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh```
- Run ```chmod +x Miniconda3-latest-Linux-x86_64.sh```
- Run ```./Miniconda3-latest-Linux-x86_64.sh```
- Go with the default options. When it shows you the license, hit q to continue the installation.
- Refresh your shell by logging out and logging in back again.
    
#### 2.2 Create env and install dependencies

- Create an env: ```conda create -n llama```
- Activate the env: ```conda activate llama```
- Install the dependencies (NVIDIA):```conda install torchvision torchaudio pytorch-cuda=11.7 git -c pytorch -c nvidia```
- Clone the INT8 repo by the user tloen: ```git clone https://github.com/tloen/llama-int8 && cd llama-int8```
- Install the requirements: ```pip install -r requirements.txt pip install -e .```

#### 2.3 Create a swapfile

- Create a swapfile: ```sudo dd if=/dev/zero of=/swapfile bs=4M count=13000 status=progress``` This will create about ~50GB swapfile. Edit the count to your preference. 13000 means 4MBx13000.
- Mark it as swap: ```sudo mkswap /swapfile```
- Activate it: ```sudo swapon /swapfile```

If you want to delete it, simply run ```sudo swapoff /swapfile``` and then ```rm /swapfile```

#### 2.3 Create a swapfile

- Open a terminal in your llama-int8 folder (the one you cloned).
- Run: ```python example.py --ckpt_dir ~/Downloads/LLaMA/7B --tokenizer_path ~/Downloads/LLaMA/tokenizer.model --max_batch_size=1```

You’re done. Wait for the model to finish loading and it’ll generate a prompt.

### 3. Add custom prompts

By default, the llama-int8 repo has a short prompt baked into example.py.

- Open the ```example.py``` file in the llama-int8 directory.
- Navigate to line 136. It starts with triple quotations, """.
- Replace the current prompt with whatever you have in mind.

