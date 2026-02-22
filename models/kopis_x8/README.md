# Kopis X8 Cinelifter 5" 시뮬레이션

## 1. 필요한 자료 찾는 법 (어떤 드론이든 공통)

### 1) 제조사 공식 문서
- **문서 포털**: 제품명 + `documentation`, `manual`, `wiki`, `docs`
  - 예: `Holybro Kopis manual` → [docs.holybro.com/company/user-manuals/kopis](https://docs.holybro.com/company/user-manuals/kopis)
- **제품 페이지**: `support`, `downloads`, `resources` 탭에서 Assembly Guide, Quick Start, 3D Print 파일 확인
- **검색 예시**:
  - `"제품명" assembly guide PDF`
  - `"제품명" CAD STL dimensions`
  - `"제품명" specs wheelbase arm length`

### 2) 치수·도면이 나오는 자료
- **Assembly Guide / 조립 가이드 PDF**: 암 길이, 로터 간격, 프레임 치수 도면
- **Spare Part List**: 부품 번호·치수
- **CAD/STL**: 제조사가 제공하면 `STL`, `STEP`, `RAR`/`ZIP` 검색

### 3) Holybro Kopis X8 5" 직접 링크
- [Kopis 매뉴얼 목록](https://docs.holybro.com/company/user-manuals/kopis)
  - **KopisX8-5inch-Ducted-Assembly-Guide.pdf** – 5" 조립/치수 참고
  - **KopisX8-5inch-TPU-3D-Print.rar** – 5" 3D 프린트 STL
  - **KopisX8_Cinelifter_5in_Duct.zip** – Cinelifter 5" 덕트 관련
- STL/메쉬는 Gazebo `meshes/`에 넣고 SDF에서 `model://kopis_x8_base/meshes/...` 로 참조

### 4) 시뮬용으로 쓸 때
- **치수**: Assembly Guide로 로터 위치(m), 무게(kg) 확인 → SDF `pose`, `mass`, `inertia` 및 PX4 `CA_ROTOR*_PX/PY` 에 반영
- **외형**: STL → Blender/MeshLab로 단순화 후 DAE/OBJ로 내보내서 `meshes/` 에 추가

---

## 2. 나뒹굴음 / 난리(튜밍) 대처

- **CA 좌표계**: PX4는 body **FRD** (X 앞, Y 오른쪽, Z 아래). Gazebo SDF는 **FLU** (Y 왼쪽).  
  SDF에서 (x, y) = 앞왼쪽이면 PX4에서는 `CA_ROTOR*_PX = x`, `CA_ROTOR*_PY = -y` 로 넣어야 함.
- **로터 순서**: SDF의 `rotor_0` 위치와 에어프레임의 `CA_ROTOR0` 위치가 **같은 몸체 위치**인지 확인.
- **회전 방향(KM)**: 같은 암의 두 로터는 반대 방향(ccw/cw, KM ±0.05)으로 설정해 토크 상쇄.
- **무게/관성**: `kopis_x8_base/model.sdf` 의 `mass`, `inertia` 를 실기와 비슷하게 맞추기.
- **이득**: `MPC_THR_HOVER`(호버 스로틀), `MC_ROLL_P` 등은 무거우면 조금 낮추는 쪽으로 튜닝.
- **빙글빙글 도는(요 불안정)**: PX4에서 KM > 0 = CCW 방향(반력 +Z). Gazebo `turningDirection` ccw 모터는 `CA_ROTOR*_KM 0.05`, cw는 `-0.05`로 맞추기. 같은 암의 두 로터는 반대 부호로 토크 상쇄.
- 여전히 요가 흔들리면 `MC_YAW_P`를 약간 낮추거나, `MC_YAWRATE_P` 등을 조정해 보기.

이 README와 에어프레임의 CA PY·KM 부호 수정으로, 나뒹굴지 않고 안정적으로 뜨는지 먼저 확인하는 것을 권장합니다.
