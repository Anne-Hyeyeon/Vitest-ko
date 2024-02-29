---
description: Vitest의 특징
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

[테스트 구성](features.md)에 대해 더 알아보세요.

***

## 관찰 모드 (watch mode)

```bash
$ vitest
```

소스 코드나 테스트 파일을 수정할 때, Vitest는 모듈 그래프를 지능적으로 탐색하여 관련된 테스트만 재실행합니다. [이는 Vite에서의 HMR 작동 방식과 유사합니다](https://twitter.com/antfu7/status/1468233216939245579).

개발 환경에서 vitest는 기본적으로 `관찰 모드(watch mode)`로 시작되며, `process.env.CI`가 설정된 CI 환경에서는 실행 모드로 자동 전환됩니다. `vitest watch` 또는 `vitest run`을 사용하여 원하는 모드를 명시적으로 지정할 수 있습니다.

***

## 즉시 사용할 수 있는 일반적인 웹 관용구

ES Module / TypeScript / JSX / PostCSS 를 바로사용 가능합니다.

***

## 스레드

기본적으로 Vitest는 Tinypool( Piscina의 경량 버전)을 통해 `node:worker_threads`를 사용하여 테스트 파일들을 여러 스레드에서 실행하며, 이를 통해 테스트를 동시에 진행할 수 있습니다.&#x20;

만약 여러분의 테스트가 멀티 스레딩과 호환되지 않는 코드를 실행한다면, `--pool=forks`를 사용하여 Tinypool을 통해 `node:child_process`를 사용하는 여러 프로세스에서 테스트를 실행할 수 있도록 전환할 수 있습니다.

테스트를 단일 스레드 또는 프로세스에서 실행하려면, [poolOptions](https://vitest.dev/config/#pooloptions)를 참조하세요.

Vitest는 각 파일의 환경을 분리하여 한 파일에서의 환경 변수 변경이 다른 파일에 영향을 미치지 않습니다. 실행 성능을 우선시하고자 할 경우, CLI에 --no-isolate 옵션을 전달하여 분리 기능을 해제할 수 있습니다.

***

## 테스트 선별

Vitest는 개발에 더 집중할 수 있도록 실행할 테스트의 범위를 세밀하게 조정하여 테스트 속도를 빠르게 하는 여러 방식을 제공합니다.

[테스트 선별](https://vitest.dev/guide/filtering)에 대해 더 알아보세요.

***

## 테스트 동시 실행

연속적인 테스트에서 `.concurrent`를 사용하여 테스트를 병렬로 실행할 수 있습니다.

```typescript
import { describe, it } from 'vitest'

// The two tests marked with concurrent will be run in parallel
describe('suite', () => {
  it('serial test', async () => { /* ... */ })
  it.concurrent('concurrent test 1', async ({ expect }) => { /* ... */ })
  it.concurrent('concurrent test 2', async ({ expect }) => { /* ... */ })
})
```

스위트에 `.concurrent`를 사용하면, 그 안의 모든 테스트가 병렬로 실행됩니다.

<pre class="language-typescript"><code class="lang-typescript"><strong>import { describe, it } from 'vitest'
</strong>
// All tests within this suite will be run in parallel
describe.concurrent('suite', () => {
  it('concurrent test 1', async ({ expect }) => { /* ... */ })
  it('concurrent test 2', async ({ expect }) => { /* ... */ })
  it.concurrent('concurrent test 3', async ({ expect }) => { /* ... */ })
})
</code></pre>

`.skip`, `.only`, `.todo`를 병렬 스위트와 테스트에도 사용할 수 있습니다. [API 참조](https://vitest.dev/api/#test-concurrent)에서 더 많은 정보를 읽어보세요.

{% hint style="info" %}
병렬 스위트"는 테스트 프레임워크에서 여러 테스트 케이스를 동시에 실행하는 기능을 말합니다.  일반적으로, 테스트 스위트(test suite)는 관련된 테스트 케이스들의 집합을 의미하며, 이들 테스트 케이스는 통상적으로 순차적으로 실행됩니다.&#x20;

하지만 "병렬 스위트"에서는 이러한 테스트 케이스들이 병렬로, 즉 동시에 실행되어 전체 테스트 실행 시간을 단축시킬 수 있습니다. 이 방식은 특히 테스트 케이스 간에 의존성이 없고, 복수의 CPU 코어를 효율적으로 사용할 수 있을 때 유용합니다. (영어 원문에는 없는 내용)
{% endhint %}

{% hint style="info" %}
경고

병렬 테스트를 실행할 때, 스냅샷과 단언문의 경우  올바른 테스트가 감지되도록 지역 테스트 컨텍스트에서 제공하는 expect를 사용해야 합니다.
{% endhint %}

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

스냅샷에 대해 더 알아보려면 [스냅샷 ](https://vitest.dev/guide/snapshot)문서를 참조하세요.

***

## Chai, 그리고  Jest의 expect와의 호환성

Chai 및 Jest expect와의 호환성 단언(assertion)을 위해 Chai가 내장되어 있으며, Jest의 expect와 호환되는 API를 제공합니다.



매처(matcher)를 추가하는 타사 라이브러리를 사용하는 경우, `test.globals`를 `true`로 설정하면 더욱 향상된 호환성을 얻을 수 있습니다.

***

## 함수 모킹킹

`vi` 객체에서 Jest와 호환되는 API를 이용한 모킹을 위해 [Tinyspy](https://github.com/tinylibs/tinyspy)가 내장되어 있습니다.

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

Vitest는 DOM과 브라우저 API를 모킹하기 위해 [happy-dom](https://github.com/capricorn86/happy-dom)과 [jsdom](https://github.com/jsdom/jsdom)를 모두 지원합니다. 이들은 Vitest와 함께 제공되지 않으므로, 별도로 설치할 필요가 있습니다.

```bash
npm i -D happy-dom
# or
npm i -D jsdom
```

설치 후, 설정 파일에서 `환경 옵션(environment)`을 변경하세요:

```typescript
// vitest.config.ts
import { defineConfig } from 'vitest/config'

export default defineConfig({
  test: {
    environment: 'happy-dom', // or 'jsdom', 'node'
  },
})
```

모킹에 대해 더 알아보려면 [Mocking ](https://vitest.dev/guide/mocking)페이지를 참조하세요.

***

## 커버리지

Vitest는 `v8`을 통한 네이티브 코드 커버리지와 [Istanbul](https://istanbul.js.org/)을 통한 계측된 코드 커버리지를 지원합니다.

```json
{
  "scripts": {
    "test": "vitest",
    "coverage": "vitest run --coverage"
  }
}
```

[Coverage ](https://vitest.dev/guide/coverage.html)페이지에서 더 많은 내용을 알아볼 수 있습니다.

***

## 소스 코드 내 테스트

Vitest는 [Rust의 모듈 테스트](https://doc.rust-lang.org/book/ch11-03-test-organization.html#the-tests-module-and-cfgtest)와 유사하게, 구현 코드와 함께 소스 코드 내에서 테스트를 실행할 수 있는 방법을 제공합니다.

이 방법은 테스트가 구현과 동일한 클로저를 공유하게 하여, 내보내지 않고도 비공개 상태에 대해 테스트할 수 있게 합니다. 동시에, 개발 시 피드백 루프를 더 가깝게 가져옵니다.

```typescript
// src/index.ts

// 구
export function add(...args: number[]) {
  return args.reduce((a, b) => a + b, 0)
}

// 소스 코드 내 테스트 스위트
if (import.meta.vitest) {
  const { it, expect } = import.meta.vitest
  it('add', () => {
    expect(add()).toBe(0)
    expect(add(1)).toBe(1)
    expect(add(1, 2, 3)).toBe(6)
  })
}
```

[소스 코드 내 테스팅](https://vitest.dev/guide/in-source)에 대해 더 알아보세요.

***

## 벤치마킹

Vitest 0.23.0부터, [Tinybench](https://github.com/tinylibs/tinybench)를 통해 성능 결과를 비교하는 벤치마크 테스트를 `bench` 함수를 사용하여 실행할 수 있습니다.

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

Vitest 0.25.0부터, 타입 회귀를 찾기 위한 테스트를 작성할 수 있습니다. Vitest는 사용자에게 이해하기 쉽고 유사한 API를 제공하는 `expect-type` 패키지와 함께 제공됩니다.

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
