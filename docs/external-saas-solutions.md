# 외부 SaaS/보안 제품 후보 비교

이 문서는 ISMS-P 대응 과정에서 우선 검토할 외부 SaaS와 보안 제품 후보를 정리한 자료입니다. 구매 결정을 대신하지 않습니다. 실제 도입 전에는 공식 제품 페이지, 계약 조건, 데이터 처리 위치, 한국 개인정보 처리위탁 요건, 로그 반출 가능성, API 제공 범위, 비용 구조를 별도로 확인해야 합니다.

상위 문서: [ISMS-P 클라우드 SaaS 기술 준비 런북](./isms-p-cloud-saas-runbook.md)

관련 문서: [Datadog ISMS-P 커버리지](./datadog-isms-p-coverage.md)

## 5. 외부 SaaS/보안 제품 후보 비교

### 5.1 우선 검토 후보 맵

| 영역 | 우선 검토 후보 | ISMS-P에서 보완하는 이슈 | 대표 증적 | 비고 |
|---|---|---|---|---|
| 서버 접근제어 | QueryPie ACP, Teleport | 운영 서버 접근 승인, 명령 감사, 세션 기록, 외부자 접근통제 | 접근 승인 이력, 세션 녹화, 명령 로그, 사용자별 접근권한 | QueryPie는 ACP 기준, Teleport는 Infrastructure Identity Platform 기준으로 검토 |
| DB 접근제어 | QueryPie ACP, Teleport | 개인정보처리시스템 DB 접근 승인, 쿼리 감사, 과권한 통제 | DB 접근 승인, 쿼리 로그, 접속 이력, 민감정보 접근 리포트 | 업무 목적 코드와 개인정보 조회 사유는 내부 앱 로그와 결합 필요 |
| SIEM | Datadog Cloud SIEM | 보안 로그 상관분석, 위협 탐지, 조사 이력 | 보안 신호, 탐지 룰, 로그 검색, 조사 타임라인 | 세부 규제 매핑은 Datadog 전용 문서 참조 |
| CSPM/CNAPP | Datadog Cloud Security, Wiz | 클라우드 설정 오류, 노출, 취약점, 권한 위험 | 설정 오류 finding, 취약점 finding, 보안 자세 리포트 | Datadog 중심 운영 시 Wiz는 보완 또는 비교 후보 |
| 리소스 인벤토리 | Datadog Infrastructure Monitoring, Resource Catalog | 클라우드·호스트·컨테이너 자산 가시성 | 리소스 목록, 태그, 소유자, 설정, 변경 추세 | 공식 CMDB와 인증범위 대장은 별도 관리 필요 |
| 인증·인가 관리 | Okta, Keycloak (OSS), Microsoft Entra ID | SSO, MFA, 계정 생명주기, 애플리케이션 접근통제 | 사용자 목록, MFA 적용률, 앱 접근 이력, 비활성화 이력 | Keycloak은 자체 운영 책임과 HA·감사·운영보안 설계 필요 |
| 취약점·코드 보안 | Datadog Code Security, Wiz, SonarQube, GitHub Advanced Security | SAST, SCA, IaC, secret scan, 컨테이너·클라우드 취약점 | 스캔 결과, PR 코멘트, 차단 이력, 예외 승인, 조치 티켓 | OSS/Enterprise 기능 차이를 구매 전 확인 |
| 엔드포인트 DLP | Privacy-i | 단말 내 개인정보·중요정보 반출 통제 | 매체 제어, 파일 반출 로그, 정책 위반 이력 | 공식 기능표와 에이전트 지원 OS 확인 필요 |
| Google Workspace DLP | Netkiller ISMS | Google Drive 외부 공유·민감정보 유출 통제 | Drive 전체 스캔, 외부 공유 리포트, 장기 로그인 이력 | Google Workspace 중심 조직에 적합. Microsoft 365는 별도 DLP 검토 필요 |
| SASE/SSE | Zscaler Zero Trust Exchange, Prisma SASE (Palo Alto Networks), Cloudflare One | 원격근무, SaaS 접속, 웹 보안, CASB, 네트워크 접근통제 | 사용자·앱 접근 로그, 정책 적용 이력, 차단 리포트 | 사내망, 운영망, AWS 접근 경로와 정책을 함께 설계 |
| EDR | Trend Micro Apex One / Endpoint Security | 관리자 단말, 서버, 악성코드 탐지·대응 | 탐지 이벤트, 격리·조치 이력, 에이전트 배포율 | 온프레미스 관리 콘솔 노출과 패치 운영도 통제 대상 |
| MDM/UEM | JumpCloud, Microsoft Intune | 단말 등록, 보안 정책, 패치, 디스크 암호화, 퇴사자 회수 | 디바이스 목록, 준수 상태, 패치 현황, 원격 잠금·삭제 | IdP, SASE, EDR과 조건부 접근을 연계해야 효과가 큼 |
| 배포관리 | Datadog CI Visibility | CI/CD 실행 이력, 실패 원인, 커밋·작업 연결 | 파이프라인 trace, 작업 로그, 커밋 SHA, 배포 지표 | 변경 승인과 운영 반영 승인은 ITSM/CI/CD 원천과 결합 필요 |
| 서비스 모니터링 | Datadog APM | 장애 탐지, 서비스 의존성, 배포 영향 분석 | trace, service map, SLO, 배포 전후 비교, 관련 로그 | 가용성·장애 대응 증적 보조. DR 복구시험 자체를 대체하지 않음 |

### 5.2 서버·DB 접근제어

| 후보 | 적용 범위 | ISMS-P 활용 | 대표 증적 | 한계/검증 포인트 | 공식 링크 |
|---|---|---|---|---|---|
| QueryPie ACP | 데이터베이스, 시스템, Kubernetes, 웹 애플리케이션 접근제어 | `2.3` 외부자 보안, `2.5` 인증·권한, `2.6` 접근통제, `3.2` 개인정보처리시스템 접속기록 보완 | 승인 이력, 권한 정책, 감사 로그, 세션 기록, 명령·쿼리 로그 | DB 쿼리의 업무 목적, 개인정보 조회 사유, 앱 관리자 행위는 내부 로그와 결합 필요 | <https://www.querypie.com/en/solutions/acp> |
| Teleport | 서버, 데이터베이스, Kubernetes, 애플리케이션, 클라우드 리소스 접근 | SSH key와 bastion 의존도 축소, JIT 접근, RBAC, 세션 녹화, SIEM 연동 | 접근 요청·승인, 역할 정책, 감사 이벤트, 세션 녹화, structured audit export | Enterprise 기능, self-hosted 운영 보안, SSO/SCIM/IdP 연동 범위 확인 필요 | <https://goteleport.com/docs/> |

### 5.3 인증·인가 관리

| 후보 | 강점 | ISMS-P 활용 | 대표 증적 | 한계/검증 포인트 | 공식 링크 |
|---|---|---|---|---|---|
| Okta Workforce Identity | SSO, MFA, lifecycle, governance, device assurance, 다수 SaaS 연동 | 퇴사자 비활성화, MFA, SaaS 접근권한, 외부자 계정관리 | 사용자·그룹 목록, 앱 할당, 로그인 이력, MFA 현황, lifecycle 이벤트 | AWS IAM, EKS RBAC, DB 권한의 실제 의미는 별도 매핑 필요 | <https://www.okta.com/products/workforce-identity/> |
| Keycloak (OSS) | OIDC, OAuth 2.0, SAML, LDAP/AD federation, 중앙 인증 | 내부 서비스 SSO, 표준 프로토콜 기반 인증, 자체 호스팅 IAM | realm/client 설정, 사용자·역할, 로그인 이벤트, 2FA 설정 | OSS 자체 운영이므로 HA, 백업, 패치, 감사로그 보존, 관리자 접근통제를 직접 책임져야 함 | <https://www.keycloak.org/> |
| Microsoft Entra ID | Entra ID, conditional access, identity governance, Entra Suite와 네트워크 접근 연계 | Microsoft 365, Azure, SaaS 접근통제와 계정 생명주기 | 사용자·그룹, conditional access, access review, sign-in log, provisioning log | P1/P2, Governance, Suite 등 라이선스별 기능 차이를 구매 전 확인 | <https://www.microsoft.com/en-us/security/business/microsoft-entra> |

### 5.4 Datadog 중심 관측·보안 스택

| 후보 | 적용 범위 | ISMS-P 활용 | 대표 증적 | 한계/검증 포인트 | 공식 링크 |
|---|---|---|---|---|---|
| Datadog Cloud SIEM | 로그 기반 위협 탐지와 조사 | `2.9` 로그, `2.11` 사고대응, 보안 이벤트 상관분석 | 보안 신호, 탐지 룰, Log Explorer, 조사 뷰 | 원본 로그 보존, 위변조 방지, 사고 보고서 승인 절차는 별도 필요 | <https://www.datadoghq.com/product/cloud-siem/> |
| Datadog Cloud Security | CSPM, 취약점, ID 위험, 규정 준수 상태 | `2.1` 자산, `2.6` 접근위험, `2.10` 취약점, 클라우드 설정 점검 | 설정 오류, 취약점, ID 위험, 보안 자세 리포트 | ISMS-P 전체 통제와 1:1 매핑은 아니므로 내부 통제 해석 필요 | <https://www.datadoghq.com/product/cloud-security/> |
| Datadog Infrastructure Monitoring | 호스트, 컨테이너, Kubernetes, 클라우드 인프라 관측 | 리소스 인벤토리, 운영 상태, 장애·용량 증적 | 호스트 인벤토리, Resource Catalog, 태그, 메트릭, 대시보드 | Datadog에 연결되지 않은 SaaS·단말·계약 자산은 별도 CMDB 필요 | <https://www.datadoghq.com/product/infrastructure-monitoring/> |
| Datadog Code Security | SAST, SCA, IAST, IaC, secret scanning | `2.8` 개발보안, `2.10` 취약점 관리 | 코드 finding, SCA finding, PR 코멘트, 조치 상태 | 코드 리뷰 승인, 예외 승인, 릴리스 차단 정책은 CI/CD와 결합 필요 | <https://www.datadoghq.com/product/code-security/> |
| Datadog CI Visibility | CI/CD 파이프라인 관측 | `2.8` 개발보안, `2.9` 변경·배포 이력 | 파이프라인 trace, 작업 로그, 커밋 SHA, 실패율, 실행 시간 | 배포 승인권자, 운영 반영 승인, 롤백 승인 기록은 별도 필요 | <https://www.datadoghq.com/product/ci-cd-monitoring/> |
| Datadog APM | 애플리케이션 trace, 서비스 맵, SLO | 서비스 운영, 장애 대응, 배포 영향 분석 | trace, 서비스 맵, 배포 전후 비교, SLO, 관련 로그 | 개인정보 조회 목적과 관리자 행위 감사는 앱 감사 로그로 별도 수집 필요 | <https://www.datadoghq.com/product/apm/> |

### 5.5 취약점·코드 보안

| 후보 | 강점 | ISMS-P 활용 | 대표 증적 | 한계/검증 포인트 | 공식 링크 |
|---|---|---|---|---|---|
| Wiz | agentless 클라우드 자산, attack path, CSPM, CIEM, DSPM, 컨테이너·Kubernetes 보안 | 클라우드 위험 설명, 취약점 우선순위, 노출·권한·데이터 리스크 분석 | 자산 그래프, attack path, 취약점, 설정오류, 권한 위험 | Datadog을 표준 관측 플랫폼으로 둘 경우 중복 범위와 비용 조정 필요 | <https://www.wiz.io/platform/wiz-cloud> |
| SonarQube (OSS/Enterprise) | 코드 품질, SAST, quality gate, PR 분석 | 개발보안, 코드리뷰 보조, 릴리스 품질 기준 | quality gate, issue list, PR decoration, 보안 hotspot | 브랜치/PR/언어/보안 규칙은 에디션별 차이 확인 필요 | <https://www.sonarsource.com/products/sonarqube/> |
| GitHub Advanced Security | code scanning, secret scanning, dependency review, Dependabot | GitHub 중심 개발조직의 보안 게이트 | code scanning alert, secret alert, dependency alert, PR check | GitHub Enterprise/라이선스 전제. 런타임 취약점과 클라우드 설정은 별도 필요 | <https://github.com/security/advanced-security> |
| Datadog Code Security | 개발부터 런타임까지 취약점 우선순위화 | AppSec 결과와 APM/서비스 소유자 연결 | SAST/SCA/IAST finding, 서비스-코드 매핑, PR 코멘트 | 코드 저장소 권한과 리뷰 승인 정책은 GitHub/GitLab 쪽 원천 기록 필요 | <https://www.datadoghq.com/product/code-security/> |

### 5.6 DLP, SASE, 원격접속 보호

| 후보 | 적용 범위 | ISMS-P 활용 | 대표 증적 | 한계/검증 포인트 | 공식 링크 |
|---|---|---|---|---|---|
| Privacy-i | 엔드포인트 DLP | 단말에서 개인정보·중요정보 반출 통제, 보조저장매체·출력·전송 통제 검토 | DLP 정책, 위반 이벤트, 차단 로그, 사용자별 반출 이력 | 공식 기능표, OS 지원, macOS/Windows 정책 차이, 개인정보 탐지 패턴 확인 필요 | <https://www.somansa.com/> |
| Netkiller ISMS | Google Drive DLP, Google Workspace 보안 관리 | Google Drive 외부 공유, 민감정보 탐지, 장기 로그인 이력 보관 | Drive scan report, 외부 공유 목록, 로그인 이력, 파일 권한 리포트 | Google Workspace 외 데이터 저장소, Microsoft 365, 로컬 단말 DLP는 별도 필요 | <https://netkiller.com/> |
| Zscaler | Zero Trust SASE, SSE, ZIA/ZPA, DLP, CASB | 원격근무, 웹·SaaS 접근, 데이터 유출 방지, VPN 대체 | 사용자·앱 접근 로그, DLP 이벤트, CASB finding, 정책 변경 이력 | 운영망/개발망/AWS 접근 경로와 IdP 조건부 접근 설계 필요 | <https://www.zscaler.com/> |
| Prisma SASE (Palo Alto Networks) | SASE, Prisma Access, SD-WAN, DLP, SaaS Security | 사용자·앱·데이터·디바이스 보호와 네트워크 접근통제 | 접속 로그, DLP 이벤트, 보안 정책, 위협 차단 리포트 | Palo Alto 네트워크·SOC 생태계와 결합 시 효과가 커짐 | <https://www.paloaltonetworks.com/sase> |
| Cloudflare One | SASE, ZTNA, CASB, SWG, Magic WAN | 원격접속, SaaS 보호, 내부 앱 접근통제, 인터넷 경계 보호 | Access 로그, Gateway 로그, CASB finding, 정책 변경 | DNS/CDN/Zero Trust 경로를 Cloudflare로 수렴할지 아키텍처 판단 필요 | <https://www.cloudflare.com/sase/> |

### 5.7 엔드포인트, MDM, 단말 보안

| 후보 | 적용 범위 | ISMS-P 활용 | 대표 증적 | 한계/검증 포인트 | 공식 링크 |
|---|---|---|---|---|---|
| Trend Micro Apex One / Endpoint Security | EDR/EPP, 엔드포인트 위협 탐지·대응 | 관리자 단말, 업무 단말, 서버 악성코드 대응 | 에이전트 배포율, 탐지 이벤트, 격리·조치 이력, 정책 현황 | 온프레미스 Apex One 콘솔을 쓸 경우 콘솔 접근통제와 패치 운영도 심사 대상 | <https://www.trendmicro.com/en_us/business/products/endpoint-security.html> |
| JumpCloud | Cloud Directory, SSO/MFA, UEM/MDM, patch, asset | 중소·성장 조직의 ID·단말 통합 운영 | 디바이스 인벤토리, 준수 상태, 패치 상태, 사용자 생명주기, MFA | Microsoft Entra ID/Okta와 중복되는 IdP 역할을 명확히 정해야 함 | <https://jumpcloud.com/> |
| Microsoft Intune | UEM/MDM, endpoint compliance, app management, remote help, endpoint privilege | Microsoft 365/Microsoft Entra 중심 단말 보안과 조건부 접근 | 디바이스 준수 상태, 구성 프로파일, 앱 배포, 원격 작업 | macOS/Linux/모바일 정책 범위와 E5/P1/Plan 2 라이선스 차이 확인 필요 | <https://www.microsoft.com/en-us/security/business/microsoft-intune> |

### 5.8 도입 판단 기준

| 판단 축 | 질문 |
|---|---|
| 통제 소유권 | 이 제품이 탐지·수집만 하는가, 승인·차단·회수까지 책임지는가 |
| 증적 품질 | 심사 기간별 내보내기, API, 원본 로그, 변경 이력, 담당자 승인 기록을 제공하는가 |
| 데이터 위치 | 개인정보나 로그가 어느 국가·리전에 저장되고, 처리위탁 계약이 가능한가 |
| IdP 연동 | Okta, Microsoft Entra ID, Keycloak, Google Workspace, GitHub와 SCIM/SAML/OIDC 연동이 충분한가 |
| 운영 범위 | AWS, EKS, ECS, EC2, RDS, SaaS, 관리자 단말, Google Workspace까지 실제 인증범위를 덮는가 |
| 락인과 중복 | Datadog, Wiz, SASE, EDR, MDM 간 중복 비용과 운영 책임을 설명할 수 있는가 |
| ISMS-P 매핑 | 제품 리포트를 ISMS-P 항목별 증적으로 재분류할 수 있는가 |
