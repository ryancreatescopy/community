# 안정화 수수료

## 안정화 수수료\(Stability Fee\)는 무엇인가요?

Maker 스마트컨트랙트는 CDP에 축적된 담보와 대출된 Dai의 양을 계산하여 안정화 수수료를 청구합니다. 안정화 수수료는 MakerDAO의 임시 리스크 팀이 제안하고 MKR 보유자들이 투표하여 변동될 수 있습니다.

## 안정화 수수료는 언제 내야하나요?

유저가 CDP에 DAI를 전송하여 부채를 상환할 때, _시스템이 회수하는 DAI양에 비례하여_ 안정화 수수료가 책정됩니다. 안정화 수수료는 MKR 토큰이나 Dai로 지불할 수 있습니다.

## 안정화 수수료가 변경되면, 변경 이전의 부채도 새로운 수수료가 적용되나요?

그렇지 않습니다. 안정화 수수료는 소급적용이 되지 않습니다. 변동 금리 대출과 비슷하게, 수수료가 바뀌면 바뀐 그 순간부터 새롭게 적용되어 수수료가 축적됩니다.

## 안정화 수수료를 청구하는 이유는 무엇인가요?

안정화 수수료는 암호화폐 시장이 낮은 성장률을 보이거나 마이너스 성장을 할 때, Dai 토큰의 수요와 공급의 균형이 깨지는 것을 막아주는 리스크 변수입니다.

수수료의 메커니즘은 단순합니다; Dai의 시장수요가 _감소_ 한다면, Dai를 발행하기 위한 수수료를 증가하여 새로운 발행을 늦추어 수요-공급의 균형을 이룰 수 있도록 합니다. 반대로 수요가 _증가_ 하는 경우에는 수수료를 감소시켜 공급을 늘려 균형을 유지합니다. 이러한 구조는 CDP 소유자들이 Dai로 대출을 받거나 상환하여 소각하는 행동에 영향을 미쳐서 소프트-페그 안정화 효과를 나타내게 됩니다.

## 안정화 수수료는 왜 변동되나요?

만약 Dai의 거래가가 목표치인 $1를 상회하거나 밑돌게 되면, 이는 거시적인 Dai의 수요와 공급이 균형을 이루지 못했다는 것을 의미합니다. 안정화 수수료는 Dai를 생성하기 위한 비용에 직접 작용하기 때문에 Dai의 공급에 일차적인 영향을 미치게 됩니다. 안정화 수수료가 낮아지면, 더 많은 유저가 Dai를 대출할 것이고, 반대로 수수료가 높아지면 Dai를 대출하려는 유저가 적어질 것입니다. 이러한 안정화 수수료는, MKR 토큰 보유자들이 페그의 지속적인 안정성을 고려하여 변화시킬 수 있습니다.

Dai가 지속적으로 $1 위에서 거래된다면, 수요가 공급을 앞선 것이며, 시장 참여자들은 Dai를 사기 위해서 프리미엄을 기꺼이 지불할 용의가 있다는 것을 의미합니다. 만약 이러한 일이 자주 발생할 경우, 안정화 수수료를 낮춰서 Dai의 공급을 늘려 균형을 맞춰야합니다.

Dai가 지속적으로 $1 아래에서 거래된다면, 공급이 수요를 앞선 것이며, 시장에 Dai가 너무 많다는 것을 의미합니다. 만약 이러한 일이 자주 발생할 경우, 안정화 수수료를 높여서 Dai의 생성을 늦춰서 균형을 맞춰야 합니다.

불행하게도, 모든 결과는 시장의 반응에 따라 달라지기 때문에, 새로운 수수료가 적용되기 이전에 그 효과를 미리 정확히 예측하는것은 불가능합니다. 하지만 시간이 지나면서 데이터가 쌓이면 예측 모델이 더욱 정확해질 것이고, 새로운 탄탄한 반응형 모델이 개발된다면, 이를 통해 안정화 수수료를 더욱 완벽하게 조절할 수 있게 될 것입니다.

## 안정화 수수료는 어떻게 계산되나요?

안정화 수수료는 지속적으로 계산되며, Dai로 산정되고, DAI나 MKR로 지불할 수 있습니다. 아래의 공식에서 보실 수 있듯이, 복리는 주, 달, 연을 기준으로 측정되는 것이 아닌 매우 짧은 시간을 기준으로 측정됩니다. 이렇게 계산되는 수수료는 연 복리로 계산한 예상값과 매우 근접하게되는데, 이러한 방식이 도입된 이유는 CDP 를 강제로 유지하는 최소기간이 없어, 유지기간의 변동성이 매우 크기 때문입니다. 따라서 시스템은 매우 짧은 시간을 기준으로 측정할 수 있어야 합니다.

다음은 365일 동안 100,000 DAI 부채를 가지고 있는 상황에서 여러 방식의 복리구조를 통해 발생하는 안정화 수수료를 계산한 결과입니다.

### 수식:

변수:

**A** = 이자

**P** = 최초 투자 금액 \(최초 입금 혹은 대출 금액\)

**r** = 연 이율 \(%\)

**n** = 1년 동안 이자가 복리로 계산되는 횟수

**t** = 돈을 투자하거나 빌린 년 수

**e** = 오일러 상수

* **P** \(1 + r/n\)^nt - P = A: 연복리 \(annual compounding\)
* **P** \(1 + r/n\)^nt - P = A: 월복리 \(monthly compounding\)
* **P** \(e\)^rt - P = A: 연속 복리 \(continuous compounding\)

### 간소화

연 복리로 계산 할 경우에 안정화 수수료는 다음과 같습니다:

```text
100,000 × (1 + (2.5% / 1)) ^ (1 × 1) - 100,000 = 2500 DAI
```

월 복리로 계산 할 경우에 안정화 수수료는 다음과 같습니다:

```text
100,000 × (1 + (2.5% / 12)) ^  (12 × 1)  - 100,000 = 2528.85 DAI
```

연속 복리로 계산 할 경우에 안정화 수수료는 다음과 같습니다:

```text
100,000 × 2.7183... ^ (2.5% × 1) - 100,000 = 2,531.51 DAI
```

2.5% 연이율로 연복리와 연속복리를 계산했을 때, 100,000 DAI의 부채가 있는 경우 약 **31.51DAI** 정도의 차이가 발생합니다. 몇 가지 예시를 더 보시겠습니다.

### 단순한 예시

주어진 조건:

* 1000 DAI의 안정화 부채가 CDP에 존재
* 30일 동안 CDP가 개설되어있음
* MKR 토큰의 최근 가격은 100 DAI
* 2.5%의 안정화 수수료
* **50 DAI**를 유저가 상환함

**30일**간 유지된 **1000 DAI** 부채에서 **50 DAI**를 상환했을 때 DAI로 산정되는 수수료는 **0.102846041 DAI**이며 대략 2센트\(USD\) 입니다.

수수료를 MKR로 변환하면, 트랜잭션을 완료하기 위해서 **0.001028460 MKR**가 필요합니다.

### 자세한 예시

CDP 전체에서 발생하는 안정화 수수료는 다음과 같이 계산될 수 있습니다:

> \(\(\(DAI로 계산된 전체 안정화 부채 \* \(1 + 소수점 형태의 최신 안정화 수수료\)\) ^ \(안정화 부채의 유지 일 수/365\)\) - DAI로 계산된 전체 안정화 부채\) = DAI로 계산된 전체 안정화 수수료

공식에 각 수치를 대입해보면 다음과 같습니다:

```text
 (1000 * (1 + 0.005) ^ (30÷365)) - 1000 = 0.410018954 DAI
```

이제 DAI로 계산된 전체 안정화 수수료를 구했으니, 1 MKR의 가치를 100 DAI로 가정하여 변환하겠습니다:

```text
0.410958904 ÷ 100 = 0.004109589 MKR
```

그리고 유저가 50 DAI를 상환하므로, 그만큼의 수수료를 계산해보겠습니다.

```text
 (50 * (1 + 0.005) ^ (30÷365)) - 50 = 0.020500948 DAI
```

다시 MKR로 변환하겠습니다:

```text
0.020500948 ÷ 100 = 0.000205009 MKR
```

유저는 **30일**간 사용한 **50 DAI**에 대해서 지갑에 **0.000205009 MKR** 가 있어야 수수료를 지불할 수 있습니다.

트랜잭션이 완료되면, CDP에 남아있는 수수료는 다음과 같아질 것입니다:

0.004109589 - 0.000205009 = **0.00390458 MKR**

## 시스템은 모인 수수료를 어떻게 처리하나요?

수수료가 모이면, 스마트컨트랙트 플랫폼은 [소각주소\(Burner\)](https://etherscan.io/address/0x69076e44a9c70a67d5b79d95795aba299083c275) 로 이름 붙여진 컨트랙트로 MKR를 전송합니다. 소각주소에서 소각되기 전의 MKR는 전체 순환량에서 빠지며, 그 누구도 해당 주소에서 자산을 이동할 수 없습니다.

## 지불해야 할 안정화 수수료는 어디에서 확인할 수 있나요?

[이전 CDP 대시보드](https://dai.makerdao.com/): DAI 대시보드의 "거버넌스 부채\(Governance Debt\)" 항목을 통해 CDP에서 지불해야 할 수수료 잔고를 확인할 수 있습니다.

[새로운CDP 포탈](https://cdp.makerdao.com/): 우측 패널에서 “상환\(Payback\)” 버튼을 누르면 CDP에서 지불해야 할 수수료 잔고를 확인할 수 있습니다.

이외에도 Awesome-MakerDAO 레포지토리에 있는 [Watch your Dai section](https://github.com/makerdao/awesome-makerdao/blob/master/README.md#watch-your-dai)에서 제 3자가 제공하는 툴들도 사용하실 수 있습니다.

## 수수료가 수요와 공급에 어떤 영향을 미치나요?

안정화 수수료가 높아지면, CDP 유저가 DAI를 빌리는데 필요한 비용이 높아지게 되므로, CDP 사용의 매력을 떨어뜨려서 Dai의 공급이 줄어들게됩니다. 반대로, 안정화 수수료가 낮아지면 \(빌리는 비용\) Dai의 추가 공급에 대한 인센티브가 발생하므로 일종의 정책도구로서 공급을 늘리게 됩니다.

## 변동되는 안정화 수수료를 어떻게 계산하나요?

아래 제공되는 단순한 수식을 통해 지불해야 할 안정화 수수료를 계산할 수 있습니다:

> \(\(\(DAI로 계산된 전체 안정화 부채 \* \(1 + 소수점 형태의 최신 안정화 수수료\)\) ^ \(안정화 부채의 유지 일 수/365\)\) - DAI로 계산된 전체 안정화 부채\) = DAI로 계산된 전체 안정화 수수료

0.5% 이자율로 10,000 DAI를 31일간 대출하였을 경우에 수수료는 다음과 같이 계산됩니다:

```text
(10000 x (2.7183...) ^ (0.5%*(31/365)) - 10000 = 4.2474 DAI
```

2.5%일때는 다음과 같습니다:

```text
(10000 x (2.7183...) ^ (2.5%*(31/365)) - 10000 = 21.2554 DAI
```

## 리스크 팀에 대해 더 알아보고, 미래의 시스템 변화에 대해서 재단과 소통하려면 어떻게 해야하나요?

모든 이슈들을 훨씬 더 자세하게 토의하는 주간 [Governance and Risk](https://calendar.google.com/calendar/embed?src=makerdao.com_3efhm2ghipksegl009ktniomdk%40group.calendar.google.com&ctz=America%2FLos_Angeles) 미팅에 참여하시면 됩니다. 미팅의 어젠다는 주기적으로 [r/MakerDAO](https://www.reddit.com/r/MakerDAO/)에 게시됩니다. 또한 Awesome-MakerDAO 레포지토리의 [Governance section](https://github.com/makerdao/awesome-makerdao/blob/master/README.md#governance)도 참고하세요.

## 안정화 수수료 변동의 제한이 있나요?

리스크 팀이 안정화 수수료의 최대 변동치에 관련된 제안\(변동의 횟수, 페그의 편차\(deviation of the peg\), 샘플링 시간\)을 제시하고 Maker 투표자들의 승인을 받을 것입니다.

## CDP소유자들이 어떻게 하면 안정화 수수료 변동에 의한 리스크를 줄일 수 있을까요?

수수료를 헤징하기 위한 몇 가지 옵션들이 있습니다.

MKR의 수요와 안정화 수수료는 양의 관계로 비례할 수도 있습니다. 만약 이 관련성이 의미있고 지속된다면, CDP 소유자들은 MKR을 보유함으로써 안정화 수수료 변동에 대한 헤징을 할 수 있습니다.

고정 약관과 고정금리로 대출받으려는 사람은, 다른 대출 플랫폼에서 고정금리로 대출받으려는 상대방을 찾아 미래의 수수료 변동성에 대한 헤징을 할 수 있습니다.

가장 좋은 리스크 관리 전략은 거버넌스 과정에 참여하여 제안이 비준될 때 투표를 꼭 하는 것입니다.

## 안정화 수수료 변동 투표가 부결되면 어떻게 되나요?

안정화 수수료 변동 투표가 부결된다면, 수요-공급 불균형이 계속 이어져서 Dai 가격의 하락을 일으킬 수 있습니다. 만약 이러한 상황이 지속된다면 재단으로서는 Dai 보유자들의 경제적 가치를 보존하기위해 응급 셧다운을 실시할 수 밖에 없게 됩니다

투표 메커니즘은 커뮤니티가 페그를 관리하고, 여러 약관 조항들을 조절할 수 있는 방법입니다. 그 어떤 ‘백도어’도 없으며, 그 누구도 리스크 변수들을 독단적으로 조절할 수 없습니다.

## 안정화 수수료가 얼마나 자주 변동되나요?

새로운 수수료가 얼마가 될지 그리고 언제 바뀔지는 미리 예측할 수 없습니다.

임시 리스크 팀이 이전의 변화들을 지속적으로 모니터링하여 필요 시에 비슷하거나 완전히 다른 변화를 제안할 수 있습니다. 시장에 얼마나 빨리, 그리고 얼마나 넓은 범위로 영향을 미치는지 알기는 매우 힘들기 때문에, 리스크팀은 각 사례를 바탕으로 접근할 것입니다.

안정화 수수료의 변동은 조작의 리스크가 있기 때문에 신중하게 접근해야하고, 안정화 수수료가 정확히 어떻게 바뀌어야 하는지에 관련된 신호들을 확인하는 것이 필요합니다. 알고리즘으로 진행되는 완전 자동화된 프로세스는 조작에 취약하기 때문에 이를 면밀히 고려해야합니다.
