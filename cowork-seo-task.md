# Cowork 작업 지시 — 배수민 법률사무소 SEO/AEO/GEO 개선

## 컨텍스트
- 정적 1페이지 사이트. 핵심 파일: `index.html`(1318줄), `robots.txt`, `sitemap.xml`, `manifest.json`
- 배포: Cloudflare Pages, 대표 도메인 `https://baesumin.com`(apex, www→apex 301 완료)
- GitHub: https://github.com/jeonwoochul0515-cell/baesumin (main)
- 업종: 법률(규제). **과장·단정·승률·비교광고 표현 금지.** 근거·출처 명시, 불확실 시 상담 권고 톤.
- 적용 기준 문서: `NAVER_SEO_최적화_가이드.md`, `SEO_GEO_AEO_최적화_가이드.md`(전역). 화면에 없는 내용을 스키마에만 넣지 말 것.

## 사전 확인(사용자에게 받아야 진행 가능한 값)
- [ ] 네이버 Search Advisor 소유확인 코드
- [ ] 구글 Search Console 소유확인 코드
- [ ] GA4 측정 ID (`G-XXXXXXXXXX` 대체)
- [ ] 운영 채널 URL(블로그/인스타/유튜브/카카오채널 등) — JSON-LD `sameAs`용. 없으면 생략.
- [ ] 정확한 우편번호·주소 표기(현재 JSON-LD postalCode=47590 / WHOIS=616-100 불일치 → 하나로 통일)

---

## ✅ 터미널에서 이미 완료한 항목 (커밋 45a68a5)
- [x] **A. robots.txt** — Yeti·AI 검색봇(OAI-SearchBot/ChatGPT-User/PerplexityBot/Claude) 명시 허용
- [x] **C(일부). geo 좌표** — LegalService에 GeoCoordinates 추가 (sameAs는 채널 URL 받아야 함 → 미완)
- [x] **D. FAQ 섹션(#faq) + FAQPage JSON-LD** — 5개 Q&A 1:1 동기화, 내비 링크 추가
- [x] **E. dateModified** — LegalService에 2026-06-23
- [x] **H. 이미지 alt** — 프로필 img alt 보유, 나머지는 CSS 배경(해당없음). 추가 작업 불필요

## ⬜ Cowork에서 할 남은 항목 (사용자 입력 필요)

## 작업 체크리스트 (위→아래 순서, 각 항목 verify 포함)

### A. robots.txt 강화 — AI 검색/RAG 봇 + Yeti 명시
`robots.txt`를 아래로 교체. 검색·AI 검색봇은 허용, 학습봇은 권리 판단(기본은 허용 유지).
```
User-agent: *
Allow: /

# 네이버 검색로봇
User-agent: Yeti
Allow: /

# AI 검색/응답 크롤러(인용 노출용)
User-agent: OAI-SearchBot
Allow: /
User-agent: ChatGPT-User
Allow: /
User-agent: PerplexityBot
Allow: /
User-agent: Perplexity-User
Allow: /
User-agent: Claude-SearchBot
Allow: /
User-agent: Claude-User
Allow: /

Sitemap: https://baesumin.com/sitemap.xml
```
- verify: `curl -s https://baesumin.com/robots.txt`에 Sitemap·Yeti 라인 존재.

### B. 소유확인 메타 활성화 (index.html 34~37줄 주석 해제 + 실코드)
- 발급 코드로 `<meta name="naver-site-verification">`, `<meta name="google-site-verification">`를 `<head>`에 노출.
- verify: `view-source`에 메타 존재. Search Advisor·GSC에서 소유확인 통과.

### C. JSON-LD 보강 (index.html 46~83줄 LegalService)
기존 LegalService에 다음 필드 추가(화면 정보와 1:1):
- `"geo": {"@type":"GeoCoordinates","latitude":35.1905895,"longitude":129.0737616}` (본문 지도 좌표와 동일)
- `"priceRange"`는 비공개면 생략(임의 값 금지).
- 운영 채널이 있으면 `"sameAs":[...]` 추가.
- verify: https://validator.schema.org/ 에 붙여 오류 0.

### D. FAQ 섹션 신설 (AEO/GEO 최대 레버리지) ★
1. 본문에 `#faq` 섹션 추가(연락처 섹션 앞 권장). 5~8개 질문.
   - 헤딩을 **질문 그대로** H3로: "여성폭력 피해를 당했는데 어떻게 상담받나요?", "형사사건 국선변호도 가능한가요?", "상담 비용은 어떻게 되나요?", "부산 외 지역도 상담 가능한가요?" 등.
   - 각 답변은 **두괄식 40~60단어**, 단정·승률 표현 금지, 사실+상담 권고.
2. 화면 FAQ와 **1:1 동기화된 `FAQPage` JSON-LD** 추가.
   - 각 Q&A는 문맥 없이도 완결되게(주어 포함).
- verify: 화면 FAQ 개수 == FAQPage mainEntity 개수. schema.org validator 통과.

### E. dateModified / 신선도
- JSON-LD(LegalService 또는 별도 WebPage)에 `"dateModified":"2026-06-23"` 추가, 갱신 시 함께 수정.
- sitemap.xml `lastmod`도 동기화.
- verify: 두 값이 같은 날짜.

### F. AEO 본문 리라이트 (경력/업무분야)
- `#practice`(612줄~) 각 업무분야 첫 문장을 **두괄식 정의**로: "○○은 …" 한 문장.
- 가능하면 경력을 수치/연도로 구체화(근거 있는 사실만). 과장 금지.
- verify: 각 업무분야 카드 첫 문장이 그 분야를 1문장으로 정의.

### G. GA4 측정 (index.html 1201~1206줄)
- 주석 해제 + 실제 측정 ID로 `G-XXXXXXXXXX` 2곳 교체.
- verify: 실시간 보고서에 접속 1건 집계.

### H. 이미지 alt 점검
- 모든 `<img>`에 의미 있는 alt(키워드 나열 금지). 프로필/로고 등.
- verify: `grep -c '<img' index.html` == alt 보유 개수.

---

## 마무리
- 변경은 의미 단위로 커밋(가이드 9번). 예: "robots AI봇 정책", "FAQ 섹션+FAQPage 추가".
- 푸시 후 Cloudflare Pages 자동 배포 확인 → `https://baesumin.com`에서 반영 확인.
- 네이버/구글에 sitemap 재제출, IndexNow/수집요청.
- 배포 후 `site:baesumin.com` 질의로 색인 확인.

## 제외(건드리지 말 것)
- Tailwind CDN(`cdn.tailwindcss.com`) 로딩 방식 — 과거 프로덕션 빌드 교체 시 데스크탑 레이아웃 깨져 CDN으로 복원한 이력 있음. 성능 개선 명목으로 임의 교체 금지.
