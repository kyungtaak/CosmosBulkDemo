---
type: workshop
title: Azure Cosmos DB 대량 처리 워크샵
description: .NET SDK로 Cosmos DB 대량 적재와 배치 읽기, 파티션 키 쿼리, TransactionalBatch를 실행하고 Portal에서 결과를 확인합니다.
level: intermediate
authors: [Sehee Kim]
contacts: ["@yujeny"]
duration_minutes:
tags: [cosmos-db, dotnet, bulk-execution]
language: ko
execution: [codespaces, local, portal]
status: draft
source: original
last_updated: 2026-10-11
validated_on:
original_content_date: 2025-10-25
---

# Azure Cosmos DB 대량 처리 워크샵

## 개요와 학습 목표

.NET SDK로 Cosmos DB에 주문 데이터를 적재하고 ReadMany, 파티션 키 쿼리, TransactionalBatch를 실행합니다. Markdown 가이드가 학습 경로이며, 기존 .NET 콘솔 프로그램을 검증 자료로 사용합니다.

## 사전 요구사항

.NET 9 SDK, Azure CLI, PowerShell과 Azure 구독이 필요합니다. 로컬 또는 Codespaces에서 코드를 실행하고 Azure Portal에서 데이터를 확인합니다.

<!-- TODO: 저자가 필요한 Azure 권한과 비용을 확인한다. -->

## 소요 시간

<!-- TODO: 사람이 실습 소요 시간을 측정한 뒤 duration_minutes와 본문에 같은 값을 기록한다. -->

## 학습 경로

| 랩 | 내용 |
|---|---|
| [01. Cosmos DB 대량 처리](01-cosmos-bulk-demo/README.md) | 환경 준비, Cosmos DB 생성, .NET 실행과 Portal 검증 |

[첫 랩 시작](01-cosmos-bulk-demo/README.md)

## 실행 방법

### 로컬

이 저장소를 VS Code에서 열고 [랩 안내서](01-cosmos-bulk-demo/README.md)를 따릅니다. 기존 프로그램은 [프로젝트 파일](01-cosmos-bulk-demo/src/CosmosBulkDemo.csproj)에 있으며, NuGet 패키지 버전은 원본과 같습니다.

### Codespaces와 Dev Container

[devcontainer 설정](.devcontainer/devcontainer.json)은 .NET 9, PowerShell, Azure CLI를 준비합니다. VS Code의 **Dev Containers: Reopen in Container** 명령으로 사용할 수 있습니다. 환경 생성 후 .NET 패키지를 복원하지만 Azure 로그인, 리소스 생성, 프로그램 실행은 자동으로 수행하지 않습니다.

<!-- TODO: 최종 devcontainer가 포함된 게시 브랜치와 실제 Codespaces 실행을 확인한 뒤 해당 브랜치를 가리키는 Open in Codespaces 배지를 추가한다. -->

### Portal

Portal은 코드 실행 후 데이터와 TTL 설정을 확인하는 단계입니다. 전체 실습은 Portal만으로 진행하지 않습니다.

## 검증

[랩의 검증 절차](01-cosmos-bulk-demo/README.md#검증)를 따릅니다.

<!-- TODO: 사람이 최종 콘텐츠를 선언한 실행 환경에서 완주한 날짜만 validated_on에 기록한다. -->

## 정리 (Clean-up)

실습을 마치면 [랩의 정리 절차](01-cosmos-bulk-demo/README.md#정리-clean-up)에 따라 실습 전용 리소스 그룹을 삭제합니다. 그룹 안의 모든 리소스와 데이터가 삭제되므로 보존할 자료를 먼저 확인하세요.

## 트러블슈팅

[랩 안내서](01-cosmos-bulk-demo/README.md#트러블슈팅)의 실행 정책 안내를 확인합니다.

## 출처와 라이선스

원본은 [kyungtaak/CosmosBulkDemo](https://github.com/kyungtaak/CosmosBulkDemo)입니다. 원본 코드와 실습 내용을 유지한 표준화 작업본입니다.

워크샵 코드와 스크립트·설정은 [MIT 라이선스](LICENSE.md), 이 안내서와 랩 안내서는 [CC BY-SA 4.0](LICENSE-DOCS)을 적용합니다. 표준화 템플릿의 원래 Microsoft 저작권 고지는 [MIT 라이선스 문서](LICENSE.md#third-party-template-material)에 함께 보존합니다. 이는 워크샵 자체의 Microsoft 소유권이나 지원 정책을 뜻하지 않습니다.
