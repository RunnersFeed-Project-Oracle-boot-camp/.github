# Runners Feed Project Archive

2D 러닝 영상 분석 프로젝트에서 **측정 가능한 자세 피처를 검토하고, 영상 처리 병목을 실험으로 확인한 뒤, RunPod GPU 비동기 분석 서버를 구현해 팀 제품에 반영한 과정**을 정리한 개인 기술 아카이브입니다.

> Runners Feed는 Oracle 부트캠프에서 진행한 팀 프로젝트입니다.
> 이 Organization은 박주환이 직접 수행한 실험, 프로토타입과 기술 기록을 중심으로 구성하며, 팀 전체 결과와 개인 기여를 구분해 표시합니다.

## Project

Runners Feed는 사용자가 측면에서 촬영한 러닝 영상을 업로드하면 관절 움직임을 분석하고, 자세 측정값과 개선 행동을 리포트로 제공하는 비의료용 러닝 자세 분석 서비스입니다.

- [최종 팀 저장소](https://github.com/Temu-F4/Runners_Feed)
- 최종 발표자료 PDF: 공개 파일 연결 예정
- 1분 45초 서비스 시연 영상: 공개 파일 연결 예정

## My Contributions

### 1. 2D 영상에서 측정 가능한 자세 피처 검토

- 러닝 생체역학 연구의 지표가 측면 2D 영상과 Halpe-26 좌표로 계산 가능한지 검토했습니다.
- 지면반력, 관절 모멘트와 대사량처럼 별도 장비가 필요한 지표는 2D 영상 측정 대상에서 제외했습니다.
- 거리 관련 값은 실제 길이로 단정하지 않고 신체 크기로 정규화한 비율로 다뤘습니다.
- 연구 참고값을 의료 진단 기준이 아닌 비교용 정보로 구분했습니다.

### 2. 영상 처리 성능과 결과 차이 검증

- PNG와 JPEG 품질 95의 프레임 저장시간, 저장용량과 Halpe-26 좌표 차이를 비교했습니다.
- OCI CPU와 RunPod GPU의 영상 전송, 분석, 렌더링, 인코딩과 결과 다운로드를 포함한 왕복시간을 측정했습니다.
- 속도 개선뿐 아니라 CPU·GPU 포즈 결과의 차이와 실험 한계를 함께 기록했습니다.

### 3. RunPod 비동기 영상 분석 서버 구현

- OCI와 RunPod 사이의 Object Storage 기반 입력·산출물 전달 구조를 구현했습니다.
- 작업 등록, 상태 polling, 중복 추론 방지와 작업 상태 보존 기능을 추가했습니다.
- 모델 release, checksum과 결과 manifest를 이용해 산출물을 추적할 수 있도록 했습니다.
- 구현을 팀 저장소에 PR로 반영하고 운영 중 발견한 오류에 회귀 테스트를 추가했습니다.

## Results

| 검증 항목 | 기존 방식 | 변경 방식 | 관측 결과 |
|---|---:|---:|---:|
| 프레임 저장시간 | PNG 17.69초 | JPEG 품질 95 3.07초 | 82.6% 단축 |
| 저장용량 | PNG 902MB | JPEG 품질 95 186MB | 79.4% 감소 |
| 255프레임 처리 | OCI CPU 16.876초 | RunPod GPU 왕복 8.773초 | 48.01% 단축 |
| 631프레임 처리 | OCI CPU 83.351초 | RunPod GPU 왕복 17.461초 | 79.05% 단축 |

위 결과는 프로젝트의 제한된 시험 영상과 실행 환경에서 측정한 값입니다. 모든 입력 영상이나 GPU 환경에서 같은 결과를 보장하지 않습니다.

## Featured Work

### [OCI CPU / RunPod GPU 영상 분석 성능 검증](https://github.com/RunnersFeed-Project-Oracle-boot-camp/CPU-GPU-Speed-Compare-by-RunPod)

영상 분석 병목을 측정하고, OCI CPU와 RunPod GPU의 실제 왕복 처리시간을 비교했습니다. 이후 Object Storage와 비동기 작업 계약을 이용한 GPU 분리 구조를 팀 제품에 연결했습니다.

### [PNG vs JPEG 품질·성능 검증](https://github.com/RunnersFeed-Project-Oracle-boot-camp/PNG-vs-JPEG-Rendering-Performance-and-FPS-Comparison)

프레임 저장 형식 변경이 처리시간, 용량과 포즈 좌표에 미치는 영향을 동일 영상으로 비교했습니다. 큰 오차가 발생한 화면 진입·퇴장 구간도 별도로 확인했습니다.

### [Running Pose Feature Prototype 2](https://github.com/RunnersFeed-Project-Oracle-boot-camp/running-pose-feature-prototype2)

RTMDet과 RTMPose-M Halpe-26을 이용해 영상 입력부터 자세 추정, 피처 계산과 코칭 리포트 생성까지 연결한 기능 검증용 프로토타입입니다.

### Additional Work

- [러닝 자세 분석 서비스 설문 EDA](https://github.com/RunnersFeed-Project-Oracle-boot-camp/survey-analysis)
- [논문 기반 피처 설계 아카이브](https://github.com/RunnersFeed-Project-Oracle-boot-camp/Find-features-based-on-Paper-)
- [프로젝트 스크럼과 회고](https://github.com/RunnersFeed-Project-Oracle-boot-camp/Project-Scrum-Notes-Retrospective)

## Pull Request Evidence

- [PR #23 · RunPod video analysis dispatch pipeline](https://github.com/Temu-F4/Runners_Feed/pull/23)
- [PR #24 · RunPod proxy User-Agent 수정](https://github.com/Temu-F4/Runners_Feed/pull/24)
- [PR #25 · GPU 계약 UUID 직렬화 수정](https://github.com/Temu-F4/Runners_Feed/pull/25)
- [PR #26 · systemd 동기화와 배포 디스크 보호](https://github.com/Temu-F4/Runners_Feed/pull/26)
- [PR #28 · 팀 검토 절차를 위한 초기 변경 회수](https://github.com/Temu-F4/Runners_Feed/pull/28)
- [PR #34 · 비동기 RunPod 서버 재구현 및 main 병합](https://github.com/Temu-F4/Runners_Feed/pull/34)

초기 통합 변경은 팀의 브랜치 검토 순서를 맞추기 위해 한 차례 되돌렸습니다. 이후 구조와 적용 범위를 다시 정리해 비동기 서버로 구현했고 PR #34를 통해 팀 저장소 `main`에 병합했습니다. 후속 모델 통합과 안정화 작업은 팀원들의 공동 기여입니다.

## Scope and Limitations

- 이 결과는 의료 진단이나 부상 예측을 제공하지 않습니다.
- 착지와 toe-off는 2D 관절 좌표 변화로 추정한 이벤트입니다.
- 카메라 각도, 가림, 모션 블러와 화면 진입·퇴장 구간이 결과에 영향을 줄 수 있습니다.
- 최종 앱, API, 모델과 운영 인프라 전체는 팀 공동 결과입니다.
- PRD, WBS, BMC와 최종 발표자료는 팀 공동 산출물이며 개인 단독 작성물이 아닙니다.

## Documentation

- PoC 검증과 기술적 판단: 공개 문서 연결 예정
- MVP 범위와 개인 기여: 공개 문서 연결 예정
- 실험 조건, 한계와 PR 이력: 공개 문서 연결 예정
