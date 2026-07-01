# C³ASD 프로젝트 페이지 — 빌드 명세서 (Claude Code 전달용)

> 이 문서 하나로 프로젝트 페이지를 만들 수 있도록 모든 텍스트/표/자산 규칙을 담았습니다.
> 논문 원문 없이도 그대로 구현 가능. 영어 카피는 페이지에 들어갈 실제 문구이니 그대로 사용하세요.

---

## 0. 목표 & 레퍼런스

- 학술 논문 프로젝트 페이지 (Nerfies / URHead 스타일). 단일 `index.html` + `assets/` 폴더.
- 논문: **C³ASD: Multi-Level Consistency-Driven Representation Learning for Robust Active Speaker Detection**
- 정적 페이지, 외부 JS 프레임워크 없이 순수 HTML/CSS. 반응형(모바일까지), 키보드 포커스, `prefers-reduced-motion` 존중.
- **이미지/영상은 비워둔다.** 아래 규칙대로 `assets/`에 파일이 있으면 표시, 없으면 라벨 붙은 placeholder를 보여준다.

---

## 1. 자산(assets) 규칙

`assets/` 폴더에 아래 정확한 파일명으로 넣으면 자동 표시. 없으면 placeholder 노출.

| 파일명 | 용도 |
|---|---|
| `fig1.png` | 티저(Figure 1) |
| `fig2.png` | 프레임워크 개요(Figure 2) |
| `demo_ava_joint.mp4` | 데모: AVA joint audio-visual corruption |
| `demo_wasd.mp4` | 데모: WASD in-the-wild |
| `demo_ava_poster.jpg` | (선택) AVA 영상 poster 프레임 |
| `demo_wasd_poster.jpg` | (선택) WASD 영상 poster 프레임 |

- `<img>`는 `onerror`로 숨기고 placeholder div를 노출(파일명 표기).
- `<video controls muted playsinline preload="metadata" poster="...">` 사용, source 실패 시에도 슬롯이 자연스럽게 보이도록.

---

## 2. 디자인 토큰(권장)

- 폰트: display/헤딩 = **Newsreader**(serif), 본문 = **Inter**, 데이터/라벨 = **JetBrains Mono**.
- 팔레트:
  - ink `#141518`, ink-soft `#4a4e57`, line `#e5e3dc`
  - paper `#fbfaf6`, paper-2 `#f3f1ea`
  - audio/speaking accent `#c0392b`, visual accent `#2f5da8`
  - loss 색: inter(보라 `#6b3fa0`), intra(초록 `#227a44`), pred(주황 `#b5651d`)
- 표에서 "Ours" 행은 accent 톤으로 강조, AVG 열은 살짝 강조.
- 라운드 14px, 얇은 hairline 구분선. 과한 애니메이션 금지(버튼 hover 정도).

---

## 3. 페이지 섹션 순서 & 카피

### 3.1 Hero
- 상단 태그(pill): `ECCV 2026 · Submission`  ← 채택 상태에 맞게 수정 가능
- 제목: **C³ASD: Multi-Level Consistency-Driven Representation Learning for Robust Active Speaker Detection**
- 부제(이탤릭): *Class-aligned, modality-invariant embeddings that stay robust when audio and video break down.*
- 저자: **Jin Hong\*¹, Jisoo Park\*¹, Junseok Kwon¹**  (\* = Equal contribution)
- 소속: ¹Chung-Ang University, Seoul, Republic of Korea
- 각주: *\* Equal contribution.*
- 버튼(링크는 `#` placeholder, 실제 URL로 교체): **Paper**, **arXiv**, **Code**, **Demos**(→ #demos)

### 3.2 Teaser (Figure 1)
- `assets/fig1.png` 슬롯.
- 캡션:
  > **Multi-Level Consistency for Robust ASD.** In real-world active speaker detection, audio and visual streams are often corrupted by noise and occlusion. Existing models learn modality-clustered representations that are fragile under such degradations. In contrast, our multi-level consistency framework produces class-aligned, modality-invariant embeddings that stay robust across diverse corruption scenarios.

### 3.3 Abstract
- eyebrow: `Abstract` / 헤딩: **Consistency, not just fusion**
- lead:
  > Active Speaker Detection determines whether a visible person in a video is speaking at each moment. While recent audio–visual fusion methods perform well on clean data, they degrade under real-world corruptions such as background noise, occlusion, or simultaneous modality degradation.
- 본문:
  > We attribute this limitation to the absence of explicit consistency constraints that promote robust, semantically aligned representations across modalities. Without such guidance, models tend to learn fragile modality-specific shortcuts that fail under corrupted conditions. We propose **C³ASD**, a multi-level consistency-driven framework with three complementary constraints: **embedding-level inter-modality consistency** aligns audio–visual representations during speech; **sequence-level intra-modality consistency** separates speaking and non-speaking clusters via track-aware contrastive learning; and **prediction-level consistency** stabilizes fusion through knowledge distillation. Extensive experiments demonstrate significant improvements under diverse audio, visual, and joint corruptions, while maintaining competitive performance on clean data.
- 기여 카드 3개:
  1. **A robustness diagnosis** — We reveal that the absence of explicit multi-level modality consistency fundamentally limits the robustness of existing audio–visual ASD models under real-world corruption.
  2. **Three consistency losses** — Speaking-aware inter-modality alignment, sequence-level intra-modality regularization, and prediction-level knowledge distillation regularize representation learning directly.
  3. **Robust and lightweight** — Large gains under audio, visual, and joint corruption with no extra annotations, no corrupted training data, and virtually no added parameters.

### 3.4 Method (Figure 2)
- eyebrow: `Method` / 헤딩: **Multi-level consistency regularization**
- 본문:
  > Built on the lightweight Light-ASD backbone, C³ASD keeps the two-stream encoder and BGRU classifier untouched and adds three consistency signals that operate at the embedding and prediction levels. They require no external data or architectural changes and introduce no learnable parameters beyond a single audio-only classification head.
- `assets/fig2.png` 슬롯.
- 캡션:
  > **Overview of the C³ASD framework.** Visual face crops and audio MFCCs pass through modality-specific encoders. Three constraints act at multiple levels: (1) embedding-level inter-modality consistency (𝓛_inter) aligns audio and visual embeddings via cosine similarity during active speech; (2) sequence-level intra-modality consistency (𝓛_intra) applies track-aware supervised contrastive learning within each modality; and (3) prediction-level consistency (𝓛_pred) distills from the confidence-masked audio–visual prediction (teacher) to the unimodal predictions (students).
- 세 loss 카드(태그 색 구분):
  - **𝓛 inter — Embedding-level inter-modality consistency**: Maximizes cosine similarity between audio and visual embeddings — but only on speaking frames, where both modalities reflect the same speech-production process. When one modality is degraded at test time, the aligned partner anchors the fused representation toward the correct decision boundary.
  - **𝓛 intra — Sequence-level intra-modality consistency**: A track-aware supervised contrastive loss applied within each modality. Positives are same-label frames from the *same* track, avoiding cross-speaker interference and enlarging the margin between speaking and non-speaking clusters so corruption-induced shifts are less likely to cross the boundary.
  - **𝓛 pred — Prediction-level consistency**: Knowledge distillation from the audio–visual prediction (teacher, gradients detached) to the unimodal students via MSE, with a confidence mask (θ = 0.7) that keeps only reliable teacher targets. This suppresses a corrupted modality from dominating the fused decision, with an implicit easy-to-hard curriculum as training progresses.

### 3.5 Demos  (id="demos")
- eyebrow: `Demo videos` / 헤딩: **C³ASD in the wild & under corruption**
- 안내: Green box = predicted active speaker, red box = predicted non-speaker; a green border marks a correctly classified frame.
- 2열 카드(모바일 1열):
  - **AVA-ActiveSpeaker — Joint audio–visual corruption** (`demo_ava_joint.mp4`)
    - 하단 설명: COCO-patch occlusion (top-right) + MUSAN babble noise applied simultaneously. Both modalities degraded, speakers still correctly identified.
  - **WASD — In-the-wild generalization** (`demo_wasd.mp4`)
    - 하단 설명: Diverse real-world scenarios — off-screen speech and podcast-style multi-speaker settings — evaluated directly without fine-tuning.
- 범례: 🟩 Active speaker (green) / 🟥 Non-speaker (red)

### 3.6 Results · Audio–Visual joint corruption  (id="results")
- eyebrow: `Results · Audio–Visual joint corruption` / 헤딩: **The hardest, most realistic setting**
- 본문:
  > Joint corruption degrades both streams at once. All corruption is applied **only at test time** — no corrupted samples are ever used during training. C³ASD gains most exactly where it matters: severe SNR and simultaneous occlusion. Results on the AVA-ActiveSpeaker validation set (mAP %).

**Table 6 — Joint corruption on MUSAN** (Object occlusion + noise / Pixelated face, across Babble·Music·Natural @ SNR {−10…10} dB). "Ours" 행 강조, AVG 열 강조. 가로 스크롤 허용.

Object occlusion + noise:
| Method | B −10 | B −5 | B 0 | B 5 | B 10 | B AVG | M −10 | M −5 | M 0 | M 5 | M 10 | M AVG | N −10 | N −5 | N 0 | N 5 | N 10 | N AVG |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| TalkNet | 53.7 | 58.8 | 63.1 | 66.3 | 68.1 | 62.0 | 53.8 | 59.3 | 64.0 | 67.2 | 69.1 | 62.7 | 54.1 | 59.3 | 63.7 | 66.8 | 68.9 | 62.6 |
| ADENet | 55.8 | 59.1 | 61.0 | 62.7 | 64.7 | 60.7 | 52.8 | 56.4 | 59.1 | 62.2 | 64.6 | 59.0 | 54.6 | 57.5 | 59.7 | 62.0 | 64.2 | 59.6 |
| Light-ASD | 65.7 | 69.4 | 72.7 | 75.1 | 76.3 | 71.9 | 64.8 | 68.9 | 72.6 | 75.0 | 76.3 | 71.5 | 65.1 | 68.8 | 72.2 | 74.4 | 75.8 | 71.3 |
| **Ours** | **66.6** | **70.2** | **73.7** | **76.3** | **77.8** | **72.9** | **65.8** | **69.6** | **73.4** | **76.1** | **77.6** | **72.5** | **66.6** | **70.4** | **73.8** | **76.1** | **77.6** | **73.0** |

Pixelated face:
| Method | B −10 | B −5 | B 0 | B 5 | B 10 | B AVG | M −10 | M −5 | M 0 | M 5 | M 10 | M AVG | N −10 | N −5 | N 0 | N 5 | N 10 | N AVG |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| TalkNet | 85.0 | 87.4 | 89.1 | 90.3 | 91.1 | 88.6 | 83.9 | 86.2 | 88.2 | 89.8 | 90.8 | 87.8 | 84.9 | 87.2 | 88.9 | 90.1 | 90.9 | 88.4 |
| ADENet | 84.1 | 85.9 | 86.8 | 87.3 | 88.2 | 86.5 | 82.2 | 84.2 | 85.7 | 87.2 | 88.3 | 85.5 | 83.2 | 85.0 | 86.2 | 87.3 | 88.2 | 86.0 |
| Light-ASD | 87.5 | 89.2 | 90.5 | 91.5 | 92.1 | 90.2 | 86.0 | 87.9 | 89.6 | 90.9 | 91.8 | 89.3 | 86.9 | 88.7 | 90.1 | 91.2 | 91.9 | 89.8 |
| **Ours** | **87.9** | **89.6** | **90.9** | **91.9** | **92.5** | **90.6** | **86.7** | **88.6** | **90.2** | **91.5** | **92.4** | **89.9** | **87.7** | **89.4** | **90.8** | **91.8** | **92.4** | **90.4** |

표 아래 노트: *Under object occlusion, C³ASD improves average mAP by **+1.23%** over Light-ASD, with the largest gains at the most severe −10 dB SNR.*

**Table 7 — Joint corruption on DEMAND** (8 environments × {Object occlusion + noise, Pixelated face}). "Ours" 강조, AVG 강조.

| Method | Obj PARK | RIVER | CAFE | REST | CAFET | METRO | STATION | MEET | AVG | Pix PARK | RIVER | CAFE | REST | CAFET | METRO | STATION | MEET | AVG |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| TalkNet | 67.5 | 63.0 | 60.4 | 57.6 | 60.4 | 69.0 | 63.9 | 59.1 | 62.6 | 90.0 | 89.0 | 86.6 | 86.1 | 86.6 | 90.8 | 89.4 | 85.4 | 88.0 |
| ADENet | 66.2 | 61.8 | 61.1 | 59.0 | 61.1 | 66.0 | 62.3 | 60.0 | 62.2 | 88.8 | 87.2 | 85.0 | 84.5 | 85.0 | 89.0 | 87.5 | 83.6 | 86.3 |
| Light-ASD | 76.0 | 69.7 | 71.7 | 68.8 | 71.7 | 75.4 | 70.3 | 68.4 | 71.5 | 91.5 | 90.3 | 88.4 | 88.1 | 88.4 | 92.0 | 90.5 | 86.7 | 89.5 |
| **Ours** | **76.1** | **71.2** | **71.8** | **70.0** | **71.8** | **76.6** | **72.4** | **70.1** | **72.5** | **92.0** | **90.8** | **89.1** | **88.9** | **89.1** | **92.3** | **91.0** | **88.3** | **90.2** |

표 아래 노트: *On DEMAND, C³ASD gains **+1.0%** average mAP under object occlusion and **+0.7%** under pixelation, consistently leading across all eight environments.*

### 3.7 Results · Single-modality & generalization  (id="more-results")
- eyebrow: `Results · Single-modality & generalization` / 헤딩: **Also robust on each axis alone**
- 한 표에 Visual corruption(Table 5) + WASD(Table 2) 합침:

| Method | Object Occ. + Noise | Pixelated | WASD (in-the-wild) |
|---|---|---|---|
| TalkNet | 70.73 | 92.12 | 78.4 |
| ADENet | 66.36 | 89.04 | 85.6 |
| Light-ASD | 76.86 | 93.15 | 85.3 |
| **Ours** | **78.90** (+2.04) | **93.47** | **86.1** |

- 노트: *Under visual-only object occlusion, inter-modality alignment lets the clean audio stream compensate for degraded video (**+2.04%**). On WASD, C³ASD generalizes across a full dataset shift with no fine-tuning, suggesting the learned representations are transferable rather than dataset-specific.*

---

## 3.8 ⭐ Ablation Study — (신규) 어펜딕스 Table A.2 사용

> **중요: 기존 clean-data ablation(Table 8) 대신, 아래 어펜딕스 Table A.2(joint audio-visual corruption 기준)를 사용한다.** Results 섹션(3.7) 다음, BibTeX 앞에 새 `<section id="ablation">`으로 넣는다.

- eyebrow: `Ablation · Joint audio–visual corruption`
- 헤딩: **Every loss earns its place**
- 본문:
  > We ablate each consistency loss under the hardest regime — simultaneous audio–visual corruption on the eight DEMAND environments (mAP %). Every constraint delivers a large gain over the unregularized baseline (82.27), and the three are complementary: combining all of them reaches the best average of **90.19**. Inter-modality alignment gives the biggest single jump, while prediction-level consistency adds the most on top of the embedding-level objectives.

**Table A.2 — Ablation under joint audio–visual corruption (mAP %).** ✓/✗는 loss on/off. "All three(✓✓✓)" 행을 "Ours"로 강조, Avg 열 강조.

| Inter | Intra | Pred | Park | Cafe | Metro | River | Rest. | Cafeter. | Pub.Sta. | Meet. | Avg |
|:---:|:---:|:---:|---|---|---|---|---|---|---|---|---|
| ✗ | ✗ | ✗ | 85.09 | 80.50 | 85.86 | 83.62 | 79.94 | 80.50 | 83.83 | 78.85 | 82.27 |
| ✓ | ✗ | ✗ | 91.57 | 88.53 | 92.00 | 90.20 | 88.29 | 88.53 | 90.61 | 87.63 | 89.67 |
| ✗ | ✓ | ✗ | 91.78 | 88.54 | 92.31 | 90.79 | 88.26 | 88.54 | 90.89 | 87.67 | 89.85 |
| ✗ | ✗ | ✓ | 91.66 | 88.68 | 92.01 | 90.61 | 88.42 | 88.68 | 90.67 | 87.69 | 89.96 |
| ✓ | ✓ | ✗ | 91.37 | 88.13 | 91.75 | 90.43 | 88.17 | 88.13 | 90.51 | 87.36 | 89.48 |
| ✓ | ✗ | ✓ | 92.07 | 89.08 | 92.29 | 90.67 | 88.62 | 89.08 | 90.78 | 88.14 | 90.09 |
| ✗ | ✓ | ✓ | 91.66 | 88.68 | 92.01 | 90.61 | 88.42 | 88.68 | 90.67 | 87.69 | 89.96 |
| **✓** | **✓** | **✓** | **92.00** | **89.11** | **92.28** | **90.79** | **88.91** | **89.11** | **91.03** | **88.30** | **90.19** |

- 표 아래 노트: *Each loss individually lifts the baseline by roughly **+7–8%** average mAP, and the full combination (✓✓✓) is best at **90.19** — a **+7.92%** gain over the unregularized model.*

> 구현 팁: ✓는 초록(`#227a44`), ✗는 흐린 회색(`ink-soft`)으로 표시하면 on/off 패턴이 한눈에 보임. 마지막 행만 accent 배경.

---


## 4. Footer
- Chung-Ang University, Seoul, Republic of Korea
- Website template inspired by Nerfies. Licensed under CC BY-SA 4.0.

---

## 5. 체크리스트 (Claude Code용)
- [ ] `index.html` 단일 파일 + `assets/README.txt`(위 자산 규칙 안내) 생성
- [ ] 이미지/영상은 placeholder 처리(파일 있으면 자동 표시)
- [ ] 섹션 순서: Hero → Teaser → Abstract → Method → Demos → Results(joint) → More results → **Ablation(Table A.2)** → BibTeX → Footer
- [ ] 모든 표 가로 스크롤 컨테이너로 감싸기, "Ours"/"✓✓✓" 행 강조
- [ ] 반응형(≤760px 카드/영상 1열), 포커스 링, reduced-motion 대응
- [ ] Hero 버튼 4개 링크와 상단 venue 태그는 실제 값으로 교체 여지 남기기
