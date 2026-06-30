# NEBELOUN

> 영혼과 함께 균열을 안정화하며 몰려오는 적을 버티는 3D 뱀서라이크 슈팅 게임

[![Unity](https://img.shields.io/badge/Unity-2021.3.23f1-black?logo=unity)](https://unity.com/)
[![C#](https://img.shields.io/badge/C%23-Unity%20Gameplay-512BD4?logo=csharp)](https://learn.microsoft.com/dotnet/csharp/)
[![Platform](https://img.shields.io/badge/Platform-PC-blue)](#)
[![Genre](https://img.shields.io/badge/Genre-3D%20Vampire%20Survivors--like-red)](#)

## 1. 프로젝트 소개

**NEBELOUN**은 쿼터뷰 기반의 3D 뱀서라이크 액션 슈팅 게임입니다.  
플레이어는 계속해서 몰려오는 적과 보스 패턴을 피하며 성장하고, 전장에 생성되는 균열 지점으로 이동하는 **영혼(Spirit)**을 보호해야 합니다. 영혼이 균열을 안정화하는 동안 적들이 플레이어와 영혼을 압박하며, 제한 시간 안에 안정화에 실패하면 스테이지가 실패합니다.

| 항목 | 내용 |
| --- | --- |
| 게임명 | NEBELOUN |
| 장르 | 3D 뱀서라이크 / 액션 슈팅 / 디펜스 |
| 시점 | 쿼터뷰 기반 3인칭 |
| 플랫폼 | PC |
| 엔진 | Unity |
| 개발 언어 | C# |
| 개발 기간 | 3주 |
| 팀 규모 | 8명(기획 2명, 프로그래밍 6명) |
| 저장소 기준 Unity 버전 | 2021.3.23f1 |

## 2. 시연 영상

[![NEBELOUN Demo](https://img.youtube.com/vi/R_Unb4zZpRw/maxresdefault.jpg)](https://www.youtube.com/watch?v=R_Unb4zZpRw)

- YouTube: <https://www.youtube.com/watch?v=R_Unb4zZpRw>

## 3. 게임 목표

플레이어의 목표는 **영혼과 함께 균열을 안정화하고 스테이지를 클리어하는 것**입니다.

1. 전장에 균열이 생성됩니다.
2. 영혼이 균열 위치로 이동합니다.
3. 영혼이 균열에 도착하면 안정화를 시작합니다.
4. 플레이어는 안정화가 끝날 때까지 영혼과 자신을 방어합니다.
5. 균열 안정화에 성공하면 다음 전투 흐름으로 넘어갑니다.

## 4. 주요 기능

### 전투 및 플레이 루프

- `Main -> Stage -> Character -> InGameScene`으로 이어지는 씬 흐름
- 자동 공격 기반 전투와 이동, 대시, 캐릭터 스킬 조합
- 경험치 획득, 레벨업, 증강 선택으로 반복 성장 루프 구성
- 스테이지 진행에 따라 강화되는 웨이브 기반 적 스폰

### 데이터 기반 웨이브 시스템

- `Assets/Resources/Data/Stage1.json`
- `Assets/Resources/Data/Stage2.json`
- `Assets/Resources/Data/Stage3.json`
- `Assets/Resources/Data/Stage_collapse.json`

스테이지별 웨이브 데이터는 JSON으로 관리됩니다. 각 웨이브는 스폰 딜레이, 지속 시간, 적 종류, 스폰 가중치를 포함하며, 코드 수정 없이 데이터 조정으로 난이도 곡선을 변경할 수 있습니다.

### 균열 안정화 시스템

- `CollapseZone`이 일정 시간 동안 균열 안정화 타이머와 진행도를 관리
- `Spirit`은 균열이 생성되면 해당 위치로 이동하고 안정화 상태로 전환
- 영혼이 안정화 중일 때 적의 타겟이 플레이어 또는 영혼으로 전환되어 방어 압박 형성
- 안정화 성공 시 영혼 버프를 갱신하고 클리어 카운트를 증가

### 적 AI와 보스 패턴

- 일반 적은 플레이어와 영혼 상태를 기반으로 타겟을 선택
- 근접, 원거리, 보스형 적을 분리하여 공격 패턴 구현
- `Nebeloun`, `Giant`, `Blood` 등 보스/특수 패턴 구현
- NavMeshAgent, Rigidbody, 애니메이션 레이어를 활용한 이동 및 패턴 연출

### 증강 시스템

- 레벨업 시 랜덤 증강 선택 UI 제공
- 공격형, 보조형, 스탯형 증강으로 플레이 스타일 확장
- 이벤트 기반 구조로 공격, 피격, 업데이트, 적 생성 시점에 증강 효과 적용
- 예시:
  - 분열 사격
  - 스플래시 사격
  - 넉백 사격
  - 에너지 필드
  - 독 장판
  - 보호막
  - 메테오
  - 핵폭발

### 오브젝트 풀링

- 탄환, 적, 경험치, 이펙트, 균열 존 등 반복 생성 객체를 풀링
- 풀 잔여 개수가 부족하면 추가 할당
- `IPoolable` 인터페이스로 생성, 활성화, 반환 흐름 분리

### UI / HUD

- 킬 카운트, 레벨, 경험치, 스킬 쿨타임 표시
- 균열 생성 시간, 안정화 제한 시간, 안정화 진행도 표시
- 레벨업 증강 선택 UI와 일시정지 UI 구성
- `Tab` 키로 보유 증강 아이콘 패널 표시

### 사운드

- `SoundManager` 싱글톤 구조로 배경음과 효과음 관리
- 씬 전환에 따른 BGM 재생
- 플레이어 위치 기반 3D 이동 사운드 볼륨 조절
- 효과음 캐싱으로 반복 로드 최소화

## 5. 조작법

| 입력 | 동작 |
| --- | --- |
| `WASD` / 방향키 | 이동 |
| 마우스 | 조준 방향 보정 |
| `Space` | 대시 |
| `Q` | 캐릭터 스킬 |
| `Tab` | 증강 아이콘 패널 표시/숨김 |
| `Esc` | 일시정지 |

## 6. 기술 스택

| 구분 | 기술 |
| --- | --- |
| Engine | Unity |
| Language | C# |
| Camera | Cinemachine |
| AI / Navigation | NavMeshAgent |
| UI | Unity UGUI, TextMeshPro |
| Data | JSON, CSV, Resources |
| Rendering / Effects | URP/HDRP 패키지, Post Processing, Visual Effect Graph |
| Optimization | Object Pooling, Occlusion Culling |

## 7. 프로젝트 구조

```text
Assets
├─ 01. Scenes
│  ├─ Main.unity
│  ├─ Stage.unity
│  ├─ Character.unity
│  └─ InGameScene.unity
├─ 02. Scripts
│  ├─ Augmentation
│  ├─ Entities
│  │  ├─ Playable
│  │  └─ Enemies
│  ├─ Object Pooling
│  ├─ StatusEffect
│  ├─ UI
│  ├─ Sound
│  ├─ GameManager.cs
│  └─ Spawner.cs
└─ Resources
   └─ Data
      ├─ Stage1.json
      ├─ Stage2.json
      ├─ Stage3.json
      ├─ Stage_collapse.json
      ├─ Character_Enemy_Boss_Stat_Chart.CSV
      ├─ Character_Level_Chart.CSV
      ├─ Reinforce_Chart.CSV
      └─ Aug_Explanation_Chart.csv
```

## 8. 핵심 구현 상세

### GameManager

- 씬 전환과 비동기 로딩 패널 관리
- 캐릭터 선택 정보에 따른 플레이어 프리팹 생성
- CSV 기반 캐릭터, 레벨, 증강 데이터 로드
- 씬별 배경음 및 전투 시작 효과음 재생

### Spawner

- JSON 스테이지 데이터를 파싱하여 웨이브 구성
- 웨이브별 스폰 딜레이, 지속 시간, 스폰 대상, 가중치 계산
- 카메라 뷰와 맵 영역을 기준으로 적 스폰 범위 갱신
- 균열 존 스폰과 일반 적 스폰 흐름 분리

### Entity / Playable / Enemy

- `Entity`를 기반으로 HP, 피해, 회복, 상태 효과 처리
- `PlayableCtrl`에서 이동, 자동 공격, 대시, 스킬, 레벨업 처리
- `EnemyCtrl`에서 영혼 상태에 따른 타겟 전환과 공격 처리

### Augmentation

- `ON_START`, `ON_UPDATE`, `ON_ATTACK`, `ON_HIT`, `ON_DAMAGE`, `ON_SPAWN_ENEMY`, `ON_UPDATE_ENEMY` 등 이벤트 타이밍 정의
- 플레이어 이벤트에 증강 효과를 구독시켜 기능 확장
- 신규 증강 추가 시 공통 인터페이스와 이벤트 구조를 재사용 가능

### ObjectPool

- `ObjectType` enum으로 풀링 대상 관리
- 초기 풀 생성 후 필요 시 동적 추가 할당
- `GetObject`, `ReturnObject`, `IPoolable.OnActivate` 흐름으로 재사용 객체 초기화

## 9. 트러블슈팅 및 개선 경험

### 전투 컨트롤러 구조 정리

**문제**

- 스폰 실행, 웨이브 전환, 시간 계산, UI 상태 변경이 한 흐름에 섞이면서 유지보수가 어려웠습니다.
- 웨이브 전환 시 UI 표시 상태와 실제 전투 상태가 어긋나는 문제가 발생했습니다.

**해결**

- 전투 세션 흐름은 `GameManager` 중심으로 관리하고, `Spawner`는 스폰 실행에 집중하도록 역할을 분리했습니다.
- 웨이브 종료 시점에 균열/존 상태 변경 규칙을 고정했습니다.
- UI는 전투 로직을 직접 판단하지 않고 상태 변화에 맞춰 표시를 갱신하도록 정리했습니다.

**결과**

- 전투 밀도와 플레이 리듬이 안정화되었습니다.
- 스테이지 데이터 수정만으로 난이도와 웨이브 흐름을 조정하기 쉬워졌습니다.

## 10. 팀 및 담당 역할

| 이름 | 역할 | 담당 |
| --- | --- | --- |
| 모규찬 | PD / 기획 | 전투, 적 몬스터, 보스 기획 |
| 박승원 | PM / 기획 | 메인 기획, 콘셉트, 캐릭터 |
| 김태욱 | Lead Programmer | 캐릭터 능력치, 증강 시스템, Boss 개발 |
| 허한결 | Programmer | 오브젝트 풀링, 플레이어블 캐릭터 |
| 김영웅 | Programmer | 적 생성 로직, 오클루전 컬링 |
| 박새한 | Programmer | UI 개발, 시나리오 |
| 한동욱 | Programmer | 증강 개발 |
| 한승엽 | Programmer | 적 AI, UI 로직 시스템, 사운드 구현 |

## 11. 실행 방법

```bash
git clone https://github.com/Yeop-Team/VamSurLike.git
```

1. Unity Hub에서 프로젝트 폴더를 엽니다.
2. 저장소 기준 Unity 버전인 **2021.3.23f1** 또는 호환 가능한 Unity 버전을 사용합니다.
3. 패키지 임포트가 끝날 때까지 기다립니다.
4. `Assets/01. Scenes/Main.unity`를 열고 실행합니다.
5. 빌드 시 `ProjectSettings/EditorBuildSettings.asset` 기준으로 다음 씬이 포함되어 있는지 확인합니다.
   - `Main`
   - `Stage`
   - `Character`
   - `InGameScene`

## 12. 참고

- 본 저장소에는 외부 에셋 및 패키지가 포함되어 있을 수 있으므로 재배포/상업적 이용 전 각 에셋의 라이선스를 확인해야 합니다.
- 현재 저장소의 기존 README는 간단한 임시 문구만 포함되어 있어, 본 문서는 프로젝트 설명과 포트폴리오 문서를 기반으로 재구성한 GitHub README 초안입니다.
