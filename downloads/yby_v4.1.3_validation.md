# V4.1.3 실행 오류 수정 검증

2026-10-17 (한국 시간). Linux/Python 3.12/Chromium에서 확인했습니다.

- V4.1.1과 V4.1.2의 client_connect.ps1 비교: 코드 내용은 같고 V4.1.2에서 UTF-8 BOM 3바이트만 빠졌음을 확인했습니다.
- V4.1.3 클라이언트 스크립트가 V4.1.1의 원본 바이트와 정확히 동일함을 확인했습니다.
- 배포 PowerShell 4개 전부 UTF-8 BOM, 유효한 UTF-8 및 구문 트리 검사 통과.
- 자동검사 164개 통과. 6개는 BOM 검사, 원본→실행폴더→공개 접속 ZIP 바이트 보존, BOM 없는 스크립트 차단, Windows 구문 검사 호출 및 실패 시 설치 차단을 검사합니다.
- Windows 전용 구문 검사에서 예기치 않은 PowerShell 오류도 중단하도록 보완한 뒤 관련 6개 검사 재실행 통과.
- 최종 ZIP 압축 해제본의 인코딩 관련 6개 검사와 실제 부트스트랩 실행/로그인/업무 저장/중복 실행/원본 DB 보존 확인.
- V4.1.2에서 V4.1.3으로 실제 HTTPS 서버 업데이트 시험: 실행 중인 구버전 무변경, 설치 전 DB 바이트 동일, 기존 계정/비밀번호/카드/취소 이력/재보고 대기/행·메시지/첨부/인증서·개인키 보존 확인.
- 생성된 LAN_CLIENT.zip의 client_connect.ps1에도 BOM 유지 확인.
- 실제 Chromium에서 신뢰한 CA를 사용한 HTTPS 로그인, HTTP→HTTPS 이동, 로그아웃·창 닫기 후 다시 로그인 확인. 인증서 검증 우회 없음.
- 최종 ZIP CRC·매니페스트 SHA-256 및 고객 업무 DB/개인키/인증서 미포함 확인.

Windows PowerShell 5.1, Windows 인증서 저장소·DPAPI·NTFS·Edge·회사망은 여기서 직접 실행하지 않았습니다. Windows 전용 Parser.ParseFile 사전 검사는 구현과 모의 실패 검사를 했으며 실제 실행 결과는 해당 Windows PC에서 확인해야 합니다. RUN_LAN_WINDOWS.bat는 Windows에서 파일 교체 전에 이 검사를 실행합니다. 설치 후 VERIFY_WINDOWS.bat와 클라이언트 CHECK_CONNECTION.bat로 실제 환경을 확인하세요.

앞선 Linux의 PowerShell 구문 트리 검사는 Windows PowerShell 5.1의 인코딩 선택을 재현하지 못했습니다. 이번 수정은 인코딩 바이트 검사와 실제 Windows 실행 전 파서 검사를 함께 적용합니다.
