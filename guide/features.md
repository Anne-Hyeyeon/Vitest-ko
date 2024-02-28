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
