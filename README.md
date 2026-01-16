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

All statistics below are computed over **100 evaluation games per agent**.

### 1. Score statistics

| Metric | Random Agent | Logic Agent | Q-Learning Agent |
|------|-------------:|------------:|-----------------:|
| Min | 0.0 | 12.0 | 14.0 |
| Max | 1.0 | 122.0 | 116.0 |
| **Mean** | **0.0** | **57.8** | **58.8** |
| Mode | 0.0 | 31.0 | 49.0 |
| Median | 0.0 | 54.0 | 55.5 |
| Std | 0.2 | 26.0 | 24.4 |
| Games | 100 | 100 | 100 |

---

### 2. Steps until death

| Metric | Random Agent | Logic Agent | Q-Learning Agent |
|------|-------------:|------------:|-----------------:|
| Min | 44.0 | 409.0 | 541.0 |
| Max | 1000.0 | 4488.0 | 5638.0 |
| **Mean** | **309.0** | **2163.7** | **2348.1** |
| Mode | 128.0 | 4158.0 | 1488.0 |
| Median | 252.0 | 1975.5 | 2227.0 |
| Std | 211.2 | 1088.8 | 1137.5 |
| Games | 100 | 100 | 100 |

---

### 3. Steps to get first food

| Metric | Random Agent | Logic Agent | Q-Learning Agent |
|------|-------------:|------------:|-----------------:|
| Min | 79.0 | 2.0 | 2.0 |
| Max | 514.0 | 83.0 | 48.0 |
| **Mean** | **273.0** | **29.2** | **25.3** |
| Mode | 79.0 | 21.0 | 23.0 |
| Median | 249.5 | 27.0 | 24.0 |
| Std | 180.2 | 16.9 | 11.0 |
| Games | 4 | 100 | 100 |

> Note: the random agent rarely reaches food, so statistics are based on only 4 successful games.

---

### 4. Average number of steps to get food

| Metric | Random Agent | Logic Agent | Q-Learning Agent |
|------|-------------:|------------:|-----------------:|
| Min | 79.0 | 27.6 | 29.1 |
| Max | 514.0 | 44.0 | 50.3 |
| **Mean** | **273.0** | **36.1** | **38.4** |
| Mode | 79.0 | 35.5 | 35.8 |
| Median | 249.5 | 35.8 | 38.1 |
| Std | 180.2 | 3.3 | 3.8 |
| Games | 4 | 100 | 100 |

---

## Key observations

- The **Q-learning agent achieves the highest average score** and survives longer than the logic-based agent.
- Both intelligent agents dramatically outperform the random baseline across all metrics.
- The Q-learning agent reaches the first food faster on average, indicating more efficient early exploration.
- Higher variance in steps-to-death reflects more aggressive exploration behavior compared to the logic agent.

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

