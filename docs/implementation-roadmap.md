# 구축 로드맵과 운영 체크리스트

이 문서는 ISMS-P 클라우드 SaaS 런북을 실행 계획, 수용 기준, 운영 체크리스트, 구매 체크리스트로 풀어낸 자료입니다.

상위 문서: [ISMS-P 클라우드 SaaS 기술 준비 런북](./isms-p-cloud-saas-runbook.md)

## 6. 권장 구축 로드맵

### 0단계: 인증 범위 고정

- 인증 대상 서비스, AWS 계정, 리전, VPC, EKS/ECS/EC2, 데이터 저장소, 외부 SaaS 범위를 고정한다.
- 개인정보 처리 흐름과 수탁/제3자 제공 목록을 먼저 만든다.
- 운영명세서와 실제 자산 인벤토리가 같은 식별자를 쓰도록 태그와 네이밍 규칙을 정리한다.

### 1단계: P0 증적 기반선

- IAM Identity Center와 MFA를 기준으로 AWS 콘솔/CLI 접근을 통합한다.
- CloudTrail organization trail, AWS Config, Security Hub, GuardDuty, Inspector, Macie를 전 계정/리전에 적용한다.
- CloudTrail, WAF, ALB, EKS audit, app 감사 로그를 보존 기간과 삭제 방지 정책이 있는 저장소로 모은다.
- KMS, Secrets Manager, S3/RDS/EBS/ECR 암호화, secret rotation 기준을 표준화한다.
- 백업 정책과 복구 리허설을 월간 또는 분기 단위로 실행한다.
- 개인정보 데이터 맵과 파기 오케스트레이터의 MVP를 만든다.

### 2단계: 운영 워크플로 자동화

- 접근권한 검토, 취약점 조치, 변경관리, 사고 대응, 파기 실패, 외부 SaaS 검토를 티켓 시스템과 연결한다.
- 예외 승인은 만료일, 책임자, 위험수용 근거가 없으면 승인되지 않게 한다.
- 배포 전 SAST/SCA/secret/IaC/image scan 게이트를 적용하고 릴리스 증적과 연결한다.
- SIEM/SOAR 또는 Security Hub/EventBridge 기반으로 탐지 이벤트를 사고 대응 허브에 자동 접수한다.

### 3단계: 심사 패키징과 고도화

- ISMS-P 통제 항목별 증적 패키지를 자동 생성한다.
- 월간 보안 KPI를 경영 보고 자료로 만든다.
- CNAPP, DSPM, GRC 도구를 내부 플랫폼과 연계해 수작업 캡처를 줄인다.
- 외부 SaaS 도입 영역은 계약, 개인정보 처리위탁, 로그 반출, API 권한, 데이터 보관 위치를 구매 체크리스트에 포함한다.

## 7. 수용 기준

심사 전 최소 수용 기준은 다음과 같다.

- 인증 범위 내 AWS 리소스의 95% 이상이 소유자, 중요도, 개인정보 처리 여부, 환경 태그를 가진다.
- AWS 콘솔/CLI 관리자 권한은 SSO, MFA, 승인된 permission set, 정기 검토로만 부여된다.
- CloudTrail, Config, GuardDuty, Security Hub, Inspector, Macie 적용 범위와 예외가 계정/리전 단위로 설명 가능하다.
- P0 취약점과 high/Critical finding은 SLA, 예외 승인, 재검증 기록을 가진다.
- 개인정보 저장소, 로그, 분석 데이터, 외부 SaaS, 백업의 보유 기간과 파기 방식이 데이터 맵에 연결된다.
- 최근 1회 이상 복구 리허설, 사고 대응 모의훈련, 접근권한 정기 검토, 개인정보 파기 검증 결과가 존재한다.
- 모든 외부 SaaS는 처리 데이터, 접근권한, 계약/DPA, 보안성 검토, 해지/파기 절차가 등록되어 있다.

## 8. 운영 체크리스트

| 주기 | 점검 항목 | 담당 |
|---|---|---|
| 매일 | GuardDuty/Security Hub Critical finding 확인, SIEM alert triage | Security/SRE |
| 매주 | 취약점 SLA 위반, WAF 차단 이벤트, secret scan 실패, 백업 작업 실패 확인 | Security/SRE |
| 매월 | 자산 인벤토리 drift, 접근권한 검토, 로그 수집률, 개인정보 파기 결과, 외부 SaaS 변경 확인 | Security/Infra/Privacy |
| 분기 | 복구 리허설, 사고 대응 모의훈련, KMS/secret 정책 검토, 수탁사 보안성 재검토 | Security/Infra/Privacy/Legal |
| 반기 | 인증 범위 재확인, 운영명세서 갱신, P0/P1 개선 현황 경영 보고 | CISO/Compliance |

## 9. 구매 체크리스트

외부 SaaS 또는 보안 제품을 도입할 때는 다음 항목을 계약 전 확인한다.

- AWS Organizations 멀티 계정과 EKS/ECS/EC2/RDS/S3/ECR 범위를 지원하는가.
- 서울 리전 또는 한국 개인정보 처리 요구에 맞는 데이터 보관 위치와 하위처리자 정보를 제공하는가.
- 원본 로그와 증적을 우리 계정 또는 독립 저장소로 반출할 수 있는가.
- ISMS-P 커스텀 통제 매핑과 증적 내보내기가 가능한가.
- 관리자 접근, API token, SCIM/JIT provisioning, SSO/MFA, RBAC, 감사 로그를 제공하는가.
- finding suppression, exception, risk acceptance에 만료일과 승인자를 강제할 수 있는가.
- 탐지 결과가 Jira/ServiceNow/Slack/PagerDuty/SIEM으로 연동되는가.
- 제품 해지 시 데이터 export, 삭제증명, 보존 기간 종료 처리가 가능한가.
- 가격이 수집량, 자산 수, 사용자 수, 계정 수, 워크로드 수 중 무엇에 연동되는가.

## 10. 다음 산출물

이 런북을 실제 프로젝트로 전환할 때는 다음 문서를 추가한다.

- `control-mapping.xlsx`: ISMS-P 세부점검항목별 내부 모듈, 외부 도구, 증적 위치 매핑
- `aws-baseline.md`: AWS 계정/리전별 보안 서비스 활성화 기준
- `privacy-data-map.md`: 개인정보 항목, 저장소, 처리 목적, 보유 기간, 파기 방식
- `vendor-evaluation.md`: 실제 후보 제품 PoC 결과와 계약/개인정보 검토 결과
- `audit-evidence-index.md`: 심사 제출용 증적 인덱스
