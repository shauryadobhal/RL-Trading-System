# 🤖 Autonomous Deep Reinforcement Learning Trading System

This repository serves as a portfolio showcase for an autonomous, AI-driven trading system powered by Deep Reinforcement Learning (DRL). It acts as an autonomous hedge fund analyst, capable of reasoning about market conditions, formulating strategies, and executing trades with strict risk management.

> **Note**: This is a public showcase repository. The proprietary source code, dataset processing pipelines, and trained model weights (`.pt` files) are kept in a separate, secure private repository to protect the intellectual property.

---

## 🏗️ System Architecture

The core of the decision-making engine is built upon a **Double Deep Q-Network (Double DQN)** constructed using PyTorch. The architecture is designed to handle non-stationary financial time-series data while aggressively mitigating the risk of catastrophic overfitting.

### 🧠 Core Agent Engine (`DQNAgent`)
- **Framework:** PyTorch (`torch.nn`)
- **Algorithm:** Double DQN (mitigates the overestimation bias inherent in standard Q-Learning).
- **Network Topology (`QNet`):** 
  - Multi-layer perceptron (MLP) architecture.
  - Integration of `LayerNorm` and `Dropout` (0.2) specifically engineered to prevent the model from memorizing specific historical market regimes (e.g., the pre-2019 bull market).
- **Optimization:** Adam Optimizer with Smooth L1 Loss (Huber) and Gradient Norm Clipping.
- **Exploration Strategy:** $\epsilon$-greedy exploration with linear decay over extensive environmental steps.

### 🔄 Memory & Replay
- **Experience Replay:** A high-capacity `ReplayBuffer` (up to 200,000 transitions) breaks the temporal correlation of sequential financial data.
- **Target Network:** Separate target network updated periodically to stabilize the temporal difference (TD) targets during Q-value updates.

---

## 📊 Modules & Pipeline

While the code is private, the system is architected into several robust, scalable modules:

1. **Feature Engineering (`/features`):** Processing raw market data into stationary, normalized state-space vectors suitable for neural network ingestion.
2. **Custom Trading Environment (`/env`):** A custom OpenAI Gym-style environment that accurately simulates market mechanics, including transaction costs, slippage, and liquidity constraints.
3. **Risk Management (`/risk`):** Dynamic position sizing and hard stop-loss mechanisms ensuring the agent's actions never exceed predefined drawdown limits.
4. **Portfolio Management (`/portfolio`):** Tracking historical allocations, cash balances, and overall portfolio value across time.
5. **Execution (`/scripts`):** Walk-forward validation (`paper_trade_walk.py`) and daily paper trading modes (`paper_trade_daily.py`).

---

## 🎯 Objective & Metrics

The fundamental objective of this agent is to maximize risk-adjusted returns rather than absolute cumulative wealth. The reward function naturally incentivizes consistently positive expected value (EV) decisions while penalizing high-variance actions. 

**Target Performance Metrics:**
- High Sharpe & Sortino Ratios
- Minimized Maximum Drawdown (MDD)
- Consistent Average Market Exposure

---
*Developed by Shaurya Dobhal.*
