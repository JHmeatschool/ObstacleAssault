# 🚀 Obstacle Assault (Unreal Engine 5.6 C++)

**Udemy의 'Unreal Engine 5 C++ Developer' 과정을 기반으로 제작된 물리 기반 장애물 게임 프로젝트입니다.**
단순한 액터 이동 구현을 넘어, 프레임 독립적인 물리 계산과 벡터 수학을 활용한 위치 보정 로직을 실전 프로젝트에 적용하는 데 집중했습니다.

---

## 📺 Project Showcase
- **Demonstration Video:** [https://youtu.be/sMnZqFeyU7s]

---

## 🛠 Core Technical Implementations (C++)

### 🎯 Key Source Code: `MovingPlatform`
이 프로젝트의 핵심인 `AMovingPlatform` 클래스는 하드웨어 성능에 관계없이 일정한 움직임을 보장하며, 누적되는 위치 오차를 수학적으로 해결합니다.

#### **1. 정밀한 위치 보정 및 벡터 연산 (Overshoot Correction)**
단순히 거리를 체크하여 방향을 반전시키면, 매 프레임의 이동량($PlatformVelocity \times DeltaTime$)만큼 목표 지점을 지나치는 **Overshoot** 문제가 발생합니다. 이를 해결하기 위해 다음 로직을 구현했습니다.
- **Normalized Vector 활용:** `PlatformVelocity.GetSafeNormal()`을 통해 이동 방향 벡터를 정규화하여 정확한 보정 위치를 산출합니다.
- **누적 오차 제거:** `StartLocation + MoveDirection * MoveDistance` 공식을 사용하여, 오버슈트가 발생한 즉시 액터를 정확한 목표 지점에 재배치하고 `StartLocation`을 갱신함으로써 플랫폼이 경로를 이탈하는 것을 방지했습니다.

#### **2. 에디터 최적화 및 디버깅 (Editor Workflow)**
- **`UPROPERTY` 활용:** `PlatformVelocity`, `MoveDistance`, `RotationVelocity` 등을 에디터에 노출하여 코드 수정 없이 실시간으로 파라미터를 조정할 수 있게 설계했습니다.
- **`UE_LOG` 시스템:** `*MyStringParam`을 통한 `FString` 디퍼런싱 등 언리얼 전용 로그 시스템을 활용하여 런타임 환경에서 오버슈트 발생량과 리턴 값을 추적했습니다.

#### **3. 효율적인 물리 계산**
- **`FVector::Dist`:** 두 벡터 사이의 유클리드 거리를 계산하여 이동 거리를 산출하는 로직을 `GetDistanceMoved()` 함수로 캡슐화했습니다.
- **`AddActorLocalRotation`:** 전역 좌표계가 아닌 액터의 로컬 좌표계를 기준으로 회전 속도를 적용하여 복합적인 장애물 움직임을 구현했습니다.

---

## 📦 Assets & Resources
- **Logic:** 모든 게임 플레이 로직 및 액터 제어는 C++로 직접 구현되었습니다.
- **Environment Assets:** 시각적 퀄리티를 위해 Unreal Marketplace의 외부 에셋을 활용하였습니다.
  - *Asian Village, Construction Volume, Survival Character* 등 (저장소 최적화를 위해 `.gitignore` 처리됨)

---

## 💡 Problem Solving (Troubleshooting)

**Problem:** 3.1GB 이상의 대용량 프로젝트 업로드 시 `HTTP 500 RPC failed` 발생.

**Solution 방법:**
1. **Git LFS 도입:** `.uasset`, `.umap` 등 대용량 바이너리 파일을 별도로 관리하여 전송 안정성을 확보했습니다.
2. **Data Filtering:** `.gitignore`를 정밀하게 구성하여 무거운 외부 에셋 폴더와 100MB 초과 영상 파일을 분리함으로써 리포지토리 용량을 최적화했습니다.

---

## 🔧 Tech Stack
- **Engine:** Unreal Engine 5.6
- **Language:** C++ (Visual Studio 2022)
- **Version Control:** Git, GitHub, Git LFS
