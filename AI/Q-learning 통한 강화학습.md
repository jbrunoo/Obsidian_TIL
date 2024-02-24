```python
pip install gym==0.25.2 , pygame==2.5.1
```

```python
from collections import defaultdict
import pickle
import random
from IPython.display import clear_output
import numpy as np

import click
import gym
```

```python
env = gym.make("Taxi-v3").env
env.reset()
state = list(env.decode(61))
state
action = 0
env.step(action)
print(env.render(mode='ansi'))

from PIL import Image
from IPython.display import display, clear_output
import time
env.reset()
for _ in range(10):  # 10 스텝 시뮬레이션
    img = env.render(mode='rgb_array')
    display(Image.fromarray(img))  # 주피터 노트북에 이미지 표시
    clear_output(wait=True)  # 이전 출력 내용 지우기
    time.sleep(0.1)  # 0.1초 기다리기 (시뮬레이션 속도 조절)
```

```python
done = False
while not done:
    img = env.render(mode='rgb_array')
    display(Image.fromarray(img))  # 주피터 노트북에 이미지 표시
    clear_output(wait=True)  # 이전 출력 내용 지우기
    i = int(input()) # 0 = 남쪽, 1 = 북쪽, 2 = 동쪽, 3 = 서쪽, 4 = 픽업, 5 = 하차
    obs,reward,complete,info = env.step(i) # 여기에서 환경에 대한 단계를 실행
    print('Observation = ', obs, '\nreward = ', reward, '\ndone = ', complete, '\ninformation = ', info)
    done = complete
```


## Q-learning
본질적으로 Q-learning은 에이전트가 환경의 보상을 사용하여 시간이 지남에 따라 주어진 상태에서 취해야 할 최상의 조치를 학습할 수 있도록 합니다.

AI에 무엇이 효과적 이었나를 기억하기 위해 각 단계의 결과를 **Q-table**이라는 테이블에 저장합니다. 이 테이블에는 (상태, 행동) -> Q-value의 맵이 있습니다. Q-value는 어떤 행동이 유익한지 아닌지를 나타내는 숫자입니다.

다음은 Q-table의 예제입니다
![[Pasted image 20231027144901.png]]

1. 알파(Alpha) 값. Alpha 값은 0에서 1 사이의 숫자입니다. 학습률의 척도입니다.
2. 감마(Gamma) 값. 이 값은 알고리즘이 얼마나 탐욕스러운지를 측정한 것입니다. 감마 값이 0이면 학습 알고리즘이 더 근시안적입니다.
3. 엡실론(Epsilon) 값. 이 변수는 훈련이 이전 데이터에 얼마나 의존해야 하고, 새로운 데이터에 얼마나 의존해야 하는지를 설정합니다.

```python
# 하이퍼 파라미터
alpha = 0.1
gamma = 0.6
epsilon = 0.1

NUM_EPISODES = 100000

all_epochs = []
all_penalties = []
total_reward = 0

for i in range(1, NUM_EPISODES+1):
    state = env.reset()

    epochs, penalties, reward, = 0, 0, 0
    done = False
    
    while not done:
        if random.uniform(0, 1) < epsilon: # 이전 지식을 사용하는 대신 새로운 행동을 탐구할 확률이 10%입니다.
            action = env.action_space.sample() # 작업 공간 탐색
        else:
            action = np.argmax(q_table[state]) # 학습된 값 이용

        next_state, reward, done, info = env.step(action) # 다음 단계를 수행합니다.
        total_reward += reward
        
        old_value = q_table[state, action]
        next_max = np.max(q_table[next_state])
        
        new_value = (1 - alpha) * old_value + alpha * (reward + gamma * next_max)
        q_table[state, action] = new_value

        if reward == -10:
            penalties += 1

        state = next_state
        epochs += 1
        
    if i % 100 == 0:
        clear_output(wait=True)
        print(f"Episode: {i}, rewards: {reward}, total_rewards: {total_reward} penalty: {penalties}")

print("Training finished.\n")
```

### 평가
```python
total_epochs, total_penalties = 0, 0
episodes = 100

for _ in range(episodes):
    state = env.reset()
    epochs, penalties, reward = 0, 0, 0
    
    done = False
    
    while not done:
        action = np.argmax(q_table[state])
        state, reward, done, info = env.step(action)

        if reward == -10:
            penalties += 1

        epochs += 1

    total_penalties += penalties
    total_epochs += epochs

print(f"Results after {episodes} episodes:")
print(f"Average timesteps per episode: {total_epochs / episodes}")
print(f"Average penalties per episode: {total_penalties / episodes}")
```

