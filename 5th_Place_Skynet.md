# Pommerman

Team competition : 2018. 12. 8 NeurIPS 컨퍼런스에서 진행됨

### 2nd Place in learning agents _ Skynet (5th place in global rank)

original blog : https://www.borealisai.com/en/blog/pommerman-team-competition-or-how-we-learned-stop-worrying-and-love-battle/

[Blog 번역글]

### 1. The rules of engagement

- Pommerman team competition은 4개의 bomber 에이전트로 구성

- 각 에이전트는 11x11 사이즈 보드판의 각 코너에 놓이게 됨
- 2개의 팀이며 각 팀에 2개의 에이전트로 구성



#### Rules

- 매 타임스텝마다 각 에이전트는 6개의 행동 중 하나를 실행할 수 있다. 행동은 상하좌우로 움직이거나 제자리에 있거나 폭탄을 놓는 것이다.
- 보드의 각 셀은 통로, 부숴지지 않는 단단한 벽, 폭탄으로 부술수 있는 나무판으로 구성될 수 있다.
- 게임 맵은 랜덤하게 생성되나 에이전트 들 사이의 통로는 보장되고 점차적으로 생성되는 맵은 플레이 할 수 있도록 보장된다.
- 에이전트가 폭탄을 놓으면 10 타임스텝 이후에 폭발하며 불길은 2 타임스템 동안 유지된다. 불길은 나무 판을 제거하고 불길 범위에 있는 에이전트를 죽일 수 있다. 나무판이 부숴지면 통로가 나타나거나 아이템이 나온다.
- 아이템은 에이전트의 능력을 강화시켜주는데 세 가지 종류가 있다. 1) 폭탄의 불길 범위가 늘어나거나 2) 에이전트가 놓을 수 있는 폭탄의 개수가 증가하거나, 폭탄을 찰 수 있는 능력을 갖게된다.
- 각 게임 에피소드는 최대 800 타임스텝이며 게임 종류에는 2가지 조건이 있다. 한 쪽 팀이 이기거나 800 타임스텝을 다 채우게 되면 게임은 종료된다.



### 2. Challenges

 : Pommerman team competition은 강화학습에 있어 도전적인 문제이다. 그에 대한 이유로는 아래와 같다.

- **Sparse, delayed reward** : 에이전트는 게임이 종료될 때까지 어떤 보상도 받지 못한다. 보상은 +1 또는 -1이다. 양쪽 팀의 에이전트가 같은 타임스텝에서 모두 죽게 되면 각각 -1을 받고 일시적인 신뢰 할당 문제(credit-assignment problem)를 만든다. 신뢰 할당 문제는 sparse하고 delayed 되는 보상으로 부터 원자적 행동에 대한 보상 문제를 해결하려고 한다.

- **Noisy rewards** : Pommerman 게임에서 관찰되는 보상들이 잠재적인 자살행위로 인해 노이즈가 발생할 수 있다. 예를 들면, 오직 두 에이전트가 있는 간단한 게임 시나리오를 생각해보면, 이 경우 우리의 학습 에이전트와 상대가 있을 것이다. 우리는 (우리의 학습 에이전트의 전투 스킬이 아닌) 상대의 자살로 인해 우리의 에이전트가 +1의 보상으 받게 되는 false positive 에피소드와 우리 에이전트의 자살로 인해 -1의 보상을 받게 되는 false negative 에피소드를 고려할 수 있다. False negative 에피소드는 model-free RL 내에서 순수한 탐험을 통해 합리적인 행동을 학습하는데 있어 주요한 걸림돌이다. 또한 false positive 에피소드는 상대방과 적극적으로 부딪히기 보다 camping과 같은 임의의 정책에 대해 에이전트에게 보상을 줄 수 있다.

- **Partial observability** : 에이전트는 오직 그들의 

- **Vast search space** : 
- **Lack of a fast-forward simulator** : 
- **Difficulty of learning to place a bomb** :
- **Multi-agent challenges** : 
- **Opponent generalization challenge** : 



### 3. Our Skynet team

Skynet team은 단일 신경망으로 되어 있으며 다섯가지 단계를 기반으로 한다.

1. parameter sharing
2. reward shaping
3. an Action Filter module
4. opponent curriculum learning
5. an efficient RL algorithm

우리는 **파라미터 공유 방식**을 사용한다. 즉, 우리는 에이전트들이 단일 네트워크의 파라미터를 공유할 수 있도록 했다. 이것은 두 에이전트의 경험을 통해 네트워크가 학습되도록 한다. 그러나 이것은 또한 각각의 에이전트가 다른 시각을 갖기 때문에 에이전트 간의 다양한 행동들을 허용하도록 한다. 

게다가 우리는 에이전트가 학습 성능을 향상하도록 **dense reward**를 추가했다. 우리는 다른 보상 방식으로 부터 영감을 얻었다.