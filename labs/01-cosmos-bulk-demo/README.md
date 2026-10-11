---
title: 01. Cosmos DB 대량 처리
duration_minutes:
last_updated: 2026-10-11
---

# 01. Cosmos DB 대량 처리

## 개요와 학습 목표

[.NET 콘솔 프로그램](src/Program.cs)은 bulk upsert, ReadMany, 파티션 키 쿼리, TransactionalBatch를 실행합니다. Cosmos DB 생성과 프로그램 실행을 마친 뒤 Portal에서 데이터를 확인합니다.

## 사전 요구사항

로컬 .NET SDK, Azure CLI, PowerShell과 Azure 구독을 준비합니다. Codespaces에서는 [devcontainer](../../.devcontainer/devcontainer.json)를 사용합니다.

<!-- TODO: 저자가 필요한 권한, Azure 비용과 최종 실행 환경별 완주 절차를 확인한다. -->

## 소요 시간

<!-- TODO: 실제 소요 시간을 측정하고 duration_minutes와 본문에 같은 값을 기록한다. -->

## 실습 단계

### 1. .NET SDK 준비

`dotnet --version`으로 SDK 설치를 확인합니다.

기존 프로젝트는 [src](src)에 있습니다. 아래는 원본의 프로젝트 생성 명령이며, 경로만 표준 랩 위치로 변경했습니다.

```bash
mkdir -p labs/01-cosmos-bulk-demo/src/Models
mkdir -p .vscode
cd labs/01-cosmos-bulk-demo/src
dotnet new console -n CosmosBulkDemo
dotnet add package Microsoft.Azure.Cosmos --version 3.54.0
dotnet add package Newtonsoft.Json --version 13.0.3
dotnet restore
dotnet rerun
```

명령이 정상적으로 실행되는지 확인합니다.

<!-- TODO: 원본의 dotnet rerun 명령과 dotnet new의 중첩 프로젝트 경로를 확인한다. 배포된 기존 프로젝트 실행 경로와 프로젝트 신규 생성 경로를 구분한다. -->

### 2. Cosmos DB 생성

```powershell
az login
az account set --subscription "<구독 이름 또는 ID>"
```

저장소 루트에서 [리소스 생성 스크립트](create-cosmosdb-demo.ps1)를 실행합니다.

```powershell
.\labs\01-cosmos-bulk-demo\create-cosmosdb-demo.ps1
```

출력된 `COSMOS_ENDPOINT`와 `COSMOS_KEY`를 [appsettings.json](src/appsettings.json) 또는 [launch.json](../../.vscode/launch.json)에 입력합니다. 실제 값을 넣은 파일은 커밋하지 마세요.

<!-- TODO: 원본 스크립트의 primary key 콘솔 출력과 커밋되는 설정 파일의 실제 값 입력 안내를 검토하고 안전한 자격 증명 취급 절차를 확정한다. -->

### 3. 프로그램 실행

저장소 루트에서 실행합니다.

```powershell
cd labs/01-cosmos-bulk-demo/src
dotnet clean
dotnet build
dotnet run
```

## 검증

Azure Portal에서 Cosmos DB 계정을 선택한 뒤 **Data Explorer → demoDB → orders**로 이동해 생성된 데이터를 확인합니다. 원본 안내의 파티션 키는 `/userID`입니다.

<!-- TODO: 원본의 /userID 표기와 생성 스크립트·모델의 /userId를 대조해 저자가 확정한다. -->

TTL 설정은 **Container → Scale & Settings → Settings → Time to Live**에서 확인합니다.

```powershell
# TTL Monitoring
az monitor metrics list `
  --resource "/subscriptions/<Subscription ID>/resourceGroups/<resource group name>/providers/Microsoft.DocumentDB/databaseAccounts/<cosmod DB account name>" `
  --metric "TotalRequestUnits, ThrottledRequests, StorageUsed" `
  --interval PT1H `
  --output table
```

<!-- TODO: 실제 Portal UI, metric 이름과 ReadMany 결과를 최종 실행 환경에서 확인하고 관찰값을 기록한다. -->

## 정리 (Clean-up)

<!-- TODO: 원본에 정리 절차가 없다. 생성 리소스와 삭제 범위, 비용 발생 중단 확인 방법을 저자가 작성한다. -->

## 트러블슈팅

실행 정책 오류가 발생하면 원본 안내의 명령을 사용합니다. 이 명령은 현재 사용자 범위의 PowerShell 실행 정책을 변경합니다.

```powershell
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
```

<!-- TODO: 실행 정책 변경의 필요성과 대안을 저자가 검토한다. -->

## 이전·다음

[이전: 워크샵 홈](../../README.md) · [다음: 워크샵 홈](../../README.md)
