# wGAN Math

## Metric (Distance)

* 특징
  * 𝒅(𝑥, 𝑦) ≥ 0
  * 𝒅(𝑥, 𝑦) = 0 ⟷ x = y
  * 𝒅(𝑥, 𝑦) = 𝒅(𝑦, 𝑥) → x, y는 대칭
  * 𝒅(𝑥, 𝑧) ≤ 𝒅(𝑥, 𝑦) + 𝒅(𝑦, 𝑧)

* 공간 별 metric
  * 실수공간 ℝ, 복소공간 ℂ : |𝑥 - 𝑦|
  * 유클리드 공간 ℝn : 유클리드 거리 √(∑|𝑥 - 𝑦|²)
    * 맨헤튼 거리 : ∑|𝑥 - 𝑦|
  * **힐베르트 공간** : 내적 𝒅(𝑢, 𝑣) = √((𝑢, 𝑣) ∙ (𝑢, 𝑣))
  * 함수 공간(L1공간 L2공간)

* 수렴을 정의하기 위해 Metric 개념이 중요
  * 𝑥n ⇒ 𝑥   ⟺   𝓁𝒾𝓂 𝒅(𝑥n, 𝑥) = 0

  * fn과 f의 차를 제곱하여 적분한 값(L2거리)이 0으로 수렴 → L2수렴

    * ![L2수렴]()

  * fn이 모든 𝑥에 대해 𝟄범위 안에 들어오면서 수렴 (L∞거리) → L∞수렴 or 균등 수렴 (uniformly converge)
    *  𝟄범위 벗어나면 측도 수렴 (converge in measure)

    * ![L inf 수렴]()

> ⇨ **거리함수가 바뀌면 수렴의 방식이 바뀜**

* 수렴간의 비교
  * 𝒅₁-수렴이 𝒅₂수렴보다 강하다 (𝒅₁ is stronger than 𝒅₂)
    * 𝒅₁(𝑥, 𝑦) → 0 ⇒ 𝒅₂(𝑥, 𝑦) → 0
  * 거꾸로 성립 : 약하다 (weaker)
  * 양방향 성립 : 동등하다 (equivalent)
    * 유클리드 거리 and 맨헤튼 거리
  * 공간마다 차이 때문에 항상 비교 가능한건 아님

* 유한 측도를 가진 공간에서는 다음이 성립
  * L∞ ⇒ L2 ⇒ 측도 수렴 (converge in measure)
* WassersteinGAN에서는 확률분포 공간에서의 Wasserstein distance를 다룸

## Compact metric set

[Compact란?](https://ko.wikipedia.org/wiki/%EC%BD%A4%ED%8C%A9%ED%8A%B8_%EA%B3%B5%EA%B0%84)

* compact 집합을 가져온 이유
  * 연속함수들이 항상 최대 최소를 가짐 (최대 최소의 정리)
  * 모든 확률변수 𝑿에 대해 조건부 확률분포가 정의
  * 완비공간이다 (Complete space)


* 확률 측도 = 확률 분포 

## Different Distance (Metrics)

### Total Variation (TV)

* 두 확률측도의 측정값이 벌어질 수 있는 값들 중 가장 큰 값
![tv1]()
* 만약 교집합이 ∅ 이면, TV = 1

### Kullback-Leibler divergence

![kl]()
* metric의 특징(대칭성, 삼각부등식)이 성립 X
  * 그래도 사용가능
* stronger than TV
* 𝛳 ≠ 0 → ㏒ = ∞ → KL = ∞

### Jensen-Shanonon divergence



