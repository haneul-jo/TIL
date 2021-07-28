---
title: 타입스크립트 에러 처리 (Error Handling)
date: "2021-07-28T14:44:39.888Z"
template: "post"
draft: false
slug: "typescript-error-handling"
category: "TypeScript"
tags:
    - "TypeScript"
    - "Error"
description: "에러처리를 잘 하면 유지보수성, 안정성이 높다. 예측 가능한 에러와 예측 불가능한 에러를 구분하여 잘 처리하여야 한다."
---

**에러처리를 잘 하면 유지보수성, 안정성이 높다.**

-   예측 가능한 에러는 (expected error) `error state` 라고 부른다.

    ❓ 왜 `error state`라고 부르는지는 잘 모르겠으나, 명확한 에러인 경우는 사전에 미리 알고 대응이 가능하여 에러상태를 알수있다. 라는 표현이라고 생각하기로 했다.

-   예측 불가능한 에러 (unexpected error)는 `exception` 라고 부른다.
-   exception처리는 try → catch → finally 로 처리, 에러에 대한 처리를 할 수 있는 곳에서 catch 하는 것이 좋다.
-   TypeScript `catch(error)`에 에러타입은 `any` 타입이다. 어떠한 타입정보도 전달되지 않아 `instanceof`를 사용할 수 없다. →❗️ 어떤 에러인지 파악이 불가능

    -   예상할 수 있는 (expected error), `error state`는 `type`을 정의하여 사용한다.

        ```tsx
        type SuccessState = {
            result: true;
        };

        type ErrorState = {
            result: false;
            reason: "timeout" | "unknown"; // 에러는 이유를 알려주는 게 좋다.
        };

        type ResultState = SuccessState | ErrorState;

        class TimeoutError extends Error {}

        // throw
        class ServerClientThrow {
            tryConnect(): void {
                throw new TimeoutError();
            }
        }

        // state
        class ServerClientState {
            tryConnect(): ResultState {
                return {
                    result: false,
                    reason: "timeout"
                };
            }
        }
        ```
