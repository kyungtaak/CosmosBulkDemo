# 워크샵 에이전트 작업 지침

Standardization template material: Copyright (c) Microsoft Corporation.
Original MIT terms are preserved in [scripts/LICENSE.md](scripts/LICENSE.md).

## 리포 규칙

시크릿을 커밋하지 않는다. 실제 키·리소스 식별자는 환경 변수와 예시 placeholder로 구분한다. 미커밋 변경과 저자의 실습 내용을 보존한다. 루트 README는 워크샵 진입점이며, 랩은 `labs/NN-kebab-name`으로 배치한다.

이 저장소는 C형 예외다. Markdown을 본문으로, 기존 .NET 프로그램을 검증 자료로 유지한다. 표준화를 위해 notebook을 추가하지 않는다. .NET 9와 기존 NuGet 패키지 버전을 보존한다.

실제 결정 시트와 검사 로그는 Git에서 제외한 `local-only/`에 보관한다. `.venv/`, Azure 로그인 정보와 실제 설정 값을 커밋하지 않는다. 원본 저장소 이력을 다시 가져오지 않는다.

## 콘텐츠·노트북 스타일

루트 README는 전체 메타데이터, 랩 README는 `title`, `duration_minutes`, `last_updated`를 사용한다. 문서마다 H1 하나를 사용하고, 실습 단계는 H2·H3으로 구분한다.

소요 시간·비용·실행 결과를 추정해 확정하지 않는다. 원문에 없는 절차는 TODO 주석으로 남긴다. 노트북이 나중에 승인되어 추가되면 코드 셀은 `outputs: []`, `execution_count: null`을 유지한다. 랩 전용 자산은 해당 랩의 `assets/`, 여러 랩의 공용 자산은 `shared-assets/`에 둔다.

## 검증 하네스

검사 환경은 Bash 기반 devcontainer 또는 Git Bash다. gitleaks 8.19 이상, Python 3.10 이상과 `scripts/requirements-workshop.txt`의 검사 의존성을 준비한다. 설치·업데이트는 승인받는다.

검사 1~3은 working tree와 로컬 전체 refs의 이력을 검사한다. Git 이력이 없거나 shallow clone이면 이력 검사는 완료되지 않는다. 신규 골격과 승인된 squash의 첫 커밋 전에는 검사 1~3에만 `--scope tree`를 사용하고 이력 검사를 미실행으로 보고한다. 첫 커밋 후 아래 기본 범위로 9종을 다시 실행한다.

```bash
# 실패한 검사 뒤에는 진행하지 않는다.
set -e
# (1) 시크릿: working tree + 전체 이력
python3 scripts/workshop_checks.py 1
# (2) 개인 엔드포인트·구독/테넌트 ID: working tree + 전체 이력
python3 scripts/workshop_checks.py 2
# (3) 노트북 출력: 현재 제거 + 이력 검출 분류
python3 scripts/workshop_checks.py 3
# (4) 공백·비ASCII 경로
python3 scripts/workshop_checks.py 4
# (5) 루트 frontmatter: 필수 키·자료형·허용값·날짜
python3 scripts/workshop_checks.py 5
# (6) 상대 링크·이미지: Markdown 제목·참조형 링크·HTML 포함
python3 scripts/workshop_checks.py 6
# (7) 랩 frontmatter: 필수 키·자료형·날짜
python3 scripts/workshop_checks.py 7
# (8) 빈 이미지 alt-text
python3 scripts/workshop_checks.py 8
# (9) 다른 랩의 전용 assets 참조
python3 scripts/workshop_checks.py 9
```

종료 상태는 0이면 PASS, 1이면 검출, 2 이상이면 입력·도구 오류다. 앞 단계의 오류를 마지막 PASS 메시지로 덮지 않는다.

검사 4~9는 현재 파일을 검사한다. 검사기에서 제외하는 실행 환경 경로는 `.git`, `node_modules`, `.venv`, `venv`, `__pycache__`이며, Git 제외 설정만으로 모든 검사에서 제외되지는 않는다. 실제 자료는 UTF-8로 저장한다. 검사 명령·범위·대상 SHA·종료 상태·원본 로그 위치를 기록한다.

시크릿·개인 식별자 검출 시 즉시 중단하고 값은 재출력하지 않는다. 실노출을 allowlist로 숨기지 않는다. 검사 2는 Cosmos DB endpoint를 지원하지 않으므로 `documents.azure.com` 주소와 미지원 확장자·스크린샷은 별도 검토한다. 검사 6은 상대 파일의 존재 여부만 확인하며 외부 URL이나 앵커 검증은 별도다.

## 백로그·DO NOT

- `validated_on`은 사람이 최종 실습을 완주한 날짜만 기록한다.
- 원본의 `dotnet rerun`, 중첩 프로젝트 생성 경로와 파티션 키 표기를 저자가 검토한다.
- 원본의 key 출력·설정 파일 입력 안내, ReadMany 결과와 리소스 정리 절차를 저자가 검토한다.
- 승인 없이 삭제·분할·병합·라이선스 전환·이력 변경을 수행하지 않는다.
- 검사를 생략·완화하거나 미실행을 통과로 표시하지 않는다.
- 클라우드 배포·삭제, 유료 실행, 원격 push와 공개 전환을 자동으로 수행하지 않는다.
- Microsoft 소유권·보안 정책·CLA 봇·CSS 지원을 추정하지 않는다.
- TODO가 남은 콘텐츠를 E2E 검증 완료나 공개 준비 완료로 표시하지 않는다.
