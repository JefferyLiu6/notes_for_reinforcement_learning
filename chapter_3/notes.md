<!-- Include MathJax for rendering LaTeX equations -->
<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

# Chapter 3: Finite Markov Decision Processes (MDPs)

## Learning Objectives
- Define MDPs and their core components: **states**, **actions**, **rewards**, **dynamics function**, and **policies**.
- Understand the **Bellman equations** for value functions and optimality.
- Differentiate between **episodic** and **continuing tasks** and calculate **returns** with/without discounting.
- Explain how **optimal policies** and **value functions** are derived.

---

## 3.1 Agent-Environment Interface  
**MDPs** formalize sequential decision-making where actions affect both immediate rewards and future states.

### Key Components:
- **Agent**: Learner/decision-maker.
- **Environment**: Everything outside the agent.
- **State (\(S_t\))**: Representation of the environment at time \(t\).
- **Action (\(A_t\))**: Choice made by the agent.
- **Reward (\(R_{t+1}\))**: Immediate feedback from the environment.
- **Dynamics Function (\(p\))**:
  $$
  p(s', r \mid s, a) \doteq \Pr\{S_t = s', R_t = r \mid S_{t-1} = s, A_{t-1} = a\}
  $$
  Describes the probability of transitioning to state \(s'\) with reward \(r\) after taking action \(a\) in state \(s\).

### Example: Recycling Robot  
- **States**: `high` (battery level), `low`.  
- **Actions**: `search`, `wait`, `recharge`.  
- **Rewards**:  
  - Positive for collecting cans.  
  - \(-3\) if battery depletes.  
- **Dynamics**: Transition probabilities depend on current state and action (e.g., searching with `high` battery has probability \(\alpha\) to stay `high`).

---

## 3.2 Goals and Rewards  
### Reward Hypothesis  
> *All goals can be framed as maximizing cumulative reward.*

- **Reward Signal**: Immediate feedback (\(R_{t+1}\)).  
- **Value Function**: Long-term expected return (\(v_\pi(s)\) or \(q_\pi(s, a)\)).

**Example**:  
- Chess: +1 for win, \(-1\) for loss, 0 otherwise.  
- Pole-balancing: \(-1\) on failure, 0 otherwise.

---

## 3.3 Returns and Episodes  
### Returns  
- **Episodic Tasks**: Finite time steps (e.g., a game).  
  $$
  G_t = R_{t+1} + R_{t+2} + \cdots + R_T
  $$
- **Continuing Tasks**: Infinite horizon (e.g., robot operation).  
  $$
  G_t = R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + \cdots = \sum_{k=0}^\infty \gamma^k R_{t+k+1}
  $$
  where \(\gamma \in [0, 1]\) is the **discount factor**.

### Key Insight:
- **Discounting** prioritizes immediate rewards over delayed ones.

**Example**:  
If \(\gamma = 0.9\) and \(R = [2, 7, 7, \ldots]\), then  
$$
G_0 = 2 + 0.9 \times \frac{7}{1 - 0.9} = 65.
$$

---

## 3.4 Unified Notation  
- **Absorbing State**: Terminal state for episodic tasks with 0 reward thereafter.  
- Unified return formula:
  $$
  G_t = \sum_{k=t+1}^T \gamma^{k-t-1} R_k
  $$
  Handles both episodic (\(T < \infty\)) and continuing (\(T = \infty\), \(\gamma < 1\)) tasks.

---

## 3.5 Policies and Value Functions  
### Definitions:
- **Policy (\(\pi\))**: Mapping from states to action probabilities.
  $$
  \pi(a \mid s) = \Pr\{A_t = a \mid S_t = s\}
  $$
- **State-Value Function (\(v_\pi\))**: Expected return from state \(s\) under \(\pi\):
  $$
  v_\pi(s) = \mathbb{E}_\pi[G_t \mid S_t = s]
  $$
- **Action-Value Function (\(q_\pi\))**: Expected return from taking \(a\) in \(s\):
  $$
  q_\pi(s, a) = \mathbb{E}_\pi[G_t \mid S_t = s, A_t = a]
  $$

### Bellman Equation for \(v_\pi\):
$$
v_\pi(s) = \sum_a \pi(a \mid s) \sum_{s', r} p(s', r \mid s, a) \Bigl[ r + \gamma v_\pi(s') \Bigr]
$$

**Example: Gridworld**  
- States: Grid cells.  
- Actions: Move N/S/E/W.  
- Value function \(v_\pi\) computed via Bellman equations (see Figure 3.2).

---

## 3.6 Optimal Policies and Value Functions  
### Optimal Value Functions:
- **Optimal State-Value Function (\(v_*\))**:
  $$
  v_*(s) = \max_\pi v_\pi(s)
  $$
- **Optimal Action-Value Function (\(q_*\))**:
  $$
  q_*(s, a) = \max_\pi q_\pi(s, a)
  $$

### Bellman Optimality Equations:  
For \(v_*\):
$$
v_*(s) = \max_a \sum_{s', r} p(s', r \mid s, a) \Bigl[ r + \gamma v_*(s') \Bigr]
$$
For \(q_*\):
$$
q_*(s, a) = \sum_{s', r} p(s', r \mid s, a) \Bigl[ r + \gamma \max_{a'} q_*(s', a') \Bigr]
$$

**Example**:  
- **Gridworld Optimal Policy**: Actions leading to highest values (see Figure 3.5).

---

## 3.7 Summary  
- **MDPs** model sequential decision-making with states, actions, rewards, and dynamics.
- **Value functions** (\(v_\pi\), \(q_\pi\)) and **Bellman equations** are central to solving MDPs.
- **Optimal policies** maximize cumulative reward and satisfy Bellman optimality equations.
- **Approximations** are often necessary due to computational limits in real-world tasks.

---

## Exercises (Selected)
1. **Exercise 3.1**: Design three MDP tasks (e.g., robot navigation, stock trading).
2. **Exercise 3.8**: Compute returns for \(\gamma=0.5\) and reward sequence \([-1, 2, 6, 3, 2]\).
3. **Exercise 3.14**: Verify Bellman equation for Gridworld’s center state.
4. **Exercise 3.22**: Determine optimal policies for different \(\gamma\) in a simple MDP.

---

**Next**: [Chapter 4: Dynamic Programming](link) for solving MDPs with known dynamics.

