# Humanize Korean 적용 요약

대상 파일:

- `README.md`
- `docs/*.md`
- `site/*.html`

적용 내용:

- Open Design 산출물 5개 페이지와 공통 CSS를 `site/`에 반영했다.
- 문서와 HTML의 기준일을 `2026-07-08`로 맞췄다.
- 번역투, 기계적인 문장, 어색한 영문 혼용을 줄였다.
- 제품명과 대소문자를 `QueryPie ACP`, `Teleport`, `Datadog Cloud SIEM`, `Datadog Cloud Security`, `Datadog Infrastructure Monitoring`, `Okta`, `Keycloak (OSS)`, `Microsoft Entra ID`, `Datadog Code Security`, `Wiz`, `SonarQube`, `GitHub Advanced Security`, `Privacy-i`, `Netkiller ISMS`, `Zscaler Zero Trust Exchange`, `Prisma SASE (Palo Alto Networks)`, `Cloudflare One`, `Trend Micro Apex One`, `JumpCloud`, `Microsoft Intune`, `Datadog CI Visibility`, `Datadog APM` 기준으로 정리했다.
- Datadog 페이지는 `all-in-one 모니터링 플랫폼` 메시지와 `ISMS-P 인증을 보장하지 않는다`는 한계를 함께 유지했다.

검증 결과:

- HTML 링크와 active navigation 검사: 통과
- 5개 HTML 페이지의 `styles.css` 로딩: 통과
- 1200px / 390px 렌더링 overflow 검사: 통과
- Datadog 페이지 all-in-one 문구 노출: 통과
- Datadog 인증 보장 caveat 노출: 통과
- 깨진 치환 흔적 검색: 통과

<!-- HUMANIZE-SUMMARY
original_chars_estimate: 122952
final_chars: 124138
change_rate_estimate: conservative multi-file pass, roughly under 10%
category_counts:
  A-1/A-2/A-5/A-7 translationese: many -> low
  B-1/B-2 English-overuse: many -> intentional product terms only
  C-10 heading colon pattern: low -> low
  D-1 signature phrase: low -> low
  H-1 front-loaded connectors: low -> low
self_check:
  proper_nouns_numbers_quotes_preserved: pass
  change_rate_guard: pass
  genre_preserved: pass
  register_preserved: pass
  residual_s1_patterns: pass
  no_added_rhetoric: pass
grade: B
grade_reason: 다중 파일에 제품명 교정을 포함한 보수 윤문이다. 의미와 구조를 보존했고, 제품명·대소문자·주요 어색한 표현을 정리했다.
highlights:
  - "Datadog으로 클라우드 네이티브 ISMS-P 운영 가시성을 하나로 연결합니다." -> "Datadog은 클라우드 네이티브 ISMS-P 운영 신호를 한곳에 모읍니다."
  - "Resource Inventory" -> "리소스 인벤토리"
  - "Palo Alto Prisma" -> "Prisma SASE (Palo Alto Networks)"
  - "Cloudflare" -> "Cloudflare One"
  - "Keycloak OSS" -> "Keycloak (OSS)"
-->
