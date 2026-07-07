# ISMS-P 클라우드 SaaS 런북

AWS 기반 클라우드 네이티브 SaaS 조직이 한국 ISMS-P 인증을 준비할 때 참고할 기술 대응 런북입니다.

## 전제

- 대상 환경: AWS, EC2, ECS, EKS, CI/CD, IaC, 컨테이너 이미지 레지스트리, 관리형 데이터 저장소
- 개인정보 프로필: 민감정보 또는 고위험 개인정보를 처리할 수 있는 보수적 기준
- 정리 범위: ISMS-P 전체 기준 중 기술 통제, 증적 자동화, 개인정보 처리 단계별 기술 이슈
- 우선순위: 심사 부적합 가능성과 증적 확보 난이도를 기준으로 `P0`, `P1`, `P2` 구분

## 문서

- [소개용 HTML 랜딩 페이지](./site/index.html): 클라우드 네이티브 ISMS-P와 솔루션 맵을 설명하는 발표용 페이지
- [ISMS-P 클라우드 SaaS 기술 준비 런북](./docs/isms-p-cloud-saas-runbook.md): 기준, 리스크, AWS 기준선
- [내부 구축 플랫폼 모듈](./docs/internal-platform-modules.md): 내부에 만들어야 할 증적·운영 플랫폼
- [외부 SaaS/보안 제품 후보 비교](./docs/external-saas-solutions.md): 외부 도입 후보와 평가 기준
- [Datadog ISMS-P 커버리지](./docs/datadog-isms-p-coverage.md): Datadog으로 보완 가능한 규제 항목과 한계
- [구축 로드맵과 운영 체크리스트](./docs/implementation-roadmap.md): 실행 순서, 수용 기준, 구매 체크리스트

## 사용 방법

1. `P0` 항목부터 현재 운영 상태와 증적 보유 여부를 점검합니다.
2. 내부 구축 플랫폼 모듈과 외부 SaaS 도입 후보를 분리해 예산과 로드맵을 잡습니다.
3. 각 통제 영역별 증적 산출물을 월별 또는 배포 단위로 자동 생성하도록 구현합니다.
4. 심사 전에는 운영명세서, 자산 목록, 로그 보존, 접근권한 검토, 개인정보 흐름도를 같은 기준으로 갱신합니다.

## 기준일과 출처

작성 기준일은 2026-07-08입니다. ISMS-P 기준은 KISA ISMS-P 공식 자료를 기준으로 삼고, 외부 SaaS 제품명은 각 벤더의 공식 제품 페이지 표기를 따릅니다.

- [KISA ISMS-P 제도소개](https://isms.kisa.or.kr/main/ispims/intro/)
- [KISA ISMS-P 자료실](https://isms.kisa.or.kr/main/ispims/notice/)