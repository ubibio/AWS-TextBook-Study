1. DNS란

1) DNS 서비스

Domain Name System의 약어로, 네트워크 통신을 위한 주소 체계를 문자 형태인 도메인으로 매핑하여 연결하는 서비스

IP 주소를 문자 형태의 도메인 주소로 매핑해서 사용하는 서비스

도메인 주소를 이용한 통신과정

웹 서버의 IP 주소를 도메인 주소로 사용하고자 ongja.com 도메인을 구매하고 등록한다. DNS 서버는 해당 도메인의 IP 주소를 기록해둔다.

사용자는 도메인의 IP 주소를 알기 위해 DNS 서버에 ongja.com의 IP 주소를 요청한다.

이 요청은 UDP 53번 포트를 사용하는 DNS 프로토콜을 통해 수행된다.

DNS 서버가 ongja.com의 IP 주소를 응답하면 사용자는 해당 IP 주소로 통신한다.

2) 도메인 구조

루트 도메인: 모든 도메인 주소의 마지막 온점

탑 레벨 도메인 (TLD): .com, .co.kr, .org 등

세컨드 레벨 도메인 (SLD): TLD 바로 앞 영역 (예: cloudneta)

서브 도메인: SLD 앞에 붙는 구분자 (예: blog.cloudneta.net의 blog)

3) DNS 서버 종류

도메인 구조를 나누는 이유는 영역별로 DNS 네임 서버의 관리 주체를 분리하기 위해서이다.

루트 네임 서버: 루트 도메인을 관리하며 TLD 네임 서버 정보를 응답. 전 세계에 13개 존재

TLD 네임 서버: TLD를 관리하며 해당 TLD의 SLD 네임 서버 정보를 응답

예: .com TLD는 google.com SLD 정보를 알고 있음

SLD 네임 서버: 실제 도메인의 최종 관리 서버 (권한 있는 네임 서버)

DNS 해석기: 사용자와 네임 서버 사이의 중계자 역할 수행. 사용자 요청을 받아 각 네임 서버로 질의하고 최종 IP 주소를 사용자에게 전달

4) DNS 통신 흐름

사용자 PC가 blog.cloudneta.net의 IP 주소를 알아내기 위해 DNS 서버에 질의한다.

DNS 해석기는 루트 네임 서버에 질의 → .net TLD 네임 서버 주소 획득

TLD 네임 서버에 blog.cloudneta.net 질의 → cloudneta.net SLD 네임 서버 주소 획득

SLD 네임 서버에 질의 → blog.cloudneta.net의 최종 IP 주소 획득

해석된 IP 주소를 사용자에게 전달

5) DNS 레코드 유형

DNS 레코드는 도메인 요청 처리 방식을 정의함

A 레코드: IPv4 주소 매핑

예: blog.cloudneta.net A 52.219.60.13

AAAA 레코드: IPv6 주소 매핑

예: blog.cloudneta.net AAAA 2001:A10::2001

NS 레코드: 네임 서버 주소 매핑

예: net NS a.gtld-servers.net.

CNAME 레코드: 도메인 별칭

예: www.cloudneta.net CNAME cloudneta.net

2. Amazon Route 53 서비스

1) 주요 기능

도메인 이름 등록, 호스팅 영역 생성, 레코드 작성 기능 제공

도메인 이름 등록: 도메인 등록대행소 역할 수행

호스팅 영역 생성: SLD 네임 서버 역할, 고가용성 네임 서버 구성

레코드 작성: 도메인 요청 처리 방식을 정의 (단순/가중치/지연 시간/장애 조치 라우팅 등)

2) 라우팅 정책

단순 라우팅: 하나 또는 여러 대상 중 랜덤 선택

가중치 기반 라우팅: 가중치(0~255) 기반 분산 응답

지연 시간 기반 라우팅: 사용자와 가장 가까운 리전 선택

장애 조치 라우팅: 액티브/패시브 구성, 장애 발생 시 패시브 승격

3) 도메인 이름 생성

AWS 콘솔에서 Route 53 접속 → 등록된 도메인 메뉴 → 도메인 등록

호스팅 영역 생성 후 진입

레코드 생성: 이름 = 'test', 유형 = A, 값 = 8.8.8.8, 라우팅 = 단순

test.cloudnetajuju.com → nslookup/PING 시 8.8.8.8 응답 확인

3. CDN이란

Contents Delivery Network: 지리적으로 분산된 캐시 서버를 통해 콘텐츠를 빠르게 전달하는 기술

1) CDN 환경

기존 방식: 오리진 서버에서 직접 콘텐츠 전달 → 부하 증가, 지연 증가

CDN 방식: 지역별 캐시 서버에 콘텐츠 동기화 → 인접 서버에서 응답

2) 캐싱 방식

캐시 미스:

요청 → 캐시 서버에 콘텐츠 없음 → 오리진 요청 → 저장 후 사용자에게 전달

캐시 히트:

요청 → 캐시 서버에 콘텐츠 있음 → 직접 사용자에게 전달

정적 캐싱: 이미지, JS, CSS 등 변경 없는 콘텐츠. TTL 설정 후 캐시 서버에 미리 복제

동적 캐싱: 요청마다 달라지는 콘텐츠. TTL=0 설정, 캐시 서버 통과 후 오리진 전달

4. Amazon CloudFront란

AWS의 글로벌 CDN 서비스. 엣지 로케이션에 콘텐츠 캐싱, 최적의 응답 제공

1) 구성 요소

오리진: EC2, ELB, S3, 온프레미스 등

Distribution: Web 또는 RTMP로 배포 단위 구성

리전 엣지 캐시: 중간 캐시 계층

엣지 로케이션: 사용자와 가장 가까운 콘텐츠 제공 지점

2) 주요 기능

정적/동적 콘텐츠 처리 최적화

HTTPS 미지원 오리진도 HTTPS 중계 가능

다수 오리진 설정 및 선택 지원

서명된 URL/쿠키 통한 접근 제어

3) CloudFront로 CDN 구성하기

1단계: CloudFormation으로 기본 인프라 배포 (리전: sa-east-1)

스택 이름: CF-LAB → 옵션 생략 → 스택 생성

2단계: Route 53 설정 및 EC2 웹 페이지 접속

호스팅 영역 → A 레코드 생성: IP = EC2 퍼블릭 IP, 정책 = 단순

Chrome 개발자 도구 → Network 탭으로 지연 시간 측정 (예: 60ms)

3단계: Certificate Manager로 SSL 인증서 등록

리전: us-east-1 → 퍼블릭 인증서 요청 → 도메인 입력 → DNS 검증 선택 → Route 53 레코드 자동 생성

4단계: CloudFront Distribution 생성

원본 도메인: EC2 퍼블릭 DNS

HTTP only, 자동 압축 비활성화, 뷰어 정책: HTTP and HTTPS

캐시 설정: Legacy

WAF: 비활성화, 가격: 모든 엣지 로케이션

대체 도메인: cdn.도메인입력, SSL 인증서 연결, 루트 객체: /index.php, IPv6 비활성화

5단계: Route 53에서 CloudFront 별칭 레코드 생성

이름: cdn, 유형: A, 별칭 대상: CloudFront 배포 도메인

최초 접속 시: CloudFront의 엣지 로케이션에 캐시 없음 → 오리진에서 콘텐츠 전달 → x-cache: Miss from cloudfront

재접속 시: 엣지 로케이션에서 콘텐츠 제공 → x-cache: Hit from cloudfront

