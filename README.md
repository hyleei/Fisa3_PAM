# 🐧 리눅스 사용자 관리 및 PAM 설정

이 프로젝트는 리눅스 사용자 관리 방법과 PAM(Pluggable Authentication Modules)을 사용한 강화된 비밀번호 정책 설정 방법을 보여줍니다.

## 📋 목차

1. [사용자 관리](#사용자-관리)
2. [PAM 설정](#pam-설정)
3. [설정 확인](#설정-확인)

## 👥 사용자 관리

### 사용자 및 그룹 생성

다음 명령어를 사용하여 새로운 사용자와 그룹을 생성합니다:

```bash
# 새 그룹 생성
sudo groupadd -g 1001 newgroup

# user01 생성 (기본 사용자와 같은 그룹)
sudo useradd -m -u 1001 -g 1000 -s /bin/bash user01

# user02와 user03 생성 (새 그룹에 속함)
sudo useradd -m -u 1002 -g 1001 -s /bin/bash user02
sudo useradd -m -u 1003 -g 1001 -s /bin/bash user03
```

### 사용자 및 그룹 생성 확인

생성된 사용자와 그룹을 확인하려면 다음 명령어를 사용합니다:

```bash
# 사용자 정보 확인
grep -E "^(username|user01|user02|user03)" /etc/passwd
```
![image](https://github.com/user-attachments/assets/a89adaad-ce14-4f63-87cd-c56b4be9987c)

```bash
# 그룹 정보 확인
grep -E "^(username|newgroup)" /etc/group
```
![image](https://github.com/user-attachments/assets/78686078-db02-4144-8883-fcc035094ac3)

## 🔒 PAM 설정

### 비밀번호 정책 설정

최소 8자 이상의 비밀번호를 요구하는 정책을 설정하려면:

1. PAM 설정 파일 수정:
   ```bash
   sudo nano /etc/pam.d/common-password
   ```

2. 다음 줄을 추가 또는 수정:
   ```
   password requisite pam_pwquality.so retry=3 minlen=8
   ```

3. 필요한 경우 pam_pwquality 모듈 설치:
   ```bash
   sudo apt-get install libpam-pwquality
   ```

## ✅ 설정 확인

설정이 제대로 적용되었는지 확인하려면:

1. PAM 설정 파일 재확인:
   ```bash
   sudo cat /etc/pam.d/common-password | grep pam_pwquality
   ```
   ![image](https://github.com/user-attachments/assets/d8e515b7-ef0f-4e41-abcb-1f77fe52b87d)


2. 새 사용자 생성 및 비밀번호 설정 테스트:
   ```bash
   sudo adduser testuser
   ```
   ![image](https://github.com/user-attachments/assets/dca5695b-9d65-4deb-847d-9c3587670676)

---
