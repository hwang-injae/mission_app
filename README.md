# README.md 내 컴퓨터와 남 컴퓨터
# ME: TurtleBot Mission Lab
# MATE: ROS Robot Mission Lab

MATE는 ROS 기반 로봇 미션을 실험하는 가상 프로젝트입니다.  
협동로봇 팔을 활용한 스마트팩토리 조립 셀과 TurtleBot 기반 실내 자율주행 시나리오를 함께 다루며, 로봇이 환경을 인식하고 안전하게 판단하며 미션을 수행하는 전체 흐름을 설계합니다.

## 프로젝트 목표

- ROS 기반 로봇 미션 제어 구조 설계
- 협동로봇과 모바일 로봇의 미션 수행 흐름 비교 및 실험
- 센서 데이터, 경로 계획, 안전 제어, 로그 저장을 포함한 통합 구조 구성
- Gazebo와 RViz를 활용한 시뮬레이션 중심 검증
- 실제 로봇 플랫폼으로 확장 가능한 패키지 구조 학습

## 시나리오 1: 협동로봇 스마트팩토리 셀

작업자와 협동로봇이 같은 공간에서 안전하게 작업하는 조립 셀을 가정합니다. 협동로봇 팔은 부품을 인식하고 집어 올린 뒤, 작업자 동선과 충돌하지 않도록 경로를 계획하며 조립 스테이션까지 이송합니다.

### 핵심 흐름

1. 비전 노드가 컨베이어 위 부품 위치를 감지합니다.
2. 미션 매니저가 작업 우선순위와 로봇 상태를 확인합니다.
3. MoveIt 2 플래너가 안전 경로를 생성합니다.
4. 협동로봇이 부품을 픽업하고 조립 지점으로 이동합니다.
5. 작업자가 안전 영역에 접근하면 로봇이 자동으로 감속하거나 정지합니다.
6. 위험 상황이 해제되면 미션을 이어서 수행합니다.

### 주요 기능

- 미션 큐 등록 및 실행 상태 추적
- 픽앤플레이스 작업 시뮬레이션
- 실시간 안전 영역 모니터링
- 로봇 동작 로그 및 실패 원인 기록
- RViz2를 통한 로봇 자세와 경로 시각화

## 시나리오 2: TurtleBot Mission Lab

TurtleBot을 활용해 실내 자율주행, 장애물 회피, 목표 지점 이동, 웨이포인트 순회 미션을 실험합니다. Gazebo 시뮬레이션과 실제 TurtleBot 플랫폼을 모두 고려하여, 작은 서비스 로봇의 기본 동작 구조를 구성합니다.

### 핵심 흐름

1. 로봇이 초기 위치에서 출발합니다.
2. LiDAR와 odometry 데이터를 이용해 주변 환경을 인식합니다.
3. SLAM으로 실내 지도를 생성하거나 저장된 지도를 불러옵니다.
4. Navigation Stack 또는 Nav2가 목표 지점까지의 경로를 계획합니다.
5. 이동 중 장애물이 감지되면 local planner가 회피 경로를 생성합니다.
6. 모든 웨이포인트를 방문한 뒤 시작 지점으로 복귀합니다.

### 주요 기능

- LiDAR 기반 실내 지도 작성
- 지정 좌표 기반 자율주행
- 동적 장애물 감지 및 회피
- 다중 웨이포인트 순회 미션
- RViz를 통한 센서, 지도, 경로 시각화

## 시스템 구성

```text
mate/
├── mission_manager        # 공통 미션 상태 관리 및 작업 순서 결정
├── cobot_cell             # 협동로봇 픽앤플레이스 시나리오
│   ├── perception_node    # 카메라 기반 부품 인식
│   ├── safety_monitor     # 작업자 접근, 비상정지, 속도 제한 판단
│   ├── motion_planner     # MoveIt 2 경로 계획 요청
│   └── robot_interface    # 협동로봇 드라이버 연동 계층
├── turtlebot_lab          # TurtleBot 자율주행 시나리오
│   ├── slam_node          # 지도 작성 및 위치 추정
│   ├── navigation_node    # 목표 지점 이동 및 경로 추종
│   └── waypoint_mission   # 다중 웨이포인트 미션 수행
├── dashboard_bridge       # 미션 상태 시각화 API 연동
└── mission_log            # SQLite 기반 미션 로그 저장
```

## 기술 스택

- ROS 2 Humble
- ROS Noetic
- Python 3.10
- MoveIt 2
- OpenCV
- Gazebo
- RViz / RViz2
- SLAM Toolbox 또는 GMapping
- Navigation Stack / Nav2
- SQLite 기반 미션 로그 저장

## 실행 예시

협동로봇 시뮬레이션을 실행합니다.

```bash
colcon build --symlink-install
source install/setup.bash
ros2 launch mate_bringup cobot_simulation.launch.py
ros2 run mate_mission mission_manager
```

TurtleBot Gazebo 시뮬레이션을 실행합니다.

```bash
roslaunch turtlebot3_gazebo turtlebot3_world.launch
```

SLAM 노드를 실행합니다.

```bash
roslaunch turtlebot3_slam turtlebot3_slam.launch slam_methods:=gmapping
```

Navigation을 실행합니다.

```bash
roslaunch turtlebot3_navigation turtlebot3_navigation.launch map_file:=$HOME/map.yaml
```

웨이포인트 미션 플래너를 실행합니다.

```bash
rosrun mission_app waypoint_mission.py
```

## 미션 데이터 예시

```json
{
  "mission_id": "MATE-2026-0807-001",
  "robot_type": "collaborative_robot",
  "task": "pick_and_place",
  "source": "conveyor_a",
  "target": "assembly_station_2",
  "priority": "high",
  "safety_mode": "collaborative"
}
```

```json
{
  "mission_id": "MATE-2026-0807-002",
  "robot_type": "turtlebot",
  "task": "waypoint_navigation",
  "map": "indoor_lab.yaml",
  "waypoints": ["A1", "B2", "C3"],
  "return_to_home": true
}
```

## 기대 효과

- ROS 기반 로봇 미션 설계 경험 확보
- 협동로봇과 모바일 로봇의 제어 구조 비교 학습
- 반복 가능한 Gazebo 테스트 환경 구축
- 작업자 안전과 로봇 자율성 사이의 제어 전략 검증
- 실제 로봇 적용을 위한 기본 패키지 구조 확보

## 향후 계획

- 디지털 트윈 기반 생산 셀 시각화
- YOLO 기반 부품 및 장애물 검출 모델 연동
- 작업자 손동작 인식 기반 미션 승인 기능
- 다중 로봇 협업 미션 확장
- 웹 대시보드를 통한 실시간 상태 모니터링
- 배터리 상태 기반 자동 충전 스테이션 복귀

## 라이선스

이 프로젝트는 학습 및 데모 목적의 가상 프로젝트입니다.
