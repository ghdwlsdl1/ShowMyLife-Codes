# ShowMyLife - Unity Core Systems, Cinematic Tools & Stage Event Triggers

Unity 기반 ShowMyLife 프로젝트에서 직접 제작한 게임 핵심 시스템, 시네마틱 연출 모듈, 스테이지 이벤트 트리거, 에디터 유틸리티 모음입니다.  
시네머신 조작과 연출 전환, 카메라 움직임, 오브젝트 활성화, 저장 시스템까지 플레이 흐름과 개발 편의성을 모두 고려하여 작성되었습니다.

---

## 포함된 스크립트

### 게임 시스템
| 파일명 | 설명 |
|--------|------|
| ObjectPool.cs | 오브젝트 풀링 시스템 (Resources 기반, 씬 전환 유지, 큐 관리) |
| SaveLoader.cs | JSON 직렬화를 통한 데이터 저장/로드 유틸리티 |
| SavePoint.cs | 트리거 기반 세이브 포인트 (위치 저장, 범위 표시) |
| SaveData.cs | 저장 데이터 클래스 (좌표 및 SaveId 관리) |
| SaveManager.cs | 세이브/로드 관리 매니저 (GameManager 연동) |
| CameraManager.cs | Cinemachine 기반 플레이어/연출 카메라 전환 및 감도 관리 |

---

### 시네마틱 & 연출 시스템
| 파일명 | 설명 |
|--------|------|
| ThemeCameraController.cs | 테마 카메라 제어 (위치/회전/FOV 제어, 이동/줌 연출) |
| TimeEffectManager.cs | 슬로우모션 등 시간 배율 연출 |
| PostProcessingManager.cs | 색상 필터/후처리 효과 전환 |
| EmotionDirector.cs | 연출 총괄 매니저 (Sweep, 줌, 이동, 회전, 슬로우모션 등 공용 기능) |
| StageTransitionTrigger.cs | 오브젝트 활성/비활성 전환 트리거 |
| ColorZoneTrigger.cs | 영역 진입 시 색상 필터 적용 |
| DollyZoomTrigger.cs | 돌리 줌 연출 트리거 |

---

### Stage별 연출 트리거
| 파일명 | 설명 |
|--------|------|
| EnterStage.cs | 첫 진입 시 시네마틱 (기상 연출, 응시 연출) |
| EnterStage1_1.cs | 좌우 훑기 → 타겟 포커스 연출 |
| EnterStage1_2.cs | 하늘 훑기 → 타겟 응시 후 발판 활성화 |
| EnterStage2_1.cs | 오브젝트(책) 애니메이션 + 발판 활성화 |
| EnterStage2_2.cs | 줌인 → 훑기 연출 |
| EnterStage3_1_1.cs | 점진적 슬로우모션 |
| EnterStage3_1_2.cs | 멀티 타겟 이동/회전 → 시점 보정 |
| EnterStage4_1.cs | 연속 타겟 회전 연출 |
| EnterStageDolly.cs | 돌리 카메라 이동 연출 |

---

### 에디터 유틸리티
| 파일명 | 설명 |
|--------|------|
| URPMaterialConverter.cs | HDRP/Standard/Unreal 등 머티리얼을 URP Lit으로 자동 변환 및 텍스처 매핑 |
| DebugWindow.cs | 런타임 디버그 허브 (세이브 위치 이동, 저장 데이터 확인/삭제) |

---

## Stage별 연출 흐름

### Stage 1
1. EnterStage1_1: 카메라가 좌우로 훑으며 주변을 보여줌 → 지정된 타겟을 응시
2. EnterStage1_2: 하늘을 바라보다가 타겟으로 시선 이동 → 발판 활성화

### Stage 2
1. EnterStage2_1: 책 오브젝트 회전 애니메이션 → 발판 생성
2. EnterStage2_2: FOV 줌인 후 Sweep 연출 진행

### Stage 3
1. EnterStage3_1_1: 진입 후 점점 슬로우모션으로 전환
2. EnterStage3_1_2: 카메라가 여러 타겟을 순차적으로 이동/회전 → 플레이어 시점 보정

### Stage 4
1. EnterStage4_1: 지정된 타겟을 순차 회전
2. EnterStageDolly: 돌리 카트 경로를 따라 카메라 이동

---

## 주요 기능 설명

### 1. Object Pool & Save System
- 오브젝트를 동적으로 생성/반환하여 성능 최적화
- 씬 전환 시에도 유지(DontDestroyOnLoad)
- JSON 저장/로드
- SavePoint와 연동해 위치 + 상태 저장
- Gizmos로 세이브 범위 시각화

### 2. Cinematic System
- 카메라 전환 (플레이어 ↔ 테마 카메라)
- Sweep, Dolly Zoom, Move Camera, FOV Zoom, Rotate Look
- 시간 연출(TimeScale 변경)
- 색상 필터(PostProcessingManager)와 연동
- 연출 중 플레이어 조작 제한/복원

### 3. Stage Event System
- 각 스테이지 진입 시 전용 시네마틱 실행
- 오브젝트 활성화/애니메이션 실행
- 타겟 이동/회전과 시점 보정
- EmotionDirector의 공용 함수만을 사용해 유지보수성 강화

### 4. Editor Tools
- 머티리얼 자동 변환 (URP Lit)
- 알베도/노멀/메탈릭/마스크 자동 매핑
  
- 런타임 DebugWindow로 세이브 데이터 확인, 이동, 삭제 가능

---

## 코드 아키텍처

- 모듈화: Save, Camera, PostProcessing, TimeEffect, Cinematic을 각각 독립 모듈로 관리
- EmotionDirector 중심 구조: 모든 연출은 EmotionDirector의 공용 메서드 호출로 처리
- 이벤트 기반: Stage별 트리거는 OnTriggerEnter로 연출 시작
- 데이터 분리: 저장 데이터(SaveData)와 로직 분리
- 에디터 확장: URPMaterialConverter로 아티스트 작업 효율화

---

## 기술 스택
- Unity 202X
- C#
- Cinemachine
- DG.Tweening
- Newtonsoft.Json
- URP + Post Processing Stack

---

## 라이선스
MIT License
