---
description: Vitest만의 특징
---

# Features

* [Vite](https://vitejs.dev/)의 설정, 변환기, 해석기를 사용합니다.
* 개인앱 설정을 그대로 활용하여 테스트를 실행합니다.
* [HMR(Hot Module Replacement)처럼 ](https://twitter.com/antfu7/status/1468233216939245579)신속하고 지능적으로 동작하는 감시 모드를 제공합니다.
* Vue, React, Svelte, Lit, Marko 등의 컴포넌트 테스팅을 지원합니다.
* TypeScript 및 JSX를 바로 사용할 수 있습니다.
* ESM을 우선적으로 지원하며, 최상위 수준에서 await 사용이 가능합니다.
* [Tinypool](https://github.com/tinylibs/tinypool)을 통해 멀티 스레딩을 지원합니다.
* [Tinybench](https://github.com/tinylibs/tinybench)를 사용하여 벤치마킹을 지원합니다.
* 테스트 스위트 및 개별 테스트에 대한 필터링, 타임아웃, 동시 실행을 지원합니다.
* [워크스페이스](https://vitest.dev/guide/workspace) 지원으로 프로젝트 관리가 용이합니다.
* [Jest와 호환되는 스냅샷 ](https://vitest.dev/guide/snapshot)기능을 제공합니다.
* Chai를 내장하여 assert 기능을 제공하며, Jest의 expect API와 호환됩니다.
* [Tinyspy](https://github.com/tinylibs/tinyspy) 모킹을 쉽게 할 수 있습니다.
* [happy-dom](https://github.com/capricorn86/happy-dom) 또는 [jsdom ](https://github.com/jsdom/jsdom)을 통해 DOM 모킹을 지원합니다.
* [v8 ](https://v8.dev/blog/javascript-code-coverage)또는 [istanbul](https://istanbul.js.org/)을 이용한 코드 커버리지를 제공합니다.
* Rust 스타일의 [소스 내 테스팅](https://vitest.dev/guide/in-source) 을 지원합니다.
* expect-type을 사용한 [타입 테스팅](https://github.com/mmkal/expect-type)을 지원합니다.

{% hint style="info" %}
[테스트 작성 방법 배우기 ](https://vueschool.io/lessons/your-first-test?friend=vueuse)(동영상)
{% endhint %}

***

## 테스트, 개발 및 빌드 단계사이의 설정 공유

Vite의 설정, 변환기, 해석기 그리고 플러그인. Vite 앱에서 사용하는 동일한 설정으로 테스트 실행이 가능합니다.&#x20;

[테스트 구성](features.md)에 대해 더 알아보고 싶다면, 해당 링크를 방문하세요.



***

## 관찰 모드 (watch mode)

```bash
$ vitest
```

사용자의  코드나 테스트 파일을 수정할 때,  Vitest는 모듈 그래프를 똑똑하게 탐지할 수 있을  뿐만 아니라 관련 테스트만을 골라 수정합니다. [이는 HMR이 Vite에서 적용된 것과 비슷합니다.](https://twitter.com/antfu7/status/1468233216939245579)

`vitest` 는 기본 개발 환경에서는 `watch mode`  로 실행되며 `process.env.CI`  가 존재하는 CI 환경에서는 `run mode`  로 아주   똑똑하게 실행됩니다. 사용자는 `vitest watch` 또는 `vitest run` 모드를 희망에 따라 명시적으로 표시할 수 있습니다.



***

## 즉시 사용할 수 있는 웹 이디엄

ES Module / TypeScript / JSX / PostCSS 를 별도의 세팅 없이 즉시사용 가능합니다.



***

## 스레드

기본적으로 Vitest는 Tinypool( Piscina의 경량화된 버전 )을 통한 `node:worker_threads`를 사용하는 멀티 스레드를 이용해 테스트를 진행합니다. 그리고 이러한 방식은 여러테스트를  동시에  진행될 수 있게 합니다.&#x20;

만약 사용자의 테스트가 멀티 스레딩과 호환되지 않는 러닝 코드라고 해도, 사용자는  테스트가  Tinypool의 `node:child_process`  를 이용해 멀티 프로세스로 테스트를 동작시키는 `--pool=forks` 로   전환이 가능합니다.

싱글 스레드 프로세스로 테스트를 진행시키기를 원한다면, poolOptions 링크를 방문하시기 바랍니다.

Vitest는 각각 파일들의 환경을 분리시키기 때문에 한파일에서의    env 변화가 다른 파일에 영향을 끼치지 않습니다. 분리는 `--no-isolate` 명령어를 CLI에 입력함으로서 이루어집니다.



***

## 테스트 필터링

Vitest는 사용자가 개발에 집중할 수 있도록 테스트 실행 범위를 좁혀 테스트 속도를 향상시키는 다양한 방법을 제공합니다.



***

## 테스트 동시 실행

연속적인      테스트에서`.concurrent` 를 이용하면 테스트를 동시에 진행할 수 있습니다.

```typescript
import { describe, it } from 'vitest'

// The two tests marked with concurrent will be run in parallel
describe('suite', () => {
  it('serial test', async () => { /* ... */ })
  it.concurrent('concurrent test 1', async ({ expect }) => { /* ... */ })
  it.concurrent('concurrent test 2', async ({ expect }) => { /* ... */ })
})
```

`.concurrent`를 테스트 스위트에  사용한다면, 모든 테스트는 병렬로 실행됩니다.

```typescript
import { describe, it } from 'vitest'

// All tests within this suite will be run in parallel
describe.concurrent('suite', () => {
  it('concurrent test 1', async ({ expect }) => { /* ... */ })
  it('concurrent test 2', async ({ expect }) => { /* ... */ })
  it.concurrent('concurrent test 3', async ({ expect }) => { /* ... */ })
})
```

사용자는 `.skip` ,`.only` , `.todo` 를 테스트 동시 진행과  테스트 스위트를 위해 사용할 수 있습니다. [API reference](https://vitest.dev/api/#test-concurrent)에서 더 많은 내용을 확인하세요.



***

## 스냅샷

[Jest와 호환](https://jestjs.io/docs/snapshot-testing)되는 스냅샷이 지원됩니다.

```typescript
import { expect, it } from 'vitest'

it('renders correctly', () => {
  const result = render()
  expect(result).toMatchSnapshot()
})
```

[Snapshot ](https://vitest.dev/guide/snapshot.html)문서에서 더 많은 내용을 확인하세요.



***

## Chai, 그리고  Jest의 expect와의 호환성

assertion을 위한 [Chai ](https://www.chaijs.com/)라이브러리가 빌트인 되어 있으며,[ Jest의 expect](https://jestjs.io/docs/expect)와 호환되는 API를 가지고 있습니다.



사용자가  matcher를   추가하는 3자 라이브러리를 사용할 경우, `test.globals`  를 `true` 로 설정해야 더 나은 호환성을 제공합니다.&#x20;



***

## 함수 모의 (Mocking)&#x20;

`vi` object의는 Jest와 호환되는함 API에는  함수 모의를 [Tinyspy ](https://github.com/tinylibs/tinyspy)가 탑재되어 있습니다.

```typescript
import { expect, vi } from 'vitest'

const fn = vi.fn()

fn('hello', 1)

expect(vi.isMockFunction(fn)).toBe(true)
expect(fn.mock.calls[0]).toEqual(['hello', 1])

fn.mockImplementation(arg => arg)

fn('world', 2)

expect(fn.mock.results[1].value).toBe('world')
```

Vitest는 DOM과 브라우저 API를 모의하기 위한 [happy-dom](https://github.com/capricorn86/happy-dom)과 [jsdom](https://github.com/jsdom/jsdom)를 모두 지원합니다. 다만 Vitest와 함께 제공되지 않기 때문에, 따로 설치해야 할 필요가 있습니다.

```bash
npm i -D happy-dom
# or
npm i -D jsdom
```

그 후에, 사용자의 설정 파일에서`envirnment` 옵션을 변경해야 합니다.

```typescript
// vitest.config.ts
import { defineConfig } from 'vitest/config'

export default defineConfig({
  test: {
    environment: 'happy-dom', // or 'jsdom', 'node'
  },
})
```

[Mocking ](https://vitest.dev/guide/mocking.html)페이지에서 더 많은 내용을 학습할 수 있습니다.



***

## 커버리지

Vitest는 `v8` 을 통한 Native 코드 커버리지를  제공합니다.  `istanbul` 을 통해  계측된코드 커버리지 역시 제공합니다.

```json
{
  "scripts": {
    "test": "vitest",
    "coverage": "vitest run --coverage"
  }
}
```

[Coverage ](https://vitest.dev/guide/coverage.html)페이지에서 더 많은 내용을 학습할 수 있습니다.



***

## # In-source Testing (소스 코드 내부에서 테스트 코드 작성)

Vitest는 소스 코드를 구현하면서 테스트를 진행하는 방식을 지원합니다. 이것은 [Rust의 모듈 테스트](https://doc.rust-lang.org/book/ch11-03-test-organization.html#the-tests-module-and-cfgtest)랑 비슷한 방식입니다.&#x20;



이러한 방식은 테스트가구현체와 동일한 클로저를 공유하도록 합니다. 그리고 내보내기 없이 개별 상태를 대상으로 테스트하는 것을 가능하게 만듭니다. 한편, 이 방식은 개발자 모드에서 피드백 루프 클로저도 제공합니다.

```typescript
// src/index.ts

// the implementation
export function add(...args: number[]) {
  return args.reduce((a, b) => a + b, 0)
}

// in-source test suites
if (import.meta.vitest) {
  const { it, expect } = import.meta.vitest
  it('add', () => {
    expect(add()).toBe(0)
    expect(add(1)).toBe(1)
    expect(add(1, 2, 3)).toBe(6)
  })
}
```

[In-source testing ](https://vitest.dev/guide/in-source.html)페이지에서 더 많은 내용을 학습할 수 있습니다.



***

## 벤치마킹

Vitest 0.23.0 버전부터, 사용자는 [Tinybench](https://github.com/tinylibs/tinybench)의 [`bench` ](https://vitest.dev/api/#bench)기능을 이용해 결과물의 퍼포먼스를 비교해볼 수 있습니다.

```typescript
import { bench, describe } from 'vitest'

describe('sort', () => {
  bench('normal', () => {
    const x = [1, 5, 4, 2, 3]
    x.sort((a, b) => {
      return a - b
    })
  })

  bench('reverse', () => {
    const x = [1, 5, 4, 2, 3]
    x.reverse().sort((a, b) => {
      return a - b
    })
  })
})
```



***

## 타입 테스팅

Vitest 0.25.0 부터, 사용자는 타입 회귀를 잡기 위한 테스트를 작성할 수 있게 되었습니다. Vitest는 API를 쉽고 비슷하게 이해할 수 있도록 도와주는 `expect-type` 패키지를  제공합니다.

```typescript
import { assertType, expectTypeOf } from 'vitest'
import { mount } from './mount.js'

test('my types work properly', () => {
  expectTypeOf(mount).toBeFunction()
  expectTypeOf(mount).parameter(0).toMatchTypeOf<{ name: string }>()

  // @ts-expect-error name is a string
  assertType(mount({ name: 42 }))
})
```
