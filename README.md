# Snake Game — Q-Learning Agent

This repository contains an implementation of the classic **Snake** game along with several agents, including a **Q-learning agent** trained using reinforcement learning.  
The environment is implemented with **Pygame**, and training and evaluation are performed in Python.

---

## Overview

Three different agents are implemented:

- **Random Agent**  
  Chooses actions uniformly at random.

- **Logic Agent**  
  Uses a hand-crafted heuristic policy encoded as a fixed Q-table.  
  The table is not updated during gameplay.

- **Q-Learning Agent**  
  Learns a Q-table during training using the Bellman update rule and reward shaping.

All agents interact with the same environment and share the same state representation and action space.

---

## Environment

- Grid-based Snake game
- The snake dies when it hits the wall or itself
- Food spawns at random free cells
- Score increases by 1 for each eaten food

The main gameplay loop is implemented in the `start_games(...)` function, which:
1. Initializes the snake and food
2. Runs the episode until the snake dies
3. Updates statistics
4. Optionally updates the Q-table during training

---

## Q-Learning details

### Reward function

During experimentation, it was found that rewarding the agent **only for eating food** led to slow and unstable learning.  
To fix this, **reward shaping** was applied:

- `+10` — eating food  
- `+1` — moving closer to the food  
- `-1` — moving away from the food  
- `-100` — death (collision with wall or body)

This significantly stabilized training and improved convergence speed.

### Training setup

- Tabular Q-learning
- Reduced exploration rate compared to baseline implementations
- Lower discount factor due to local observability
- Fixed random seed (`seed = 42`) for reproducibility

---

## Results

### Quantitative results

| Agent               | Average score | Games played | Learning | Description |
|---------------------|--------------:|-------------:|:--------:|------------|
| **Q-Learning Agent** | **58**        | 600          | Yes      | Learned Q-table with reward shaping |
| Logic Agent         | 57             | 100          | No       | Fixed heuristic policy |
| Random Agent        | ~0             | 100          | No       | Random actions baseline |

### Observations

- The **Q-learning agent slightly outperforms** the hand-crafted logic agent.
- Comparable tutorials report an average score of ~44 after ~7,500 games, while this implementation reaches ~58 after only **600 games**.
- Reward shaping and a stronger death penalty were crucial for fast and stable learning.

---

## Reproducibility

- Fixed random seed (`42`)
- Deterministic environment dynamics
- All experiments can be reproduced by running the provided notebook

---

## References

- Q-learning Snake tutorial:
https://8thlight.com/insights/qlearning-teaching-ai-to-play-snake

- Video tutorial referenced during development:
https://www.youtube.com/watch?v=je0DdS0oIZk

