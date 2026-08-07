# MATE: ROS 협동로봇 미션 오케스트레이션 프로젝트

MATE는 작업자와 협동로봇이 같은 공간에서 안전하게 미션을 수행하도록 돕는 ROS 2 기반 로봇 제어 프로젝트입니다.  
가상의 스마트팩토리 셀을 배경으로, 협동로봇 팔이 부품을 인식하고 집어 올린 뒤 작업자 동선과 충돌하지 않도록 경로를 계획하며 조립 스테이션까지 이송하는 시나리오를 구현합니다.

## 프로젝트 목표

- ROS 2 노드 기반의 협동로봇 미션 제어 구조 설계
- 카메라, 힘/토크 센서, 안전 영역 데이터를 활용한 작업 환경 인식
- MoveIt 2 기반 경로 계획 및 충돌 회피
- 작업자 접근 상황에 따른 속도 제한, 정지, 재개 로직 구현
- 미션 상태를 실시간으로 확인할 수 있는 간단한 대시보드 연동

## 핵심 시나리오

1. 비전 노드가 컨베이어 위 부품 위치를 감지합니다.
2. 미션 매니저가 작업 우선순위와 로봇 상태를 확인합니다.
3. MoveIt 2 플래너가 안전 경로를 생성합니다.
4. 협동로봇이 부품을 픽업하고 조립 지점으로 이동합니다.
5. 작업자가 안전 영역에 접근하면 로봇이 자동으로 감속하거나 정지합니다.
6. 위험 상황이 해제되면 미션을 이어서 수행합니다.

## 시스템 구성

```text
mate/
├── mission_manager      # 미션 상태 관리 및 작업 순서 결정
├── perception_node      # 카메라 기반 부품 인식
├── safety_monitor       # 작업자 접근, 비상정지, 속도 제한 판단
├── motion_planner       # MoveIt 2 경로 계획 요청
├── robot_interface      # 협동로봇 드라이버 연동 계층
└── dashboard_bridge     # 미션 상태 시각화 API 연동
```

## 기술 스택

- ROS 2 Humble
- Python 3.10
- MoveIt 2
- OpenCV
- Gazebo / RViz2
- SQLite 기반 미션 로그 저장

## 주요 기능

- 미션 큐 등록 및 실행 상태 추적
- 픽앤플레이스 작업 시뮬레이션
- 실시간 안전 영역 모니터링
- 로봇 동작 로그 및 실패 원인 기록
- RViz2를 통한 로봇 자세와 경로 시각화

## 실행 예시

```bash
# ROS 2 워크스페이스 빌드
colcon build --symlink-install

# 환경 설정
source install/setup.bash

# 시뮬레이션 환경 실행
ros2 launch mate_bringup simulation.launch.py

# 미션 매니저 실행
ros2 run mate_mission mission_manager
```

## 미션 데이터 예시

```json
{
  "mission_id": "ASM-2026-0807-001",
  "part_type": "gear_housing",
  "source": "conveyor_a",
  "target": "assembly_station_2",
  "priority": "high",
  "safety_mode": "collaborative"
}
```

## 기대 효과

- 반복 조립 공정의 자동화 수준 향상
- 작업자와 로봇이 함께 일하는 셀 환경의 안전성 검증
- ROS 2 기반 협동로봇 제어 구조 학습
- 실제 로봇 도입 전 시뮬레이션 기반 미션 검증

## 향후 계획

- 디지털 트윈 기반 생산 셀 시각화
- YOLO 기반 부품 검출 모델 연동
- 작업자 손동작 인식 기반 미션 승인 기능
- 실제 협동로봇 드라이버와의 통합 테스트

## 라이선스

이 프로젝트는 학습 및 데모 목적의 가상 프로젝트입니다.
