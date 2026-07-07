# ISMS-P 클라우드 SaaS 기술 준비 런북

작성 기준일: 2026-07-08

이 문서는 한국 ISMS-P 인증을 준비하는 AWS 기반 클라우드 네이티브 SaaS 조직을 위한 기준 문서입니다. 대상 환경은 AWS Organizations 멀티 계정, EC2, ECS, EKS, 컨테이너 이미지, IaC, CI/CD, 민감정보 또는 고위험 개인정보 처리를 전제로 한다.

## 문서 구성

- 현재 문서: 기준, 기술 통제 매핑, 심사 리스크, AWS 네이티브 통제 기준선
- [내부 구축 플랫폼 모듈](./internal-platform-modules.md)
- [외부 SaaS/보안 제품 후보 비교](./external-saas-solutions.md)
- [Datadog ISMS-P 커버리지](./datadog-isms-p-coverage.md)
- [구축 로드맵과 운영 체크리스트](./implementation-roadmap.md)

## 1. 기준과 범위

### 공식 기준

- KISA ISMS-P 제도소개: <https://isms.kisa.or.kr/main/ispims/intro/>
- KISA ISMS-P 자료실: <https://isms.kisa.or.kr/main/ispims/notice/>
- KISA 인증기준 구조: `1. 관리체계 수립 및 운영(16)`, `2. 보호대책 요구사항(64)`, `3. 개인정보 처리단계별 요구사항(21)`
- KISA 자료실 기준 주요 자료: `ISMS-P 인증제도 안내서(2024.07)`, `ISMS-P 인증기준 안내서(2023.11)`, `ISMS-P 인증기준 세부점검항목(2023.10.31)` 계열
- KISA 자료실 직접 링크: [ISMS-P 인증기준 안내서(2023.11)](https://isms.kisa.or.kr/main/ispims/notice/?boardId=bbs_0000000000000014&cntId=21&mode=view), [ISMS-P 세부점검항목(2023.10.31)](https://isms.kisa.or.kr/main/ispims/notice/?boardId=bbs_0000000000000014&cntId=19&mode=view), [ISMS-P 인증제도 안내서(2024.07)](https://isms.kisa.or.kr/main/ispims/notice/?boardId=bbs_0000000000000014&cntId=24&mode=view)

### 이 문서의 초점

- `1.x 관리체계`: 위험관리, 운영, 점검, 개선에 필요한 기술 증적과 워크플로만 다룬다.
- `2.x 보호대책 요구사항`: AWS, 컨테이너, 접근통제, 로그, 취약점, 암호화, 운영보안, 사고대응, 재해복구를 중심으로 다룬다.
- `3.x 개인정보 처리단계별 요구사항`: 수집, 이용, 제공, 파기, 정보주체 권리 대응을 기술적으로 증명하는 체계를 다룬다.
- 법무 판단, 개인정보처리방침 문구, 조직 규정 문서 자체는 별도 산출물로 본다.

### 우선순위

| 우선순위 | 의미 | 판단 기준 |
|---|---|---|
| P0 | 심사 전 반드시 닫아야 하는 항목 | 접근통제, 로그, 개인정보 흐름, 암호화, 취약점, 백업처럼 부적합 가능성이 높고 증적 없이는 설명이 어려운 영역 |
| P1 | 운영 안정화를 위해 1차 구축해야 하는 항목 | 자동화하면 심사 부담이 줄고 반복 운영 품질이 높아지는 영역 |
| P2 | 성숙도와 효율을 높이는 항목 | 대시보드, 고급 탐지, 자동 보고, 비용 최적화처럼 초기 인증 이후 고도화 가능한 영역 |

### 기술 통제 빠른 매핑

| ISMS-P 영역 | AWS SaaS에서 직접 확인할 기술 이슈 |
|---|---|
| `1.1`, `1.2` 관리체계 기반·위험관리 | 인증 범위, 개인정보 흐름, AWS 계정/리전/VPC/EKS/ECS/CI/CD/외부 SaaS 누락 여부 |
| `2.1` 정책·조직·자산관리 | 클라우드 리소스, 컨테이너 이미지, IaC, 비밀 정보, 로그 저장소의 자산대장 반영 여부 |
| `2.3` 외부자 보안 | MSP, 외주 개발자, 수탁사, SaaS 관리자 접근권한과 계약·점검 증적 |
| `2.5`, `2.6` 인증·권한·접근통제 | IAM, SSO/MFA, EKS RBAC, DB 권한, break-glass, 관리포트, VPC/보안그룹 노출 |
| `2.7` 암호화 | 저장·전송 암호화, KMS 키 정책, 키 교체, 민감정보·고유식별정보 보호 |
| `2.8` 개발보안 | 코드리뷰, CI/CD 권한, IaC 스캔, secret scan, 테스트 데이터 비식별·마스킹 |
| `2.9`, `2.10` 운영·서비스 보안 | 변경관리, 로그·접속기록, 시간동기화, 패치, 컨테이너/노드 보안, 악성코드 대응 |
| `2.11`, `2.12` 사고대응·재해복구 | 이상행위 탐지, 취약점 조치, 사고 대응훈련, RPO/RTO, 백업 복구시험 |
| `3.x` 개인정보 처리단계 | 수집 동의, 보유 기간, 제공·위탁·국외이전, 파기, 정보주체 권리, 개인정보처리시스템 접속기록 |

## 2. 심사 리스크가 큰 기술 이슈

| 우선순위 | ISMS-P 영역 | 기술 이슈 | AWS SaaS 환경에서 흔한 결함 | 필요한 증적 |
|---|---|---|---|---|
| P0 | 1.1, 1.2, 2.1, 3.2 | 인증범위·자산·개인정보 흐름 누락 | AWS 계정, VPC, EKS 클러스터, ECS 서비스, EC2, RDS, S3, ECR, 외부 SaaS가 운영명세서와 불일치 | 인증범위 정의서, 자산 인벤토리, 개인정보 흐름도, 태그 정책, 중요도/개인정보 처리 여부, 소유자 |
| P0 | 2.5 인증 및 권한관리 | 퇴사자/직무변경자 접근권한 잔존 | IAM 사용자 장기키, 공유 계정, 과도한 관리자 권한, break-glass 계정 미검토 | 계정 목록, MFA 적용률, 권한 검토 결과, 권한 변경 승인 티켓, 장기키 제거 내역 |
| P0 | 2.6 접근통제 | 운영망과 개인정보 처리 시스템 접근 경로 불명확 | 콘솔/SSH/kubectl/DB 접속이 SSO, VPN, 승인 절차, 세션 기록 없이 열림 | 접근 경로도, 보안그룹/NACL, bastion/SSM Session Manager 기록, EKS RBAC, DB 접근 승인 기록 |
| P0 | 2.7 암호화 적용 | 민감정보 암호화와 키 관리 미흡 | RDS/S3/EBS/EFS/ECR 암호화 누락, KMS 키 정책 과다, 키 교체 기록 없음 | 암호화 설정 캡처/API 결과, KMS key policy, key rotation 설정, 비밀정보 보관 정책 |
| P0 | 2.9 운영관리, 2.10 보안관리 | 로그 수집·보존·무결성 부족 | CloudTrail 일부 리전만 활성화, EKS 감사 로그 누락, ALB/WAF/애플리케이션 로그 보존 기간 미정, 운영자가 로그 삭제 가능 | 로그 아키텍처, CloudTrail 조직 trail, S3 Object Lock 또는 별도 WORM 저장, 보존 기간 정책, 로그 조회 기록 |
| P0 | 2.9, 2.11, 3.2 | 로그·접속기록으로 사고 재구성이 불가능 | 관리자 화면, 개인정보 조회, DB 쿼리, app 감사 로그가 인프라 로그와 연결되지 않음 | 로그 수집 매트릭스, 관리자·개인정보 처리 로그 샘플, 위변조 방지 근거, 시간동기화, 정기 점검 기록 |
| P0 | 2.10 보안관리 | 취약점 진단과 조치 추적 미흡 | EC2 패치 기준 없음, ECR 이미지 스캔 실패 무시, IaC 보안검토 없음, 조치 SLA 없음 | 스캔 결과, 예외 승인, 조치 티켓, 재검증 결과, 미조치 위험수용 |
| P0 | 2.11 사고 예방 및 대응 | 탐지·대응 프로세스와 기술 로그 불일치 | GuardDuty/SIEM 알림이 티켓으로 연결되지 않거나 모의훈련 증적 없음 | 탐지 룰, 알림 라우팅, 사고 티켓, 타임라인, 사후조치 보고서, 모의훈련 결과 |
| P0 | 2.12 재해복구 | 백업/복구 검증 부재 | RDS snapshot만 있고 복구 테스트, RTO/RPO, 계정 탈취 대비가 없음 | 백업 정책, immutable backup 설정, 복구 리허설 결과, RTO/RPO 측정, DR 절차서 |
| P0 | 3.1, 3.2 | 개인정보 흐름과 저장 위치 불명확 | 수집 필드, 로그 내 개인정보, 분석 DB, S3 export, 고객지원 툴 동기화가 누락 | 개인정보 데이터 맵, 처리 목적, 보유 기간, 저장소별 필드 목록, 마스킹/암호화 적용표 |
| P0 | 3.3 | 제3자 제공/처리위탁 통제 미흡 | 외부 SaaS로 개인정보가 이동하지만 전송 근거, 계약, 접근권한, 파기 절차가 분리 관리됨 | 위탁/제공 목록, DPA/계약, API 연동 목록, 데이터 전송 로그, 수탁사 보안성 검토 |
| P0 | 3.4 | 파기 자동화와 증적 부재 | 탈퇴/보유 기간 만료 후 원장, 로그, 백업, 검색 인덱스에 개인정보 잔존 | 파기 배치 결과, 재시도/실패 큐, 백업 내 보관 정책, 파기 검증 샘플 |
| P1 | 2.8 개발보안 | 배포 전 보안 게이트 미흡 | PR 보안검토, SAST/SCA/secret scan, image scan, IaC scan 결과가 릴리스와 연결되지 않음 | 배포 승인, 보안 스캔 리포트, 차단 정책, 예외 승인 |
| P1 | 2.8, 2.9 | CI/CD와 운영환경 분리 미흡 | 배포 파이프라인이 운영 권한을 과다 보유하거나 승인·롤백·실제 반영 이력이 분리됨 | 브랜치 보호, 파이프라인 권한표, 배포 승인 이력, 운영 반영 기록, 롤백 기록 |
| P1 | 2.3 외부자 보안 | 외주/수탁 운영자 접근통제 미흡 | 협력사 계정이 내부 직원 계정과 동일하게 운영되거나 만료일 없음 | 외부자 계정 목록, 계약/보안서약, 접근기간, 작업 승인/종료 기록 |
| P1 | 2.4 물리보안 | 클라우드 책임분계 설명 부족 | AWS 책임 영역과 조직 책임 영역을 심사자에게 설명할 자료 없음 | AWS 책임공유모델 정리, 리전/가용영역 사용 정책, 클라우드 서비스 보안 인증자료 |
| P1 | 1.4 점검 및 개선 | 기술 통제의 정기 점검 자동화 부족 | 월간 점검이 수작업 캡처에 의존해 누락과 재현성 문제가 생김 | 월간 자동 점검 리포트, 미준수 티켓, 개선 완료 이력 |
| P2 | 2.8, 3.2 | 테스트 데이터 비식별·마스킹 부족 | 운영 개인정보를 개발·분석·QA 환경으로 복제하거나 마스킹 검증이 없음 | 테스트 데이터 생성 기준, 비식별 처리 결과, 운영 데이터 반출 승인 기록 |
| P2 | 2.6, 2.10 | 관리자 단말·원격근무 통제 증적 약함 | 운영자 노트북, 원격접속, 보조저장매체 통제가 AWS 접근증적과 연결되지 않음 | 관리자 단말 보안 기준, 디스크 암호화, EDR, 원격접속 승인, 저장매체 반출입 기록 |
| P2 | 2.11, 1.3 | 보안 KPI와 경영 보고 부족 | 취약점 SLA, 권한 검토율, 사고 대응 시간, 로그 수집률이 추세로 관리되지 않음 | 보안 대시보드, 월간 보고서, 경영진 검토 회의록 |

## 3. AWS 네이티브 통제 기준선

AWS 네이티브 서비스는 외부 SaaS 도입 전의 최소 기준선이다. ISMS-P 심사에서는 "서비스를 켰다"보다 "대상 범위, 정책, 예외, 검토 주기, 조치 이력"을 증명하는 자료가 더 중요하다.

| 통제 영역 | AWS 기준선 | 주요 증적 | 공식 링크 |
|---|---|---|---|
| 계정 거버넌스 | AWS Organizations, OU, SCP, delegated admin | 계정/OU 구조, 인증범위 계정 목록, SCP 정책, 위임 관리자 설정 | <https://aws.amazon.com/organizations/> |
| 계정·권한 | IAM Identity Center, IAM permission set, MFA, IAM Access Analyzer, credential report, break-glass 계정 | permission set 목록, MFA 현황, 관리자 권한 검토, 미사용 권한/외부 공유 분석, 장기키 점검 | <https://aws.amazon.com/iam/identity-center/> |
| 감사 로그 | Organization CloudTrail, CloudTrail management/data event, S3 보관, CloudWatch/EventBridge 연동 | trail 설정, 리전 적용, 로그 버킷 정책, 보존 기간, 삭제 방지 설정 | <https://aws.amazon.com/cloudtrail/> |
| 구성 준수 | AWS Config, managed/custom rule, conformance pack | 리소스 변경 이력, non-compliant 리소스, 예외 승인, remediation 기록 | <https://aws.amazon.com/config/> |
| 보안 관제 허브 | AWS Security Hub, GuardDuty, Inspector, Macie findings 집계 | finding 목록, 심각도별 조치 티켓, 월간 추세, suppression 근거 | <https://aws.amazon.com/security-hub/> |
| 위협 탐지 | Amazon GuardDuty, EKS Protection, Runtime Monitoring, S3/RDS/EC2 탐지 | GuardDuty 활성 계정/리전, finding 처리 이력, 모의훈련 결과 | <https://aws.amazon.com/guardduty/> |
| 취약점 | Amazon Inspector for EC2, ECR image, Lambda, code repository scan | critical/high 취약점, 조치 SLA, 이미지 배포 차단 정책, 재검증 | <https://aws.amazon.com/inspector/> |
| 민감정보 탐지 | Amazon Macie for S3, 별도 DSPM/DLP 연계 | S3 bucket별 민감정보 탐지 결과, 공개 설정, remediation 기록 | <https://aws.amazon.com/macie/> |
| 암호화·키 | AWS KMS, CloudHSM 필요성 검토, KMS key policy, rotation | 저장소별 암호화 표, key owner, key policy 검토, rotation 기록 | <https://aws.amazon.com/kms/> |
| 비밀정보 | AWS Secrets Manager, Parameter Store, secret rotation | secret 목록, 접근권한, rotation 성공/실패, 코드 내 secret scan 결과 | <https://aws.amazon.com/secrets-manager/> |
| 백업·DR | AWS Backup, cross-account/cross-region backup, restore test, immutable vault | 백업 정책, 복구 테스트, RTO/RPO, vault lock/air-gapped vault 검토 | <https://aws.amazon.com/backup/> |
| 웹 방어 | AWS WAF, Shield, ALB/CloudFront access logs, rate limiting | WAF rule, 차단/허용 로그, bot/rate 정책, 공격 대응 기록 | <https://aws.amazon.com/waf/> |
| EC2 운영 접속 | AWS Systems Manager, Patch Manager, Session Manager | 인벤토리, 패치 준수율, 세션 접속 로그, 명령 실행 이력, 포트 미개방 원격접속 | <https://docs.aws.amazon.com/systems-manager/latest/userguide/what-is-systems-manager.html> |
| ECS/EKS 로그 | ECS awslogs, EKS control plane logs, Container Insights | ECS task 로그, EKS API/audit/authenticator 로그, cluster/node/pod/task metric | <https://docs.aws.amazon.com/eks/latest/userguide/control-plane-logs.html> |
| CI/CD 변경관리 | CodePipeline, CodeBuild, CloudFormation/CDK, drift detection | 파이프라인 실행, 소스 리비전, 승인, 빌드·테스트 로그, 산출물, 스택 이벤트, drift 결과 | <https://docs.aws.amazon.com/codepipeline/latest/userguide/welcome.html> |
| 증적 자동화 | AWS Audit Manager 또는 대체 GRC/evidence 도구 | 자동 수집 증적, 통제 매핑, 감사 패키지 | <https://aws.amazon.com/audit-manager/> |

주의: AWS Audit Manager는 AWS 공식 페이지 기준 2026-04-30 이후 신규 고객에게 열리지 않는다고 공지되어 있다. 신규 도입은 AWS Security Hub, Config, CloudTrail, Backup Audit Manager, 외부 GRC 도구 조합을 먼저 검토한다.

### Evidence Lake 권장 구조

중앙 증적은 `Log Archive` 또는 별도 `Evidence` 계정에 모은다. 저장소는 S3 Versioning, S3 Object Lock, SSE-KMS, Public Access Block, 엄격한 bucket policy를 기본값으로 둔다. CloudTrail은 all-region organization trail과 log file integrity validation을 켜고 digest 파일까지 보관한다.

```text
s3://evidence-prod/
  cloudtrail/org/yyyy/mm/dd/
  config/snapshots/account=.../region=.../
  securityhub/findings/yyyy/mm/
  guardduty/findings/yyyy/mm/
  inspector/ecr-image-scan/yyyy/mm/
  macie/s3-sensitive-data/yyyy/mm/
  cloudwatch/ec2/
  cloudwatch/ecs/
  cloudwatch/eks/
  waf/
  rds/logs/
  backup/reports/
  cicd/codepipeline/
  cicd/codebuild/
  iac/cloudformation-drift/
```

모든 증적 객체에는 `control`, `system`, `account`, `region`, `period`, `owner`, `retention` 태그를 붙인다. 심사 제출용 AWS 자체 인증 자료는 AWS Artifact에서 별도 확보한다.
