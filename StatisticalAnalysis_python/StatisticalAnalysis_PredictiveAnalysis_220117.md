# 추론 통계(Predictive Analysis)

- 객관적 확률

  - 논리적(고전적) 확률 : 동전던지기, 주사위던지기처럼 결과의 개수가 정해져 있음
  - 경험적 확률(상대도수 확률) : 타율, 자유투 성공률처럼 실험을 계속 반복해서 비율, 즉 상대도수로 계산

  

- 주관적 확률



- 상호배반 확률 : 교집합이 없는 확률



- 주변확률 : 두 변수 중 한 변수만을 고려
- 결합확률 : 두 변수 둘다 고려한 확률



- 독립 사상 : 사상 두개가 독립적인 것

- 베이즈 정리 : 대충만 알아보기





- 확률분포(상대도수) : 도수분포표 및 히스토그램, 도수의 확률
- 확률변수 : 불량품의 개수,
- 개수가 유한 : 이산확률변수 -> 히스토그램의 막대 높이
- 몸무게처럼 유한이 아니라 범위 : 연속확률변수 -> 곡선 . 면적으로 나타냄



<확률분포>

기대값: 이산은 가중평균, 연속은 적분개념의 면적

분산: 



연속형 확률분포 --> 정규분포

베르누이 --> 실패 아니면 성공 극단적인 값 두개만(이거는 이산)

f분포는 두 그룹이상비교



연속형: 정규분포, t분포, 카이제곱, f분포

이산형 : 이항분포, 포아송분포



이항분포: 베르누이시행을 n번 실행. 동전을 5번던졌을 때 확률

- 베르누이 : 실험 결과가 두가지 하나
- 0.5보다 크면 치우치고 뭐 이런거 있음



정규분포 : 좌우대칭,가우지안 분포, 연속형, 평균과 표준편차로 모양이 결정됨. 평균중앙값최빈값이 같다

평균과 표준편차에 의해서 그래프 모양이 결정됨

표준정규분포 : 평균은 0, 표준편차는 1(z분포)



## 추정과 가설검정

df = pd.read_csv('./data/ch4_scores400.csv')
scores = df['score']
scores[:10]



freq, _ = np.histogram(sample, bins=6, range=(1,7))
pd.DataFrame({'frequency':freq,
             'rel.freq':freq/num_trial},
            index=pd.Index(np.arange(1,7), name='dice'))



fig = plt.figure(figsize=(10, 6))
ax = fig.add_subplot(111)
ax.hist(sample, bins=6, range=(1, 7), density=True, rwidth=0.8)
# 실제의 확률분포를 가로선으로 표시
ax.hlines(prob, np.arange(1, 7), np.arange(2, 8), colors='gray')
# 막대 그래프의 [1.5, 2.5, ..., 6.5]에 눈금을 표시
ax.set_xticks(np.linspace(1.5, 6.5, 6))
# 주사위 눈의 값은 [1, 2, 3, 4, 5, 6]
ax.set_xticklabels(np.arange(1, 7))
ax.set_xlabel('dice')
ax.set_ylabel('relative frequency')
plt.show()



num_trial = 10000
sample = np.random.choice(dice, size=num_trial, p=prob)

fig = plt.figure(figsize=(10, 6))
ax = fig.add_subplot(111)
ax.hist(sample, bins=6, range=(1, 7), density=True, rwidth=0.8)
ax.hlines(prob, np.arange(1, 7), np.arange(2, 8), colors='gray')
# ax.set_xticks(np.linspace(1.5, 6.5, 6))
# ax.set_xticklabels(np.arange(1, 7))
ax.set_xlabel('dice')
ax.set_ylabel('relative frequency')
plt.show()





fig = plt.figure(figsize=(10, 6))
ax = fig.add_subplot(111)
ax.hist(scores, bins=100, range=(0, 100), density=True)
ax.set_xlim(20, 100)
ax.set_ylim(0, 0.042)
ax.set_xlabel('score')
ax.set_ylabel('relative frequency')
plt.show()