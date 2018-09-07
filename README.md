### Scaling Deep Learning on Cori: Hands-on Activities
Welcome to [NERSC Data Day 2018](https://www.nersc.gov/users/training/data-day/data-day-2018/) hands-on session!  

This repo has multiple stacked branches to walk you through the code. The first
two stages will get you to run a single node test. In the 3rd stage we will train on multiple
nodes using [Horovod](https://github.com/uber/horovod).  

#### Instructions
1. **Login into Cori:**  
Ask us for a training login account if you don't have a your own NERSC account (not available for remote participants).  
```bash
ssh -A [USER NAME]@cori.nersc.gov
```

2. **Clone this repo**:  
```bash
git clone https://github.com/MustafaMustafa/data-day-2018-DL-Scaling.git
cd data-day-2018-DL-Scaling
```

3. **CNN model**:  
```bash
git checkout -b stage_1 origin/stage_1
```
Browse the model class `./models/cnn_model.py`. Then checkout the YAML configuration file (`./hparams/cnn.yaml`) for how to setup an experiment parameters.  

4. **Training using Estimator API**:  
```bash
git checkout -b stage_2 origin/stage_2
```
In this branch you will see a few utility files in addition to `train_demo.py`. Now we can test the code on one node.

5. **Single node training**:  
Allocate a batch node:
```bash
salloc --nodes 1 --time 00:30:00 -C knl --reservation scaleday
```
  
Load TensorFlow module:  
```bash
module load tensorflow/intel-1.9.0-py36
```
  
Run code:  
```bash
python3 train_demo.py hparams/cnn.yaml demo_single_node
```
6. **Plot learning curves:**
Now you can open and execute `./notebooks/plot-all-learning-curves.ipynb` in NERSC [Jupyter portal](http://jupyter-dev.nersc.gov).  

7. **Distributed training using Horovod**:
```bash
git checkout -b stage_3 origin/stage_3
```
This branch will add `train_horovod.py`. That is all you need to run in a distributed mode.  

8. **Multiple nodes training**:
Let us first try training on 2 nodes. Start by allocating the nodes:
```bash
salloc --nodes 2 --time 01:00:00 -C knl --reservation scaleday
```
In order to use regular nersc account instead of the training account, the 
command should be changed to,

```bash
salloc --nodes 2 --time 01:00:00 -C knl -q interactive
```

or 
```bash
sbatch *.sh --nodes 2 --time 01:00:00 -C knl -q regular
```

Load TensorFlow module:  
```bash
module load tensorflow/intel-1.9.0-py36
```
  
Run code:  
```bash
srun python3 train_horovod.py hparams/cnn.yaml demo_multi_node
```
Note that we need to use `srun` here to launch as many workers as there are nodes available.  

After you run on two node successfully you can try running on 4 nodes.

#### Code structure
- `./models`: contains models classes  
- `./hparams`: contains yparams wrapper and hyperparameter configuration files  
- `./data`: contains data preparation and pipeline class  
