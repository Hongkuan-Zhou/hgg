# Exploration via Hindsight Goal Generation

This is the TensorFlow implementation for the Bachelor's thesis "Goal-Based Hindsight Goal Generation for Robotic Object Manipulation with Sparse-Reward Deep Reinforcement Learning" (Matthias Brucker, 2020). The PDF of this thesis as well as a video demonstrating our experimental results can be found in this repository.
It is based on the implementation of the HGG paper [Exploration via Hindsight Goal Generation](http://arxiv.org/abs/1906.04279) accepted by NeurIPS 2019.



## Requirements
1. Ubuntu 16.04 (newer versions such as 18.04 should work as well)
2. Python 3.5.2 (newer versions such as 3.6.8 should work as well)
3. MuJoCo == 2.00 (see instructions on https://github.com/openai/mujoco-py)
4. Install gym from https://github.com/mbrucker07/gym.git. Certain environment specifications and parameters are set there. 
```bash
git clone https://github.com/mbrucker07/gym.git
cd gym
pip install -e . 
```
5. Clone HGG-Extended form https://github.com/mbrucker07/HGG-Extended.git.
```bash
git clone https://github.com/mbrucker07/HGG-Extended.git
cd HGG-Extended
```
6. Install requirements with pip install -r requirements.txt
```bash
pip install -r requirements.txt
```

## Running commands from G-HGG

Run the following commands to reproduce results on FetchPushLabyrinth, FetchPickObstacle, FetchPickNoObstacle, and FetchPickAndThrow environments.

Training Results:

```bash
# FetchPushLabyrinth
# HER (with EBP)
python train.py --tag 000 --learn normal --env FetchPushLabyrinth-v1 --goal custom 
# HGG (with HER, EBP and STOP condition)
python train.py --tag 010 --learn hgg --env FetchPushLabyrinth-v1 --goal custom --stop_hgg_threshold 0.3
# G-HGG (with HER, EBP and STOP condition)
python train.py --tag 020 --learn hgg --env FetchPushLabyrinth-v1 --goal custom --graph True --n_x 31 --n_y 31 --n_z 11 --stop_hgg_threshold 0.3 
# HER+GoalGAN
python train.py --tag 030 --learn her+goalGAN --env FetchPushLabyrinth-v1 --goal custom
python train.py --tag 040 --learn normal+goalGAN --env FetchPushLabyrinth-v1 --goal custom


# FetchPickObstacle
python train.py --tag 100 --learn normal --env FetchPickObstacle-v1 --goal custom 
python train.py --tag 110 --learn hgg --env FetchPickObstacle-v1 --goal custom --stop_hgg_threshold 0.3
python train.py --tag 120 --learn hgg --env FetchPickObstacle-v1 --goal custom --graph True --n_x 31 --n_y 31 --n_z 11 --stop_hgg_threshold 0.3
python train.py --tag 140 --learn normal+goalGAN --env FetchPickObstacle-v1 --goal custom
# hgg + route
python train.py --tag 111 --learn hgg --env FetchPickObstacle-v1 --goal custom --stop_hgg_threshold 0.5 --route True 

# FetchPickNoObstacle
python train.py --tag 200 --learn normal --env FetchPickNoObstacle-v1 --goal custom 
python train.py --tag 210 --learn hgg --env FetchPickNoObstacle-v1 --goal custom --stop_hgg_threshold 0.3
python train.py --tag 220 --learn hgg --env FetchPickNoObstacle-v1 --goal custom --graph True --n_x 31 --n_y 31 --n_z 11 --stop_hgg_threshold 0.3
python train.py --tag 240 --learn normal+goalGAN --env FetchPickNoObstacle-v1 --goal custom

# FetchPickAndThrow
python train.py --tag 300 --learn normal --env FetchPickAndThrow-v1 --goal custom 
python train.py --tag 310 --learn hgg --env FetchPickAndThrow-v1 --goal custom --stop_hgg_threshold 0.9
python train.py --tag 320 --learn hgg --env FetchPickAndThrow-v1 --goal custom --graph True --n_x 51 --n_y 51 --n_z 7 --stop_hgg_threshold 0.9
python train.py --tag 340 --learn normal+goalGAN --env FetchPickAndThrow-v1 --goal custom


# KukaReach
python train.py --tag 400 --learn normal --env KukaReach-v1 
python train.py --tag 410 --learn hgg --env KukaReach-v1 --stop_hgg_threshold 0.3
python train.py --tag 420 --learn hgg --env KukaReach-v1 --graph True --n_x 51 --n_y 51 --n_z 7 --stop_hgg_threshold 0.9

# KukaPickAndPlaceObstacle
python train.py --tag 510 --learn hgg --env KukaPickAndPlaceObstacle-v1 --stop_hgg_threshold 0.3
python train.py --tag 520 --learn hgg --env KukaPickAndPlaceObstacle-v1 --graph True --n_x 51 --n_y 51 --n_z 15 --stop_hgg_threshold 0.9
# KukaPickNoObstacle
python train.py --tag 610 --learn hgg --env KukaPickNoObstacle-v1 --stop_hgg_threshold 0.3
python train.py --tag 620 --learn hgg --env KukaPickNoObstacle-v1 --graph True --n_x 51 --n_y 51 --n_z 15 --stop_hgg_threshold 0.9

# KukaPickThrow
python train.py --tag 710 --learn hgg --env KukaPickThrow-v1 --stop_hgg_threshold 0.3 --epoch 30
python train.py --tag 720 --learn hgg --env KukaPickThrow-v1 --graph True --n_x 51 --n_y 51 --n_z 7 --stop_hgg_threshold 0.9 --epoch 30

# KukaPushLabyrinth
python train.py --tag 820 --learn hgg --env KukaPushLabyrinth-v1 --graph True --n_x 51 --n_y 51 --n_z 7 --stop_hgg_threshold 0.9

# KukaPushSlide
python train.py --tag 910 --learn hgg --env KukaPushSlide-v1 --stop_hgg_threshold 0.3 --epoch 20

```
## Plotting

To plot the agent's performance on multiple training runs, copy all training run directories into one directory. For example, we put all FetchPushLabyrinth runs in a directory called BA_Labyrinth, same for FetchPickObstacle (BA_Obstacle), FetchPickNoObstacle (BA_NoObstacle) and FetchPickAndThrow (BA_Throw). naming=0 is recommended as default. For our result plot commands, have a look at create_result_figures.sh. 

```bash
# Scheme: python plot.py log_dir env_id --naming <naming_code> --e_per_c <episodes per cycle>
python plot.py figures/BA_Labyrinth FetchPushLabyrinth-v1 --naming 0 --e_per_c 20
```

## Figures

Figures and the data they are based on can be found in the directory "figures" and were generate with the following scripts:

```bash
# Result and Ablation plots (Figures are already generated in the respective subdirectories in directory "figures"):
./create_result_figures.sh

# Other plots (Figures are already generated in directory "figures"):
python create_figures.py
```

## Playing 

To look at the agent solving the respective task according to his learned policy, issue the following command:

```bash
# Scheme: python play.py --env env_id --goal custom --play_path log_dir --play_epoch <epoch number, latest or best>


# KukaReach
python play.py --env KukaReach-v1 --play_path log/400-ddpg-KukaReach-v1-normal --play_epoch best

# KukaPickAndPlaceObstacle
python play.py --env KukaPickAndPlaceObstacle-v1 --play_path log/520-ddpg-KukaPickAndPlaceObstacle-v1-hgg-graph-stop --play_epoch best
python play.py --env KukaPickAndPlaceObstacle-v1 --play_path log/510-ddpg-KukaPickAndPlaceObstacle-v1-hgg-stop --play_epoch best

# KukaPickNoObstacle
python play.py --env KukaPickNoObstacle-v1 --play_path log/610-ddpg-KukaPickNoObstacle-v1-hgg-stop --play_epoch best
python play.py --env KukaPickNoObstacle-v1 --play_path log/620-ddpg-KukaPickNoObstacle-v1-hgg-graph-stop --play_epoch best

# KukaPickThrow
python play.py --env KukaPickThrow-v1 --play_path log/710-ddpg-KukaPickThrow-v1-hgg-stop --play_epoch best
python play.py --env KukaPickThrow-v1 --play_path log/720-ddpg-KukaPickThrow-v1-hgg-graph-stop --play_epoch best

# KukaPushLabyrinth
python play.py --env KukaPushLabyrinth-v1 --play_path log/810-ddpg-KukaPushLabyrinth-v1-hgg-stop --play_epoch best
python play.py --env KukaPushLabyrinth-v1 --play_path log/820-ddpg-KukaPushLabyrinth-v1-hgg-graph-stop --play_epoch best

# KukaPushSlide
python play.py --env KukaPushSlide-v1 --play_path log/910-ddpg-KukaPushSlide-v1-hgg-stop --play_epoch best

# FetchPushLabyrinth
# G-HGG
python play.py --env FetchPushLabyrinth-v1 --goal custom --play_path figures/BA_Labyrinth/000-ddpg-FetchPushLabyrinth-v1-hgg-mesh-stop --play_epoch best
# HGG
python play.py --env FetchPushLabyrinth-v1 --goal custom --play_path figures/BA_Labyrinth/010-ddpg-FetchPushLabyrinth-v1-hgg-stop --play_epoch best
# HER
python play.py --env FetchPushLabyrinth-v1 --goal custom --play_path figures/BA_Labyrinth/010-ddpg-FetchPushLabyrinth-v1-normal --play_epoch best

#FetchPickObstacle
python play.py --env FetchPickObstacle-v1 --goal custom --play_path figures/BA_Obstacle/100-ddpg-FetchPickObstacle-v1-hgg-mesh-stop --play_epoch best
python play.py --env FetchPickObstacle-v1 --goal custom --play_path figures/BA_Obstacle/112-ddpg-FetchPickObstacle-v1-hgg-stop --play_epoch best
python play.py --env FetchPickObstacle-v1 --goal custom --play_path figures/BA_Obstacle/120-ddpg-FetchPickObstacle-v1-normal --play_epoch best

#FetchPickNoObstacle
python play.py --env FetchPickNoObstacle-v1 --goal custom --play_path figures/BA_NoObstacle/200-ddpg-FetchPickNoObstacle-v1-hgg-mesh-stop --play_epoch best
python play.py --env FetchPickNoObstacle-v1 --goal custom --play_path figures/BA_NoObstacle/210-ddpg-FetchPickNoObstacle-v1-hgg-stop --play_epoch best
python play.py --env FetchPickNoObstacle-v1 --goal custom --play_path figures/BA_NoObstacle/220-ddpg-FetchPickNoObstacle-v1-normal --play_epoch best

#FetchPickAndThrow
python play.py --env FetchPickAndThrow-v1 --goal custom --play_path figures/BA_Throw/300a-ddpg-FetchPickAndThrow-v1-hgg-mesh-stop --play_epoch best
python play.py --env FetchPickAndThrow-v1 --goal custom --play_path figures/BA_Throw/310a-ddpg-FetchPickAndThrow-v1-hgg-stop --play_epoch best
python play.py --env FetchPickAndThrow-v1 --goal custom --play_path figures/BA_Throw/320a-ddpg-FetchPickAndThrow-v1-hgg-normal --play_epoch best
```

## Running commands from HGG paper

Run the following commands to reproduce our main results shown in section 5.1 of the HGG paper.

```bash
python train.py --tag='HGG_fetch_push' --env=FetchPush-v1
python train.py --tag='HGG_fetch_pick' --env=FetchPickAndPlace-v1
python train.py --tag='HGG_hand_block' --env=HandManipulateBlock-v0
python train.py --tag='HGG_hand_egg' --env=HandManipulateEgg-v0
```
