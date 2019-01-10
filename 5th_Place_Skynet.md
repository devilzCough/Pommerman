# Pommerman

Team competition : 2018. 12. 8 NeurIPS 컨퍼런스에서 진행됨

### 2nd Place in learning agents _ Skynet (5th place in global rank)

blog : https://www.borealisai.com/en/blog/pommerman-team-competition-or-how-we-learned-stop-worrying-and-love-battle/



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

- **Sparse, delayed reward** : 에이전트는 게임이 종료될 때까지 어떤 보상도 받지 못한다. 보상은 +1 또는 -1인데 양쪽 팀의 에이전트가 같은 타임스텝에서 모두 죽게 되면 각각 -1을 받고 