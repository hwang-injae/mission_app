# TurtleBot Mission Lab

ROS 기반 터틀봇을 활용해 실내 자율주행, 장애물 회피, 목표 지점 이동, 미션 수행 시나리오를 실험하는 가상 프로젝트입니다. Gazebo 시뮬레이션과 실제 TurtleBot 플랫폼을 모두 고려하여, 로봇이 공간을 인식하고 판단하며 안전하게 이동하는 전체 흐름을 구현하는 것을 목표로 합니다.

## 프로젝트 개요

TurtleBot Mission Lab은 ROS Navigation Stack, SLAM, 센서 데이터 처리, 미션 플래너를 조합해 작은 서비스 로봇의 동작을 구성합니다. 로봇은 주어진 실내 환경에서 지도를 생성하고, 사용자가 지정한 목적지까지 이동하며, 이동 중 감지되는 장애물을 회피합니다.

주요 활용 시나리오는 다음과 같습니다.

- 실내 공간 지도 작성 및 저장
- 지정 좌표 기반 자율주행
- LiDAR 기반 장애물 감지 및 회피
- 다중 웨이포인트 순회 미션
- Gazebo 환경에서의 반복 테스트
- RViz를 통한 센서, 지도, 경로 시각화

## 주요 기능

- **SLAM Mapping**: TurtleBot의 LiDAR 데이터를 기반으로 실내 지도를 생성합니다.
- **Autonomous Navigation**: 생성된 지도 위에서 목표 지점까지 자동으로 경로를 계획하고 이동합니다.
- **Obstacle Avoidance**: 주행 중 동적 장애물을 감지하고 안전한 우회 경로를 선택합니다.
- **Mission Planner**: 여러 웨이포인트를 순서대로 방문하는 미션을 수행합니다.
- **Simulation First**: Gazebo 환경에서 알고리즘을 먼저 검증한 뒤 실제 로봇에 적용할 수 있도록 구성합니다.
- **Visualization**: RViz에서 로봇 위치, 센서 값, costmap, global path, local path를 확인할 수 있습니다.

## 시스템 구성

```text
TurtleBot Mission Lab
├── Sensor Layer
│   ├── LiDAR scan
│   ├── Odometry
│   └── IMU
├── Perception Layer
│   ├── SLAM
│   ├── Localization
│   └── Obstacle detection
├── Planning Layer
│   ├── Global planner
│   ├── Local planner
│   └── Mission planner
└── Control Layer
    ├── Velocity command
    └── TurtleBot base controller
```

## 기술 스택

- ROS Noetic 또는 ROS 2 Humble
- TurtleBot3 Burger / Waffle Pi
- Gazebo
- RViz
- Python / C++ ROS nodes
- SLAM Toolbox 또는 GMapping
- Navigation Stack / Nav2

## 실행 예시

Gazebo 시뮬레이션 환경을 실행합니다.

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

미션 플래너를 실행합니다.

```bash
rosrun mission_app waypoint_mission.py
```

## 미션 시나리오

1. 로봇이 초기 위치에서 출발합니다.
2. LiDAR와 odometry 데이터를 이용해 주변 환경을 인식합니다.
3. 저장된 지도 위에서 첫 번째 웨이포인트까지 이동합니다.
4. 이동 중 장애물이 감지되면 local planner가 회피 경로를 생성합니다.
5. 모든 웨이포인트를 방문한 뒤 시작 지점으로 복귀합니다.
6. RViz에서 전체 이동 경로와 센서 데이터를 검토합니다.

## 기대 결과

- 실내 환경에서 안정적인 지도 생성
- 목표 지점까지의 자율 이동 성공
- 장애물 발생 상황에서 충돌 없는 회피 동작
- 반복 가능한 Gazebo 테스트 환경 구축
- 실제 TurtleBot 적용을 위한 기본 ROS 패키지 구조 확보

## 향후 개선 계획

- 음성 명령 기반 목적지 설정
- 객체 인식 카메라 연동
- 다중 로봇 협업 미션
- 웹 대시보드를 통한 실시간 상태 모니터링
- 배터리 상태 기반 자동 충전 스테이션 복귀

## 프로젝트 목표

이 프로젝트는 단순한 TurtleBot 예제를 넘어, 실제 서비스 로봇에 필요한 인식, 판단, 제어 흐름을 하나의 ROS 프로젝트 안에서 경험하는 것을 목표로 합니다. 시뮬레이션에서 시작해 실제 로봇으로 확장 가능한 구조를 지향하며, 자율주행 로봇 개발의 핵심 개념을 실습 중심으로 다룹니다.
