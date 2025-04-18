# Battery Arbitrage with Deep Reinforcement Learning and Time-Series Forecasting

This repository contains code and data for the paper "_Enhancing Battery Storage Energy Arbitrage with Deep Reinforcement Learning and Time-Series Forecasting_" by Manuel Sage, Joshua Campbell, Yaoyao Fiona Zhao submitted to ASME ES2024 (currently under review).

## Project Overview

This project explores using Deep Reinforcement Learning (DRL) combined with time-series forecasting to optimize battery energy storage system operation for energy arbitrage in electricity markets. The implementation includes:

1. DRL agents (DQN, PPO) for battery charge/discharge control
2. Battery energy storage system models with degradation considerations
3. Time-series forecasting models for electricity price prediction
4. Simulation environments for testing different strategies

## Repository Structure

- **main/**: Contains entry point scripts for running experiments
  - `run_dqn.py`: Runs experiments using the DQN algorithm
  - `run_ppo.py`: Runs experiments using the PPO algorithm
  
- **train/**: Contains training modules
  - `train.py`: Implements the core training loop for RL agents
  
- **envs/**: Contains environment implementations
  - `base_envs.py`: Base environment classes
  - `environments.py`: Specific environment implementations (e.g., FreeBatteryEnv)
  - `storage_model.py`: Battery storage models with and without degradation
  - `grid_model.py`: Grid interaction models
  - `env_params.py`: Environment parameter configurations
  
- **forecasters/**: Time-series forecasting models
  - `models.py`: Neural network architectures for time-series forecasting (CNN, LSTM, Hybrid)
  - `trained_models/`: Saved pre-trained forecasting models for different time horizons
  
- **utils/**: Utility functions and helpers
  - `callbacks.py`: Training callbacks
  - `make_env.py`: Environment creation utilities
  - `net_design.py`: Network architecture definitions
  - `utilities.py`: General utility functions
  - `wrappers.py`: Environment wrappers for pre-processing
  
- **data/**: Contains datasets
  - `alberta3/`: Alberta electricity market data (2018-2022)
    - `alberta_2022_electricity_final.csv`: Final dataset for 2022
    - `ab_2018-2022_electricity_time_climate.csv`: Combined dataset for 2018-2022

## Input Data

The models use electricity market price data from the Alberta electricity market, with:
- Hourly resolution time-series
- Price data for multiple years (2018-2022)
- Additional temporal and climate features for forecasting

## Key Components

### Battery Models

Two battery models are implemented:
1. `BESS`: Basic battery model without degradation
2. `DODDegradingBESS`: Battery model with depth-of-discharge degradation

The models track:
- State of Charge (SoC)
- Energy flows
- Degradation costs

### Reinforcement Learning Agents

The project implements two DRL algorithms:
1. **DQN** (Deep Q-Network): Uses discrete actions (-1, 0, 1) for battery control
2. **PPO** (Proximal Policy Optimization): Uses continuous actions for finer control

### Forecasting Models

Multiple neural network architectures for time-series forecasting:
1. **LSTM**: Long Short-Term Memory networks
2. **CNN**: Convolutional Neural Networks
3. **Hybrid**: CNN-LSTM hybrid models
4. **AttentionHybrid**: CNN-LSTM with attention mechanisms

Pre-trained models are available for different forecast horizons (1, 2, 3, 6, 8, 12, 18, 24 hours).

## Running Experiments

To run experiments with the DQN agent:
```
python main/run_dqn.py
```

To run experiments with the PPO agent:
```
python main/run_ppo.py
```

Both scripts will prompt for a save folder name. Results are saved in a `log` directory.

## Using Pre-trained Forecasters

The repository includes pre-trained forecasting models for different time horizons. These can be activated in the experimental parameters:

```python
'forecasts': {'log_folder_paths': [
    os.path.join(src_dir, 'forecasters/trained_models/lstm1hour'),
    os.path.join(src_dir, 'forecasters/trained_models/hybrid2hours'),
    # ...additional forecasting models
],
    'path_datafile': os.path.join(src_dir, 'data/alberta3/ab_2018-2022_electricity_time_climate.csv')},
```

## Dependencies

The implementation uses:
- PyTorch for neural network implementation
- Stable-Baselines3 for RL algorithms
- Gymnasium for environment implementation

## Contact

For comments and questions reach out to [manuel.sage@mail.mcgill.ca](manuel.sage@mail.mcgill.ca).