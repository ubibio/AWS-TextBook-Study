# 8장 AWS IAM 서비스 요약

## 1. AWS 리소스 관리 도구
- **AWS 관리 콘솔**: 웹 UI로 AWS 리소스 생성·관리
- **AWS CLI**: 명령줄 기반 통합 도구 (Windows/macOS/Linux)
- **AWS SDK**: 프로그래밍 언어별 API 라이브러리 (Python, Java 등)

## 2. AWS API
- **정의**: 서비스 간 통신 매개체
- **특징**:
  - 인증 필요
  - 명세서 기반 요청
- **활용**:
  - 모든 AWS 리소스는 API로 제어 가능
  - CloudTrail로 API 로깅 가능

## 3. IAM(Identity and Access Management)

### 3.1 정의
- AWS 리소스 접근을 안전하게 제어
- 인증과 인가 기능 제공
- 추가 요금 없음

### 3.2 구성 요소

| 구성 요소 | 설명 |
|-----------|------|
| 루트 사용자 | 계정 생성 시 최초 생성, 전체 권한 보유 |
| IAM 사용자 | 개별 사용자, 고유 자격 증명 보유 |
| IAM 그룹 | 사용자 집합, 권한 일괄 관리 가능 |
| IAM 정책 | JSON 형식 권한 정의 객체 |
| IAM 역할 | 임시 자격 증명 기반 접근 권한 |
| 보안 주체 | 인증된 사용자 또는 애플리케이션 |

### 3.3 인증과 인가
- **인증**: 자격 증명 확인 (비밀번호, 액세스 키)
- **인가**: 정책에 따른 권한 확인

## 4. IAM 사용자 실습

### 4.1 사용자 생성
- **admin 사용자**
  - 권한: `AdministratorAccess`
  - 콘솔 접근 + CLI 액세스 키 발급

- **viewuser 사용자**
  - 권한: `ViewOnlyAccess`
  - 콘솔 접근 설정

### 4.2 로그인 및 권한 확인
- `admin`: EC2 인스턴스 재부팅 가능
- `viewuser`: EC2 인스턴스 종료 시도 → 권한 없음

## 5. IAM 정책 실습

### 5.1 admin 사용자에 인라인 정책 부여

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "ec2:RebootInstances",
        "ec2:StopInstances"
      ],
      "Resource": "*"
    }
  ]
}
```

- 적용 후 재부팅 시도 → 거부됨

## 6. IAM 역할 실습

### 6.1 역할 확인
- EC2 인스턴스(S3IAMRoleEC2)는 IAM 사용자 자격 증명 없이도 역할로 AWS 리소스 접근 가능

### 6.2 명령어 실습
- `aws s3 ls` → 정상 동작
- `aws ec2 describe-vpcs` → 역할에 권한 없을 경우 오류 발생

## 핵심 정리
- IAM은 사용자·역할 기반 보안 제어 시스템
- **정책(JSON)**으로 세부 권한 설정
- 역할은 자격 증명 없이도 리소스 접근 가능
- 루트 사용자는 제한적 사용 권장
