---
description: 시작하기
---

# Getting Started

## Overview

Vitest는 Vite에 의해 만들어진 차세대 테스팅 프레임워크입니다.

Vitest 프로젝트의 배경에 대한 자세한 내용은 [Why Vitest ](https://vitest.dev/guide/why.html)에서 더 자세하게 알아볼 수 있습니다.



## 온라인에서 Vitest 실행해 보기

[StackBiltz ](https://stackblitz.com/edit/vitest-dev-vitest-cy6ihu?file=README.md\&initialPath=\_\_vitest\_\_/)에서 Vitest를 온라인으로 실행할 수 있습니다.  StackBiltz를 통해 Vitest를 브라우저에서도 바로 실행 가능합니다. StackBiltz를 이용하면 설치 없이도 로컬과 동일하게 환경설정이 가능합니다.



## Vitest를 프로젝트에 적용해 보기

[▶️  설치방법 동영상으로 보기](#user-content-fn-1)[^1]

npm

```bash
npm install -D vitest
```

yarn

```bash
yarn add -D vitest
```

pnpm

```bash
pnpm add -D vitest
```

bun

```bash
bun add -D vitest
```



{% hint style="info" %}
TIP

Vitest 1.0 requires Vite >=v5.0.0 and Node >=v18.00
{% endhint %}

위  방법들  중 하나를 택해  프로젝트의 `package.json` 에다가 `vitest` 의 사본을 설치할 수 있습니다.\
만약 클론 말고`vitest` 를 직접 실행하고 싶다면, `npx vitest` 를 시용할 수 있습니다. (`npx` 명령어는 npm과 Node.js에 포함되어 있습니다.)



`npx` 명령어는 로컬의 `node_modules/.bin` 으로부터 명령을 실행하거나, 명령어가 실행되기 위해 필요한 패키지를 설치합니다. 기본적으로, npx는 $PATH나 로컬 프로젝트 바이너리에 해당 명령어가 있는지를 확인한 후,  명령어를 실행할  것입니다. 만약 명령어를 찾을 수 없다면,  실행에 앞서 해당 명령어가 설치됩니다.\


## 테스트 코드를 작성해 보기

예시와 같이, 우리는 두 개의 숫자를 더하는 함수의 결과를 검증하는 간단한 테스트 코드를 작성해 볼 것입니다.

```javascript
// sum.js
export function sum(a, b) {
  return a + b
}
```

```javascript
// sum.test.js
import { expect, test } from 'vitest'
import { sum } from './sum'

test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3)
})
```

{% hint style="info" %}
TIP

기본적으로, 테스트의 경우 파일 이름이 ".test" 또는 ".spec"이 들어가야 합니다.
{% endhint %}



그 다음으로, 테스트를 실행하기 위해 `package.json` 에다가 아래와 같은 섹션을 추가합니다.

```json
{
  "scripts": {
    "test": "vitest"
  }
}
```



마지막으로, `npm run test` 명령어를 입력해 실행시킵니다. (또는 `yarn test` 나 `pnpm test`)  vitest는 아래와 같은 메시지를 출력합니다.

```
✓ sum.test.js (1)
  ✓ adds 1 + 2 to equal 3

Test Files  1 passed (1)
    Tests  1 passed (1)
  Start at  02:15:44
  Duration  311ms (transform 23ms, setup 0ms, collect 16ms, tests 2ms, environment 0ms, prepare 106ms)

```

vitest의 활용법에 대해 더 알고자 한다면, [API ](https://vitest.dev/api/)페이지를 참고할 수 있습니다.



## Vitest 설정하기

Vitest의 가장 큰 장점 중 하나는 Vite와의 통합 설정이 가능하다는 것입니다. `vite.config.ts` 파일이 존재할 경우, `vitest`는  루트의 `vite.config.ts`를 읽고 나서  Vitest의 플러그인과 설정을 Vite 앱과 통합시킵니다.

예를 들어, Vite의 [resolve alias](https://vitejs.dev/config/shared-options.html#resolve-alias)와[ plugins ](https://vitejs.dev/guide/using-plugins.html)과  같은 설정은 즉시 Vitest에도 적용됩니다.&#x20;

만약 Vite와 Vitest에 각각 다른 설정을 적용하고 싶다면, 다음과 같이 할 수 있습니다:

* `vitest.config.ts` 파일을 생성하세요. 이 파일은 (vite.config.ts 보다) 더 높은 우선순위를 갖게 됩니다. &#x20;
* CLI에서 `--config`옵션을 사용하세요. 예시는  다음과 같습니다.  `vitest --config ./path/to/vitest.config.ts`
* `vite.config.ts` 에 조건에 따라 다른 설정을 적용하려면,  `process.env.VITEST` 또는 `defineConfig` 의 `mode` 속성을  사용하세요. (이재파일이정의되지 않는 한, 기본적으로`test`로 설정 됩니다.&#x20;

Vitest는 Vite에서 지원하는 구성 파일과 확장 파일을 제공합니다  : `.js`, `.mjs`, `.cjs`, `.ts`, `.cts`, `.mts` 그러나 Vitest에서는 `json` 확장형은 지원되지 않습니다.

만약 Vite를 빌드 도구로 사용하고 있지 않다면,  config file에 있는`test` 프로퍼티를 이용해 Vitest를 구성할 수 있습니다 :&#x20;

```typescript
import { defineConfig } from 'vitest/config'

export default defineConfig({
  test: {
    // ...
  },
})
```

{% hint style="info" %}
Vite를 사용하지 않더라도, Vitest는 그것의 변환 파이프라인 때문에 Vite에 크게 의존합니다. 이러한 이유로, 당신은[ Vite documentation](https://vitejs.dev/config/)에 기술된 속성들을 Vitest에서도 구성할 수 있습니다.
{% endhint %}



만약 Vite를 이미 사용 중이라면, `test` 프로퍼티를 당신의 Vite Config 에 추가합니다. 또한, 당신의 설정 파일  상단에 [triple slash directive](https://www.typescriptlang.org/docs/handbook/triple-slash-directives.html#-reference-types-)를 사용하기 위해서는, Vitest types에 대한 참조를 추가해야 합니다.

```typescript
/// <reference types="vitest" />
import { defineConfig } from 'vite'

export default defineConfig({
  test: {
    // ...
  },
})
```

[Config Reference(설정 참조)](https://vitest.dev/config/) 에서 설정 옵션 리스트들을 확인할 수 있습니다.

{% hint style="info" %}
*   주의사항\
    \
    Vite와 Vitest에 대해 각각의 설정 파일을 가지는 경우, Vitest 설정 파일에다가 동일한 Vite 옵션을 정의해야 합니다.  왜냐하면 Vitest 설정 파일은 Vite를 확장하는 게 아니라, 덮어쓰기 때문입니다.  또한 Vite 설정과 Vitest 설정을 병합하기 위해  vite나 vitest/config 로부 mergeConfig 메서드를 가져와 사용할 수도 있습니다.\
    \


    vitest.config.mjs

    ```typescript
    import { defineConfig, mergeConfig } from 'vitest/config'
    import viteConfig from './vite.config.mjs'

    export default mergeConfig(viteConfig, defineConfig({
      test: {
        // ...
      }
    }))
    ```



    vite.config.mjs

    <pre class="language-typescript"><code class="lang-typescript">import { defineConfig } from 'vite'
    import Vue from '@vitejs/plugin-vue'

    export default defineConfig({
      plugins: [Vue()],
    })

    <strong>
    </strong></code></pre>

    그러나 각각의 파일을 생성하는 대신, 저희는 가급적Vite와 Vitest에 동일한 파일을 사용하는 것을 추천드리고 있습니다.
{% endhint %}

***

## 작업 공간  지원

[Vitest Workspaces](https://vitest.dev/guide/workspace.html)를 이용해, 같은 프로젝트 내에서  다양한 프로젝트 구성을 실행할 수 있습니다. `vitest.workspace` 파일에 작업 공간을 정의하는 파일 및 폴더 목록을 정의할 수 있습니다.  파일은 `js`/`ts`/`json` 확장명을 지원합니다. 이 기능은 모노레포 설정과 함께 사용하기에 좋습니다.

```typescript
import { defineWorkspace } from 'vitest/config'

export default defineWorkspace([
  // you can use a list of glob patterns to define your workspaces
  // Vitest expects a list of config files
  // or directories where there is a config file
  'packages/*',
  'tests/*/vitest.config.{e2e,unit}.ts',
  // you can even run the same tests,
  // but with different configs in the same "vitest" process
  {
    test: {
      name: 'happy-dom',
      root: './shared_tests',
      environment: 'happy-dom',
      setupFiles: ['./setup.happy-dom.ts'],
    },
  },
  {
    test: {
      name: 'node',
      root: './shared_tests',
      environment: 'node',
      setupFiles: ['./setup.node.ts'],
    },
  },
])
```

***

## CLI

Vitest가 설치된 프로젝트에서는, npm scripts 에서`vitest` 를 사용하거나, `npx vitest`를  이용해 직접적으로  vitest를 실행심키는 게  가능합니다.  다음은 Vitest 프로젝트를 만들었을 때 기본적으로 제공되는 npm 스크립트입니다.

```json
{
  "scripts": {
    "test": "vitest",
    "coverage": "vitest run --coverage"
  }
}
```

파일 변경을 감지하지 않고 테스트를 한 번만 실행시키기 위해서는, `vitest run` 을 하면 됩니다. 또한 `--port` 또는 `--https` 와 같은 추가 CLI 옵션을 지정할수 도 있습니다.  사용 가능한 CLI 옵션의 전체 목록을 보려면,  `run vitest --help`를 프로젝트에서 실행시키면 됩니다.

[Command Line Interface(CLI) ](https://vitest.dev/guide/cli.html)에 대해 더 알아보려면, 링크를 클릭하세요.

***

## IDE 통합&#x20;

Vitest는 고객의  테스트 경험을  향상시키기위해,  VSC 공식  확장 프로그램으로도 제공됩니다.

[VS Code MarketPlace에서 Vitest 설치해 보기](https://marketplace.visualstudio.com/items?itemName=ZixuanChen.vitest-explorer)

[IDE 통합](https://vitest.dev/guide/ide.html)에 대해 더 알아보기

***

## 사용  예시

| Example                 | Source                                                                                  | Playground                                                                                                                               |
| ----------------------- | --------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| `basic`                 | [GitHub](https://github.com/vitest-dev/vitest/tree/main/examples/basic)                 | [Play Online](https://stackblitz.com/fork/github/vitest-dev/vitest/tree/main/examples/basic?initialPath=\_\_vitest\_\_/)                 |
| `fastify`               | [GitHub](https://github.com/vitest-dev/vitest/tree/main/examples/fastify)               | [Play Online](https://stackblitz.com/fork/github/vitest-dev/vitest/tree/main/examples/fastify?initialPath=\_\_vitest\_\_/)               |
| `graphql`               | [GitHub](https://github.com/vitest-dev/vitest/tree/main/examples/graphql)               | [Play Online](https://stackblitz.com/fork/github/vitest-dev/vitest/tree/main/examples/graphql?initialPath=\_\_vitest\_\_/)               |
| `image-snapshot`        | [GitHub](https://github.com/vitest-dev/vitest/tree/main/examples/image-snapshot)        | [Play Online](https://stackblitz.com/fork/github/vitest-dev/vitest/tree/main/examples/image-snapshot?initialPath=\_\_vitest\_\_/)        |
| `lit`                   | [GitHub](https://github.com/vitest-dev/vitest/tree/main/examples/lit)                   | [Play Online](https://stackblitz.com/fork/github/vitest-dev/vitest/tree/main/examples/lit?initialPath=\_\_vitest\_\_/)                   |
| `marko`                 | [GitHub](https://github.com/vitest-dev/vitest/tree/main/examples/marko)                 | [Play Online](https://stackblitz.com/fork/github/vitest-dev/vitest/tree/main/examples/marko?initialPath=\_\_vitest\_\_/)                 |
| `mocks`                 | [GitHub](https://github.com/vitest-dev/vitest/tree/main/examples/mocks)                 | [Play Online](https://stackblitz.com/fork/github/vitest-dev/vitest/tree/main/examples/mocks?initialPath=\_\_vitest\_\_/)                 |
| `nextjs`                | [GitHub](https://github.com/vitest-dev/vitest/tree/main/examples/nextjs)                | [Play Online](https://stackblitz.com/fork/github/vitest-dev/vitest/tree/main/examples/nextjs?initialPath=\_\_vitest\_\_/)                |
| `playwright`            | [GitHub](https://github.com/vitest-dev/vitest/tree/main/examples/playwright)            |                                                                                                                                          |
| `preact-testing-lib`    | [GitHub](https://github.com/vitest-dev/vitest/tree/main/examples/preact-testing-lib)    | [Play Online](https://stackblitz.com/fork/github/vitest-dev/vitest/tree/main/examples/preact-testing-lib?initialPath=\_\_vitest\_\_/)    |
| `react-mui`             | [GitHub](https://github.com/vitest-dev/vitest/tree/main/examples/react-mui)             | [Play Online](https://stackblitz.com/fork/github/vitest-dev/vitest/tree/main/examples/react-mui?initialPath=\_\_vitest\_\_/)             |
| `react-storybook`       | [GitHub](https://github.com/vitest-dev/vitest/tree/main/examples/react-storybook)       | [Play Online](https://stackblitz.com/fork/github/vitest-dev/vitest/tree/main/examples/react-storybook?initialPath=\_\_vitest\_\_/)       |
| `react-testing-lib-msw` | [GitHub](https://github.com/vitest-dev/vitest/tree/main/examples/react-testing-lib-msw) | [Play Online](https://stackblitz.com/fork/github/vitest-dev/vitest/tree/main/examples/react-testing-lib-msw?initialPath=\_\_vitest\_\_/) |
| `react-testing-lib`     | [GitHub](https://github.com/vitest-dev/vitest/tree/main/examples/react-testing-lib)     | [Play Online](https://stackblitz.com/fork/github/vitest-dev/vitest/tree/main/examples/react-testing-lib?initialPath=\_\_vitest\_\_/)     |
| `react`                 | [GitHub](https://github.com/vitest-dev/vitest/tree/main/examples/react)                 | [Play Online](https://stackblitz.com/fork/github/vitest-dev/vitest/tree/main/examples/react?initialPath=\_\_vitest\_\_/)                 |
| `ruby`                  | [GitHub](https://github.com/vitest-dev/vitest/tree/main/examples/ruby)                  | [Play Online](https://stackblitz.com/fork/github/vitest-dev/vitest/tree/main/examples/ruby?initialPath=\_\_vitest\_\_/)                  |
| `solid`                 | [GitHub](https://github.com/vitest-dev/vitest/tree/main/examples/solid)                 | [Play Online](https://stackblitz.com/fork/github/vitest-dev/vitest/tree/main/examples/solid?initialPath=\_\_vitest\_\_/)                 |
| `svelte`                | [GitHub](https://github.com/vitest-dev/vitest/tree/main/examples/svelte)                | [Play Online](https://stackblitz.com/fork/github/vitest-dev/vitest/tree/main/examples/svelte?initialPath=\_\_vitest\_\_/)                |
| `sveltekit`             | [GitHub](https://github.com/vitest-dev/vitest/tree/main/examples/sveltekit)             | [Play Online](https://stackblitz.com/fork/github/vitest-dev/vitest/tree/main/examples/sveltekit?initialPath=\_\_vitest\_\_/)             |
| `typecheck`             | [GitHub](https://github.com/vitest-dev/vitest/tree/main/examples/typecheck)             | [Play Online](https://stackblitz.com/fork/github/vitest-dev/vitest/tree/main/examples/typecheck?initialPath=\_\_vitest\_\_/)             |
| `vitesse`               | [GitHub](https://github.com/vitest-dev/vitest/tree/main/examples/vitesse)               | [Play Online](https://stackblitz.com/fork/github/vitest-dev/vitest/tree/main/examples/vitesse?initialPath=\_\_vitest\_\_/)               |
| `vue-jsx`               | [GitHub](https://github.com/vitest-dev/vitest/tree/main/examples/vue-jsx)               | [Play Online](https://stackblitz.com/fork/github/vitest-dev/vitest/tree/main/examples/vue-jsx?initialPath=\_\_vitest\_\_/)               |
| `vue`                   | [GitHub](https://github.com/vitest-dev/vitest/tree/main/examples/vue)                   | [Play Online](https://stackblitz.com/fork/github/vitest-dev/vitest/tree/main/examples/vue?initialPath=\_\_vitest\_\_/)                   |
| `workspace`             | [GitHub](https://github.com/vitest-dev/vitest/tree/main/examples/workspace)             | [Play Online](https://stackblitz.com/fork/github/vitest-dev/vitest/tree/main/examples/workspace?initialPath=\_\_vitest\_\_/)             |



***

## Vitest를 이용한 프로젝트

* [unocss](https://github.com/unocss/unocss)
* [unplugin-auto-import](https://github.com/antfu/unplugin-auto-import)
* [unplugin-vue-components](https://github.com/antfu/unplugin-vue-components)
* [vue](https://github.com/vuejs/core)
* [vite](https://github.com/vitejs/vite)
* [vitesse](https://github.com/antfu/vitesse)
* [vitesse-lite](https://github.com/antfu/vitesse-lite)
* [fluent-vue](https://github.com/demivan/fluent-vue)
* [vueuse](https://github.com/vueuse/vueuse)
* [milkdown](https://github.com/Saul-Mirone/milkdown)
* [gridjs-svelte](https://github.com/iamyuu/gridjs-svelte)
* [spring-easing](https://github.com/okikio/spring-easing)
* [bytemd](https://github.com/bytedance/bytemd)
* [faker](https://github.com/faker-js/faker)
* [million](https://github.com/aidenybai/million)
* [Vitamin](https://github.com/wtchnm/Vitamin)
* [neodrag](https://github.com/PuruVJ/neodrag)
* [svelte-multiselect](https://github.com/janosh/svelte-multiselect)
* [iconify](https://github.com/iconify/iconify)
* [tdesign-vue-next](https://github.com/Tencent/tdesign-vue-next)
* [cz-git](https://github.com/Zhengqbbb/cz-git)

***

## 릴리즈되지 않은밋 사용하기

새로운 릴리즈를 기다리지 않고 최신 기능을 사용하고 싶다면, [vitest repo](https://github.com/vitest-dev/vitest)를 복사하여 로컬 기기에 클론한 다음 직접 빌드하고 링크해야 합니다. (이 경우, [pnpm ](https://pnpm.io/)이 필요합니다.)

```bash
git clone https://github.com/vitest-dev/vitest.git
cd vitest
pnpm install
cd packages/vitest
pnpm run build
pnpm link --global # you can use your preferred package manager for this step
```

그런 다음 Vitest를 사용 중인 프로젝트로 이동하여 `pnpm link --global vitest`를 실행하세요   (또는 `vitest`를 전역적으로 링크하는 데 사용한 패키지 매니저로).

***

## 커뮤니티

궁금한 점이 있거나 도움이 필요할 경우, [Discord](https://chat.vitest.dev/)나 [GitHub Discussions](https://github.com/vitest-dev/vitest/discussions)에 문의해 주세요.



[^1]: [https://vueschool.io/lessons/how-to-install-vitest?friend=vueuse](https://vueschool.io/lessons/how-to-install-vitest?friend=vueuse)

