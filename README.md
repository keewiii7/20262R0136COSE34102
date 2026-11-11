# 20262R0136COSE34102 — Operating Systems (COSE341-02)

2026년 2학기 고려대학교 운영체제(COSE341-02) 수업 학습 기록 저장소입니다.
> 과제 규정에 따라 Project #1, #2의 **소스 코드는 업로드하지 않으며**, 구현 내용만 간략히 설명합니다.

## Development Environment
| 항목 | 내용 |
|---|---|
| Host OS | macOS |
| Virtual Machine | Lima |
| Ubuntu | 22.04 LTS |
| Linux Kernel | 6.6.36 |

## Project #1 — Linux System Call 추가 (`my_stack`)
리눅스 커널에 스택 자료구조를 다루는 새 시스템 콜을 추가하고, 유저 프로그램에서 호출해 동작을 확인했습니다.

- **`syscall_64.tbl`**: 새 시스템 콜 번호를 시스템 콜 테이블에 등록
- **`syscalls.h`**: 시스템 콜 함수 원형(prototype) 선언
- **`kernel/sslab_my_stack.c`**: `SYSCALL_DEFINE` 매크로로 push / pop 동작을 커널 내부에 구현
- **`kernel/Makefile`**: 새 소스 파일을 커널 빌드 대상(`obj-y`)에 추가
- **`call_my_stack.c`**: 유저 공간에서 `syscall()`로 시스템 콜을 호출하고 결과 출력

**배운 점**: 유저 프로그램 → `syscall` 명령 → 커널 모드 전환 → 시스템 콜 테이블 조회 → 커널 함수 실행 → 유저 모드 복귀로 이어지는 호출 흐름.

## Project #2 — CPU Scheduling System Call (`ku_cpu`)
여러 프로세스가 하나의 가상 CPU 자원을 요청할 때, 커널 시스템 콜이 스케줄링 정책에 따라 어느 프로세스에 CPU를 줄지 결정하도록 구현했습니다.

- **`kernel/sslab_ku_cpu.c`**: 요청 큐를 관리하고 정책에 따라 CPU 점유 여부를 결정
- **`kucpu_run.c`**: 각 프로세스가 작업 시간만큼 시스템 콜을 반복 호출하는 유저 프로그램
- **구현 정책**: FCFS, SJF, Round-Robin
- **실험**: 정책별 Gantt chart를 그리고 평균 대기 시간(waiting time)·응답 시간(response time) 비교

| 정책 | 장점 | 단점 |
|---|---|---|
| FCFS | 구현이 단순, 공정한 도착 순서 | Convoy effect로 평균 대기 시간 증가 |
| SJF | 평균 대기 시간 최소 | 실행 시간 예측 필요, starvation 가능 |
| RR | 응답 시간이 좋음 | 문맥 교환 오버헤드, time quantum에 민감 |

## Study Notes
`01-introduction.md` ~ `07-ipc.md` 파일에 강의 단원별 요약을 정리했습니다.
