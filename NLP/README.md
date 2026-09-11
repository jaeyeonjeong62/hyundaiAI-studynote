# RNN에서 GPT까지 — 발전 과정과 계산 원리

NLP 모델이 RNN → LSTM → Seq2Seq → Attention → Transformer → BERT / GPT로 어떻게 발전했는지, 각 단계에서 **무엇이 유지되고 무엇이 추가·제거되었는지**를 정리한 노트입니다. 그림은 구조만 담고, 설명과 수식은 아래 본문에 있습니다.

모든 단계에 같은 예시 문장을 씁니다.

```text
입력: I love this movie
출력: 나는 이 영화를 좋아한다
```

## 시각 요소 약속

![시각 요소 약속](docs/img/00-legend.png)

모든 그림에서 같은 의미로 씁니다. 검은 테두리는 Encoder, 빨간 테두리는 Decoder, 검게 채운 칩은 hidden state, 점선 칩은 cell state, 빨갛게 채운 칩은 context vector입니다.

## 용어 구분

이 이름들은 같은 종류의 개념이 아닙니다. 이것이 발전 과정을 이해하는 출발점입니다.

| 용어 | 무엇인가 |
| --- | --- |
| RNN | 순환 신경망 계열 |
| LSTM | RNN의 은닉 상태 계산을 개선한 **셀** |
| Seq2Seq | Encoder와 Decoder를 연결하는 **구성 방식** |
| Attention | 필요한 입력 위치를 가중합하는 **계산 장치** |
| Transformer | RNN 없이 Attention으로 만든 **모델 구조** |
| BERT | Transformer Encoder 계열 |
| GPT | Causal Transformer Decoder 계열 |

## 공통 전제 — 토큰과 임베딩

모든 모델의 입구는 같습니다.

```text
토큰 "영화를" → 토큰 ID 4217 → embedding matrix 조회 → E(영화를) ∈ ℝ⁵¹²
```

embedding matrix는 미리 만들어 둔 Word2Vec을 반드시 쓰는 것이 아니라, 보통 모델 전체와 함께 학습됩니다.

---

## 01 · RNN

Elman, *Finding Structure in Time* (1990)

![RNN](docs/img/01-rnn.png)

토큰을 순서대로 하나씩 받아, 지금까지 읽은 내용을 hidden state 하나에 담아 다음 시점으로 넘깁니다. 시점마다 새 셀이 생기는 것이 아니라 같은 셀, 같은 파라미터를 반복 사용합니다.

$$h_t = \tanh(W_x x_t + W_h h_{t-1} + b)$$

$$y_t = \mathrm{softmax}(W_o h_t)$$

작은 예시 — $h_1 = [0.4,\, -0.2]$ 에 "영화를"이 들어오면 $h_2 = \tanh(W_x x_2 + W_h h_1) = [0.7,\, 0.1]$. 새 입력과 이전 기억이 한 번의 $\tanh$로 섞입니다.

| 유지 | 추가 | 제거 |
| --- | --- | --- |
| 토큰 임베딩 입력 | 순환 연결 $h_{t-1} \to h_t$ | 없음 (출발점) |

**남은 문제** — 기억이 $h_t$ 하나라서 매 시점 덮어쓰이고, 문장이 길어지면 역전파에서 기울기가 소실되어 앞부분 정보가 사라집니다.

## 02 · LSTM

Hochreiter & Schmidhuber, *Long Short-Term Memory* (1997)

![LSTM](docs/img/02-lstm.png)

기억 통로 $c_t$ 를 따로 두고, 세 개의 gate가 무엇을 잊고 무엇을 새로 넣고 무엇을 내보낼지 결정합니다. $f$ 가 1에 가까우면 옛 기억이 거의 그대로 통과하며, 이 곱셈 경로가 기울기 소실을 완화합니다.

$$f,\, i,\, o = \sigma(W \cdot [h_{t-1}, x_t] + b)$$

$$\tilde{g} = \tanh(W_g \cdot [h_{t-1}, x_t] + b_g)$$

$$c_t = f \odot c_{t-1} + i \odot \tilde{g}$$

$$h_t = o \odot \tanh(c_t)$$

작은 예시 — $c_{t-1} = 1.0$, $f = 0.9$, $i = 0.2$, $\tilde{g} = 0.5$ 이면 $c_t = 0.9 \times 1.0 + 0.2 \times 0.5 = 0.95$. 옛 기억의 대부분이 살아남습니다.

| 유지 | 추가 | 제거 |
| --- | --- | --- |
| 순차 처리, $h_t$ 전달 방식 | cell state, forget·input·output gate | 단일 $\tanh$ 갱신 |

**남은 문제** — LSTM은 셀일 뿐이어서, 입력 4토큰을 읽고 출력 5토큰을 만드는 번역처럼 길이가 다른 작업을 담을 구조가 없습니다.

## 03 · Seq2Seq

Sutskever, Vinyals, Le, *Sequence to Sequence Learning with Neural Networks* (2014)

![Seq2Seq](docs/img/03-seq2seq.png)

같은 LSTM 계산식을 **역할**로 나눕니다. Encoder는 입력 표현을 만들고, Decoder는 다음 토큰의 확률을 계산합니다. 같은 계산식을 쓰지만 파라미터는 서로 다릅니다($\theta_E \neq \theta_D$). Encoder와 Decoder는 특정 신경망의 이름이 아니라 역할의 이름입니다.

$$c = h_T^{enc}$$

$$s_i = \mathrm{LSTM}_{dec}(E(y_{i-1}),\, s_{i-1}), \quad s_0 = c$$

Decoder도 시점마다 새로 생기지 않습니다. 동일한 셀에 들어가는 이전 토큰과 상태만 바뀝니다.

| 생성 시점 | 입력 | 예측 토큰 |
| --- | --- | --- |
| 1 | `<BOS>`, $s_0 = c$ | 나는 |
| 2 | 나는, $s_1$ | 이 |
| 3 | 이, $s_2$ | 영화를 |
| 4 | 영화를, $s_3$ | 좋아한다 |
| 5 | 좋아한다, $s_4$ | `<EOS>` |

다음 토큰의 후보는 미리 정해진 어휘 집합(tokenizer vocabulary)입니다. Decoder 상태를 Linear layer에 넣어 어휘 크기만큼의 logits를 만들고, softmax로 확률분포를 얻습니다.

$$z_i = W_o h_i + b_o \in \mathbb{R}^{|V|}$$

$$P(y_i \mid y_{<i}, x) = \mathrm{softmax}(z_i)$$

여기서 greedy(argmax), sampling, top-k / top-p, beam search 중 하나로 한 토큰을 고릅니다.

| 유지 | 추가 | 제거 |
| --- | --- | --- |
| LSTM 셀 계산식 그대로 | Encoder·Decoder 역할 분리, 어휘 출력층 | 입력과 출력 길이가 같아야 한다는 제약 |

**남은 문제** — 문장이 20토큰이든 100토큰이든 벡터 $c$ 하나에 압축해야 합니다(고정 벡터 병목).

## 04 · Attention (Bahdanau)

Bahdanau, Cho, Bengio, *Neural Machine Translation by Jointly Learning to Align and Translate* (2015)

![Attention](docs/img/04-attention.png)

Attention은 Decoder를 새로 만든 것이 아닙니다. Encoder와 Decoder는 Attention 이전부터 있었고, Attention은 기존 RNN/LSTM Encoder–Decoder에 덧붙인 계산 장치입니다. Decoder가 다음 단어를 만들 때 Encoder의 마지막 상태만 보는 대신 모든 상태를 다시 참고하게 합니다.

$$e_{ij} = v_a^\top \tanh(W_s s_{i-1} + W_h h_j + b_a)$$

$$\alpha_{ij} = \frac{\exp(e_{ij})}{\sum_k \exp(e_{ik})}$$

$$c_i = \sum_j \alpha_{ij} h_j$$

$$s_i = \mathrm{Decoder}(s_{i-1},\, E(y_{i-1}),\, c_i)$$

그림의 숫자대로라면 $c_i = 0.07 h_1 + 0.83 h_2 + 0.10 h_3$ 이 되고, 시점 $i$ 마다 새로운 $c_i$ 가 만들어집니다. Attention은 다음 단어를 직접 고르는 장치가 아니라, **입력의 어느 부분을 얼마나 참고할지** 정하는 장치입니다.

attention weight는 사람이 지정하지 않습니다. 정답 문장과 예측의 차이로 손실을 계산하고 역전파로 함께 학습됩니다.

$$L = -\sum_i \log P(y_i^{\text{정답}})$$

```text
손실 → 출력층 → Decoder → context vector → α → 점수 신경망 → Encoder
```

| 유지 | 추가 | 제거 |
| --- | --- | --- |
| Encoder LSTM, Decoder LSTM | 점수 신경망, $\alpha$, 시점별 $c_i$ | 하나뿐인 고정 context vector |

**남은 문제** — 참고 범위는 넓어졌지만 Encoder·Decoder가 여전히 RNN이라 $h_{t-1}$ 을 기다려야 하고, 병렬 계산이 불가능합니다.

## 05 · Transformer

Vaswani et al., *Attention Is All You Need* (2017)

![Transformer](docs/img/05-transformer.png)

역할(Encoder·Decoder)은 그대로 두고, 그 역할을 수행하는 내부 부품을 LSTM에서 Attention 블록으로 교체했습니다. 모든 토큰을 동시에 받아, 각 토큰이 다른 모든 토큰을 직접 참고합니다.

$$X = \text{Token Embedding} + \text{Positional Encoding}$$

$$Q = XW^Q, \quad K = XW^K, \quad V = XW^V$$

$$\mathrm{Attention}(Q, K, V) = \mathrm{softmax}\!\left(\frac{QK^\top}{\sqrt{d_k}}\right)V$$

$$\mathrm{FFN}(x) = \mathrm{ReLU}(xW_1 + b_1)W_2 + b_2$$

순환 계산이 사라져 토큰 순서를 알 수 없으므로 positional encoding을 더합니다. Decoder는 이전 hidden state를 넘기는 대신 지금까지의 출력 토큰 전체를 다시 입력으로 받고, Masked(Causal) Self-Attention으로 미래를 가립니다.

```text
1번째 위치 · 1번 토큰만 참고
2번째 위치 · 1–2번 토큰 참고
3번째 위치 · 1–3번 토큰 참고
```

Cross-Attention에서는 Query가 Decoder의 현재 표현, Key와 Value가 Encoder 출력입니다. Bahdanau Attention과 같은 일을 $Q \cdot K \cdot V$ 행렬 곱으로 합니다.

**학습과 생성의 차이** — 학습 때는 정답을 알고 있으므로 여러 출력 위치를 병렬 계산합니다.

```text
Decoder 입력: <BOS> 나는 이 영화를
정답:          나는   이 영화를 좋아한다
```

실제 추론에서는 미래 토큰이 없으므로 한 토큰씩 auto-regressive하게 이어 붙입니다.

```text
<BOS> → 나는 → 나는 이 → 나는 이 영화를 → 나는 이 영화를 좋아한다 → <EOS>
```

| 유지 | 추가 | 제거 |
| --- | --- | --- |
| Encoder–Decoder 역할, Attention 개념, 어휘 출력층 | Self-Attention, Multi-Head, positional encoding, Add & LayerNorm, causal mask | RNN·LSTM 순환 계산, hidden state 순차 전달 |

**다음 단계** — 한계가 아니라 분기입니다. 이해가 목적이면 Encoder 블록을, 생성이 목적이면 Causal Decoder 블록을 쌓습니다.

## 06 · BERT / GPT

![BERT와 GPT](docs/img/06-bert-gpt.png)

### BERT

Devlin et al., *BERT: Pre-training of Deep Bidirectional Transformers* (2018)

Transformer Encoder 블록을 여러 층 쌓은 구조입니다. 각 토큰이 왼쪽과 오른쪽 문맥을 모두 참고하므로 입력 전체의 의미를 이해하고 표현하는 데 적합합니다. Masked Language Modeling으로 사전학습하고, 문장 분류·토큰 분류·질의응답에 fine-tuning합니다. 기본 BERT에는 생성용 Decoder가 없습니다.

$$H^{(l)} = \mathrm{EncoderBlock}(H^{(l-1)})$$

$$P(x_{\text{masked}}) = \mathrm{softmax}(W h_{[MASK]})$$

### GPT

Radford et al., *Improving Language Understanding by Generative Pre-Training* (2018)

Decoder 계열 블록을 쌓지만, 번역용 Transformer Decoder에 있던 Encoder–Decoder Cross-Attention은 없습니다. 참고할 입력이 따로 있는 것이 아니라 지금까지의 출력 자체가 입력입니다.

$$H^{(l)} = \mathrm{DecoderBlock}(H^{(l-1)},\, \text{causal mask})$$

$$\text{logits} = W_{vocab} h_t$$

$$P(y_{t+1} \mid y_{\leq t}) = \mathrm{softmax}(\text{logits})$$

고른 토큰을 입력 뒤에 붙이고 같은 계산을 반복합니다.

---

## 한 줄 요약

순차 전달 구조가 장기 기억을 얻고, Encoder–Decoder로 확장되고, Attention으로 입력을 다시 참고하게 된 뒤, RNN을 버리고 토큰 사이 관계를 직접 계산하는 Transformer가 되었으며, 여기서 이해 중심 BERT와 생성 중심 GPT가 갈라졌습니다.

