# rAthena 플레이어 이모티콘 확장 패치

최신 kRO 클라이언트에서 플레이어 이모티콘 명령이 작동하지 않는 문제를 수정합니다.

최신 클라이언트는 기존 이모티콘 패킷 대신
`CZ_REQ_EMOTION_EXPANSION (0x0BE9)` 패킷을 사용합니다.

## 테스트 환경

- kRO 클라이언트: `2026-02-11`
- rAthena
- `/흥`
- `/두리번`
- `/쪽`

위 명령어들의 정상 작동을 확인했습니다.

## 적용 대상

다음 조건의 클라이언트를 대상으로 합니다.

    PACKETVER_MAIN_NUM >= 20230705

`HEADER_CZ_REQ_EMOTION_EXPANSION` 패킷을
`clif_parse_Emotion`으로 전달하고,
패킷의 4번 오프셋에서 이모티콘 번호를 읽도록 수정합니다.

현재 `2026-02-11` kRO 클라이언트에서 정상 작동을 확인했습니다.

더 최신 클라이언트에서도 동일하게 작동할 가능성이 있으나
아직 직접 테스트하지 않았습니다.

## 수정 파일

- `src/map/clif.cpp`
- `src/map/clif_packetdb.hpp`

## 적용 방법

rAthena 루트 폴더에서 실행합니다.

    git apply emotion-expansion-2023plus.patch

적용 후 rAthena를 다시 컴파일하면 됩니다.

## 문제 원인

현재 rAthena에서는 최신 클라이언트가 보내는
`CZ_REQ_EMOTION_EXPANSION` 패킷이 `clif_parse_dull`로 연결되어 있어
플레이어가 보낸 이모티콘 요청이 그대로 무시됩니다.

이 패치는 해당 패킷을 실제 이모티콘 처리 함수로 연결하여
최신 클라이언트에서도 플레이어 이모티콘을 사용할 수 있도록 수정합니다.
