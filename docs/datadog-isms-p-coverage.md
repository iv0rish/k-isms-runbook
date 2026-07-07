# Datadog ISMS-P 커버리지

작성 기준일: 2026-07-08

이 문서는 Datadog으로 보완할 수 있는 ISMS-P 기술·운영 항목을 정리한 자료입니다. Datadog은 로그, 옵저버빌리티, 클라우드 보안, 코드 보안, CI/CD, 애플리케이션 운영 증적에 강점이 있지만, ISMS-P 인증을 단독으로 충족하는 도구는 아닙니다. 정책, 승인, 예외, 위험수용, 원본 보관, 개인정보 처리 목적처럼 조직이 직접 설명해야 하는 증적은 내부 통제와 함께 설계해야 합니다.

상위 문서: [ISMS-P 클라우드 SaaS 기술 준비 런북](./isms-p-cloud-saas-runbook.md)

관련 문서: [외부 SaaS/보안 제품 후보 비교](./external-saas-solutions.md)

## 1. 제품별 역할

| Datadog 제품/기능 | 주요 역할 | ISMS-P 활용 영역 | 공식 링크 |
|---|---|---|---|
| Cloud SIEM | 로그 기반 위협 탐지, 보안 신호 생성, 조사 | 로그, 침해 탐지, 사고 대응, 보안관제 증적 | <https://www.datadoghq.com/product/cloud-siem/> |
| Cloud Security | CSPM, 취약점, ID 위험, 규정 준수 상태 점검 | 클라우드 설정 점검, 취약점, 권한 위험, 지속 점검 | <https://www.datadoghq.com/product/cloud-security/> |
| Infrastructure Monitoring / Resource Catalog | 호스트·컨테이너·클라우드 리소스 관측과 인벤토리 | 자산관리, 운영 모니터링, 리소스 소유자·태그 관리 | <https://www.datadoghq.com/product/infrastructure-monitoring/> |
| Code Security | SAST, SCA, IAST, IaC, secret scanning | 개발보안, 소스코드·오픈소스 취약점, 배포 전 보안 게이트 | <https://www.datadoghq.com/product/code-security/> |
| CI Visibility | CI/CD 파이프라인 관측, 작업·커밋 상관관계, 실패 분석 | 변경관리, 배포 증적, 개발 생산성·품질 지표 | <https://www.datadoghq.com/product/ci-cd-monitoring/> |
| APM | 분산 trace, 서비스 맵, SLO, 배포 영향 분석 | 서비스 운영, 장애 대응, 가용성 관리, 장애 원인 분석 | <https://www.datadoghq.com/product/apm/> |

## 2. ISMS-P 항목별 커버리지

| ISMS-P 관점 | 통제 의도 | Datadog 제품/기능 | 커버 수준 | 가능한 Datadog 증적 | 보완 필요 영역 |
|---|---|---|---|---|---|
| `1.1`, `1.2`, `2.1` 인증범위·자산관리 | 인증 범위 안의 AWS, 호스트, 컨테이너, 서비스 자산을 식별 | Infrastructure Monitoring, Resource Catalog, Cloud Security | 중 | 리소스 목록, 태그, 소유자, 클라우드 계정, 호스트·컨테이너 인벤토리 | 계약 자산, 외부 SaaS, 단말, 개인정보 처리 저장소까지 포함한 기준 CMDB |
| `1.4` 점검 및 개선 | 기술 통제 상태를 정기 점검하고 개선조치 추적 | Cloud Security, 대시보드, 모니터, 케이스·티켓 연동 | 중 | 보안 자세 리포트, 설정 오류 finding, monitor history, ticket link | 월간 점검 승인, 예외 승인, 잔여위험 수용, 경영 보고 |
| `2.5`, `2.6` 인증·권한·접근통제 | 과권한, 미사용 권한, 위험한 클라우드 ID 식별 | Datadog Cloud Security의 Identity Risks, CIEM 성격 권한 분석 | 중 | ID 위험 finding, 고권한 finding, 권한 위험 리포트 | 실제 권한 회수 승인, 정기 access review, 서버/DB 세션 기록 |
| `2.6` 네트워크 접근통제 | 공개 노출, 보안그룹, Kubernetes·클라우드 설정 위험 점검 | Cloud Security, Infrastructure, Network Monitoring | 중 | 공개 노출 finding, 보안그룹 관련 finding, 리소스 관계 | 허용 경로 승인, 운영망·개인정보 처리구간 정의, 방화벽 변경 승인 |
| `2.7` 암호화 | 저장·전송 암호화와 클라우드 암호화 설정 점검 | Datadog Cloud Security 자세 점검 | 중 | S3/RDS/EBS/KMS 관련 설정 오류, 암호화 상태 | 키 소유자, KMS key policy 검토, 키 교체, 암호화 예외 승인 |
| `2.8` 개발보안 | 코드·오픈소스·IaC·secret 보안 결함을 배포 전에 식별 | Code Security, CI Visibility | 중 | SAST/SCA/IAST/IaC finding, PR comment, 파이프라인 trace, 실패한 작업 | 코드리뷰 승인, 보안 예외 승인, 릴리스 차단 정책, 보안교육 |
| `2.9` 운영관리·변경관리 | 운영 변경, 배포, 장애, 로그를 추적 | CI Visibility, APM, Log Management | 중 | 커밋 SHA, 파이프라인 작업, 배포 마커, trace, 서비스 상태, 로그 쿼리 | 변경 요청서, 승인자, 영향도 평가, 롤백 승인, 운영 반영 기록 |
| `2.9`, `2.11` 로그·보안관제 | 보안 이벤트를 탐지하고 조사 가능하게 유지 | Cloud SIEM, Log Management, Workload Protection | 상 | 보안 신호, 탐지 룰, Log Explorer, 조사 화면, 알림 라우팅 | 원본 로그 WORM 보관, 보존 기간 정책, 사고 등급화, 사후조치 보고서 |
| `2.10` 취약점·패치 | 호스트, 컨테이너, 서버리스, 코드 취약점 식별과 우선순위화 | Datadog Cloud Security Vulnerabilities, Datadog Code Security | 상 | 취약점 finding, severity, affected asset, SBOM, remediation status | 패치 적용 증적, 예외 승인, 조치 SLA, 재검증 결과 |
| `2.10` 악성코드·런타임 위협 | 워크로드 행위 기반 위협 탐지 | Workload Protection, Cloud SIEM | 중 | 워크로드 신호, 파일·프로세스·네트워크 이벤트, 탐지 룰 적중 이력 | 단말 EDR, 서버 격리 절차, 포렌식 보존, 운영자 대응 승인 |
| `2.11` 사고 대응 | 탐지 이벤트를 triage하고 조사 이력을 남김 | Cloud SIEM, 워크플로·티켓 연동, APM | 중 | 보안 신호 타임라인, 관련 로그·trace, 알림, 케이스 링크 | 사고 대응 절차서, 훈련 결과, 사후보고서, 법적 신고 판단 |
| `2.12` 백업·재해복구 | 백업 작업과 서비스 상태를 모니터링 | Infrastructure Monitoring, APM, 모니터 | 보조 | 백업 작업 메트릭과 로그, 서비스 SLO, 사고 타임라인 | AWS Backup 정책, immutable backup, 실제 복구 리허설, RTO/RPO 측정 |
| `3.1`, `3.2` 개인정보 흐름·접근기록 | 개인정보 처리 시스템 로그와 접근 이벤트 분석 | Cloud SIEM, Log Management, APM, Sensitive Data Scanner | 보조 | 개인정보 관련 애플리케이션 로그, 관리자 행위 로그, 민감정보 스캔 결과 | 개인정보 데이터 맵, 처리 목적, 정보주체, DB 쿼리 목적 코드, 파기 정책 |
| `3.3` 제공·위탁·국외이전 | 외부 연동과 데이터 이동을 모니터링 | Cloud SIEM, APM, Network Monitoring | 보조 | API 호출 로그, egress 로그, SaaS 연동 이벤트 | 위탁계약, DPA, 국외이전 고지·동의, 수탁사 보안성 평가 |
| `3.4` 보유·파기 | 파기 배치와 실패 이벤트를 모니터링 | APM, Log Management, 모니터 | 보조 | 파기 작업 로그, 실패 알림, trace, 대시보드 | 원천 DB 삭제, 백업 보존 정책, 파기 검증 샘플, 법적 보존 예외 |

커버 수준의 의미는 다음과 같습니다.

| 수준 | 의미 |
|---|---|
| 상 | Datadog이 주요 기술 증적의 핵심 소스가 될 수 있음 |
| 중 | Datadog 증적이 강하게 보완하지만 승인·정책·예외 기록과 결합해야 함 |
| 보조 | Datadog은 모니터링이나 로그 일부만 제공하며 원천 통제는 별도 시스템에 있음 |

## 3. 단독 충족이 어려운 항목

| 항목 | Datadog으로 가능한 부분 | 부족한 통제 요소 | 보완해야 할 내부 증적 |
|---|---|---|---|
| 로그 관리 및 보관 | 로그 수집, 검색, Cloud SIEM 분석, alert | 로그 소스 선정, 보존 기간, 위변조 방지, 원본 보관 책임 | 로그 수집 매트릭스, S3 Object Lock/WORM, 보존 정책, 정기 검토 기록 |
| 침해 탐지 및 대응 | 보안 신호, 탐지 룰, 조사 화면, alert routing | 사고 등급, 담당자 승인, 법적 신고 판단, 재발방지 | 사고 대응 절차서, 사고 티켓, 타임라인, 사후보고서, 모의훈련 결과 |
| 취약점 관리 | 취약점 탐지, 우선순위화, SBOM, remediation status | 패치 실행, 예외 승인, 잔여위험 수용, 재검증 | 조치 티켓, 배포 이력, 예외 승인서, SLA 준수 리포트 |
| 자산 관리 | 연결된 클라우드·호스트·컨테이너 리소스 카탈로그 | Datadog 미연동 SaaS, 계약, 단말, 비활성 자산 | 인증범위 대장, CMDB, SaaS inventory, 개인정보 처리 시스템 목록 |
| 변경·배포 통제 | 파이프라인 trace, commit, 작업 로그, 배포 영향 | 변경 승인, 권한 분리, 운영 반영 승인, 롤백 승인 | 변경요청서, 승인 이력, 릴리스 노트, 롤백 기록 |
| 권한 검토 | 클라우드 ID 위험, 과권한 finding | 사람별 직무·부서·외부자 여부와 권한 의미 해석 | IdP/HRIS 사용자 기준, access review 결과, 회수 티켓 |
| 개인정보 처리 단계 | 로그와 trace 일부 분석 | 수집 동의, 처리 목적, 보유 기간, 파기, 정보주체 권리 대응 | 개인정보 데이터 맵, 동의 이력, 파기 이력, DSAR 처리 기록 |
| Datadog 자체 운영 | Datadog 내 권한과 Audit Trail | 원천 시스템 관리자 행위와 Datadog 밖 변경 | Datadog RBAC, SSO/MFA 설정, Audit Trail, 관리자 권한 정기 검토 |

## 4. 권장 증적 패키지

| 증적 유형 | 생성 위치 | 감사 시 활용 예 | 보존/내보내기 원칙 |
|---|---|---|---|
| Cloud SIEM signal export | Cloud SIEM | 침해 탐지와 대응 이력 | 월별 export 또는 API 수집, 원본 로그와 함께 Evidence Lake 보관 |
| Detection rule inventory | Cloud SIEM | 탐지 룰 운영 여부와 변경 이력 | rule 목록, enabled 상태, 변경 승인 기록 연결 |
| Misconfiguration report | Cloud Security | CSPM 점검 결과와 개선조치 | finding, severity, asset, remediation status를 기간별 보관 |
| Vulnerability report | Cloud Security, Code Security | 취약점 진단·조치 증적 | CVE, affected asset, owner, SLA, ticket link 포함 |
| Identity risk report | Cloud Security | 과권한·위험 권한 점검 | risk finding과 access review 결과를 연결 |
| Resource inventory | Infrastructure Monitoring, Resource Catalog | 자산 식별과 인증범위 검증 | 태그, 소유자, 계정, 리전, 서비스명 기준으로 export |
| Pipeline trace | CI Visibility | 변경·배포 실행 증적 | 커밋 SHA, 파이프라인 URL, 작업 결과, 승인 단계 정보를 보관 |
| APM service report | APM | 장애 대응, 배포 영향, 서비스 운영 | 서비스 맵, SLO, trace sample, 배포 마커를 기간별 저장 |
| Datadog Audit Trail | Datadog 계정 관리 | Datadog 설정 변경과 사용자 활동 | 관리자 권한, API key, monitor/rule 변경 이력을 별도 보관 |

## 5. 도입 시 전제 조건

| 전제 | 이유 |
|---|---|
| AWS 계정, EKS/ECS, EC2, RDS, S3, GitHub, CI/CD, IdP 연동 범위를 먼저 확정 | Datadog이 보지 못하는 시스템은 증적 공백으로 남음 |
| 모든 리소스에 `service`, `env`, `owner`, `system`, `privacy`, `criticality` 태그 기준을 적용 | ISMS-P 인증범위, 담당자, 개인정보 처리 여부를 Datadog 데이터와 연결하기 위함 |
| CloudTrail, WAF, ALB, EKS audit, 애플리케이션 감사, DB 감사 로그의 수집 매트릭스를 별도 관리 | SIEM 도입보다 로그 소스 누락 방지가 더 중요함 |
| Datadog finding을 Jira/ITSM/GRC와 연결 | 심사에서는 탐지보다 조치 완료와 예외 승인 기록이 중요함 |
| Datadog export를 Evidence Lake에 주기적으로 보관 | SaaS 화면 캡처 의존을 줄이고 심사 기간별 재현 가능한 증적을 만들기 위함 |
