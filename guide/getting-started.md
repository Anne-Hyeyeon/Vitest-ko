---
description: 시작하기
---

# Getting Started

## Overview

Vitest는 Vite에 의해 만들어진 차세대 테스팅 프레임워크입니다.

Vitest 프로젝트의 배경과 동기에 대한 더 자세한 정보는 'Why Vitest'에서 확인할 수 있습니다.





## 온라인에서 Vitest 실행해 보기

StackBlitz를 사용하여 Vitest를 온라인으로 실행해 볼 수 있습니다. StackBlitz를 통해 브라우저에서 바로 Vitest를 실행할 수 있으며, 설치 없이 로컬 환경과 유사하게 설정을 진행할 수 있습니다.



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

위 방법들 중 하나를 선택하여 프로젝트의 `package.json`에 `Vitest`의 복사본을 설치할 수 있습니다. 클론하는 대신 Vitest를 직접 실행하고 싶다면, `npx vitest`를 사용할 수 있습니다. (npx 명령어는 npm과 Node.js에 포함되어 있습니다.)



\
위 방법들 중 하나를 선택하여 프로젝트의 package.json에 Vitest의 복사본을 설치할 수 있습니다. 클론 대신 Vitest를 직접 실행하고 싶다면, npx vitest를 사용할 수 있습니다. (npx 명령어는 npm과 Node.js에 포함되어 있습니다.)

`npx` 명령어는 로컬의 `node_modules/.bin`에서 명령을 실행하거나, 해당 명령어를 실행하기 위해 필요한 패키지를 설치합니다. 기본적으로, npx는 $PATH나 로컬 프로젝트의 바이너리에서 해당 명령어를 찾아 실행합니다. 만약 해당 명령어가 없으면, 실행하기 전에 해당 명령어를 설치합니다.\


## 테스트 코드를 작성해 보기

예시와 같이, 우리는 두 개의 숫자를 더하는 함수의 결과를 검증하는 간단한 테스트 코드를 작성할 예정입니다.

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

테스트의 경우 파일 이름에 ".test" 또는 ".spec"이 들어가야 합니다.
{% endhint %}



이어서, 테스트를 실행하기 위해 `package.json`에 다음과 같은 섹션을 추가합니다.\


```json
{
  "scripts": {
    "test": "vitest"
  }
}
```



마지막으로, `npm run test` 명령어를 입력하여 실행합니다. (또는 `yarn test` 또는 `pnpm test`) Vitest는 아래와 같은 메시지를 출력할 것입니다.

```
✓ sum.test.js (1)
  ✓ adds 1 + 2 to equal 3

Test Files  1 passed (1)
    Tests  1 passed (1)
  Start at  02:15:44
  Duration  311ms (transform 23ms, setup 0ms, collect 16ms, tests 2ms, environment 0ms, prepare 106ms)

```

Vitest의 사용 방법에 대해 더 알아보고 싶다면, API 문서를 참조할 수 있습니다.





## Vitest 설정하기

\
Vitest의 주요 장점 중 하나는 Vite와의 통합 설정이 가능하다는 점입니다. 만약 프로젝트 루트에 `vite.config.ts` 파일이 있다면, Vitest는 이 파일을 읽어 Vite 앱의 플러그인과 설정을 Vitest와 통합합니다. 예를 들어, Vite의 `resolve.alias`와 `plugins` 같은 설정이 Vitest에도 즉시 적용됩니다.

Vite와 Vitest에서 서로 다른 설정을 적용하고 싶다면, 다음 방법들을 사용할 수 있습니다:



1. `vitest.config.ts` 파일을 생성하세요. 이 파일은 `vite.config.ts`보다 높은 우선순위를 가지며, Vitest 전용 설정을 할 수 있습니다.
2. CLI에서 `--config` 옵션을 사용하세요. 예를 들어, `vitest --config ./path/to/vitest.config.ts`와 같이 사용할 수 있습니다.
3. `vite.config.ts` 내에서 조건부 설정을 적용하려면 `process.env.VITEST`를 확인하거나, `defineConfig`의 `mode` 속성을 사용하세요. (이 파일이 정의되지 않는 한, 기본적으로 `test` 모드로 설정됩니다.)



Vitest는 Vite에서 지원하는 다양한 구성 파일 확장자(.js, .mjs, .cjs, .ts, .cts, .mts)를 지원하지만, `.json` 확장자는 지원하지 않습니다.

Vite를 빌드 도구로 사용하지 않는 경우, `config file`에 있는 `test` 프로퍼티를 이용하여 Vitest를 구성할 수 있습니다.

```typescript
import { defineConfig } from 'vitest/config'

export default defineConfig({
  test: {
    // ...
  },
})
```

{% hint style="info" %}
Vitest는 자체 변환 파이프라인을 갖추고 있음에도 불구하고 Vite에 크게 의존합니다. 이는 Vite의 다양한 변환 및 모듈 해석 기능을 활용하기 때문입니다. 따라서, [Vite 문서](https://vitejs.dev/config/)에 기술된 속성들을 Vitest 설정에도 적용할 수 있습니다.
{% endhint %}



Vite를 이미 프로젝트에 사용 중인 경우, `vite.config.ts` (또는 `.js`) 파일에 `test` 프로퍼티를 추가하여 Vitest를 위한 설정을 구성할 수 있습니다.

또한, 당신의 Vite 설정 파일 상단에 특별한 주석인 triple slash directive를 추가하여 Vitest 타입에 대한 참조를 포함시킬 수 있습니다.&#x20;

```typescript
/// <reference types="vitest" />
import { defineConfig } from 'vite'

export default defineConfig({
  test: {
    // ...
  },
})
```

[Config Reference(설정 참조)](https://vitest.dev/config/) 에서 설정 옵션을 확인할 수 있습니다.

{% hint style="info" %}
*   주의사항\
    \
    Vite와 Vitest를 위해 별도의 설정 파일을 사용하는 경우, Vitest 설정 파일에 Vite 옵션을 명시적으로 정의해야 합니다. 이는 Vitest 설정이 Vite 설정을 확장하는 것이 아니라, 기존 설정을 덮어쓰기 때문입니다. Vite 설정과 Vitest 설정을 통합하고 싶다면, `vite` 또는 `vitest/config`에서 `mergeConfig` 메서드를 가져와 사용하여 두 설정을 병합할 수 있습니다.\


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

    그러나 별도의 파일을 만드는 것보다는, 가능한 한 Vite와 Vitest 모두에 대해 동일한 설정 파일을 사용하는 것을 권장합니다.
{% endhint %}

***

## 작업 공간  지원

[Vitest Workspaces](https://vitest.dev/guide/workspace.html)를 사용하면 하나의 프로젝트 내에서 여러 구성의 프로젝트를 실행할 수 있습니다. `vitest.workspace` 파일을 통해 작업 공간에 포함할 파일과 폴더 목록을 정의할 수 있으며, 이 파일은 `.js`, `.ts`, `.json` 확장자를 지원합니다. 이 기능은 특히 모노레포 설정에 유용하게 사용됩니다.

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

Vitest가 설치된 프로젝트에서는, `npm scripts`를 통해 `vitest`를 사용하거나, `npx vitest`를 이용하여 직접 Vitest를 실행할 수 있습니다. Vitest 프로젝트를 생성했을 때 기본적으로 제공되는 npm 스크립트는 다음과 같습니다:

```json
{
  "scripts": {
    "test": "vitest",
    "coverage": "vitest run --coverage"
  }
}
```

파일 변경을 감지하지 않고 테스트를 한 번만 실행하려면, `vitest run`을 사용하면 됩니다. 추가적으로 `--port`나 `--https`와 같은 추가 CLI 옵션을 지정할 수 있습니다. 사용 가능한 CLI 옵션의 전체 목록을 보려면, 프로젝트에서 `vitest --help`를 실행하세요.

[Command Line Interface(CLI) ](https://vitest.dev/guide/cli.html)에 대해 더 알아보려면, 해당링크를 방문하세요.

***

## IDE 통합&#x20;

Vitest는 사용자의 테스트 경험을 향상시키기 위해 Visual Studio Code(VS Code)의 공식 확장 프로그램으로도 제공됩니다.

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

## 새로운 기능 바로 사용하기

새로운 기능을 바로 사용하고 싶으시다면, Vitest 저장소를 복제하여 로컬 컴퓨터에 클론한 후, 직접 빌드하고 링크하는 과정을 거쳐야 합니다. 이 과정에는 pnpm이 필요합니다.

```bash
git clone https://github.com/vitest-dev/vitest.git
cd vitest
pnpm install
cd packages/vitest
pnpm run build
pnpm link --global # you can use your preferred package manager for this step
```

그 다음으로, Vitest를 사용 중인 프로젝트로 이동한 후, 다음 명령어를 실행하여 전역적으로 링크된 Vitest를 해당 프로젝트에 연결하세요:`pnpm link --global vitest`( 또는 전역적으로 링크하는 데 사용한 다른 패키지 매니저를 사용하세요).

\


***

## 커뮤니티

궁금한 점이 있거나 도움이 필요할 경우, [Discord](https://chat.vitest.dev/)나 [GitHub Discussions](https://github.com/vitest-dev/vitest/discussions)에 문의해 주세요.



[^1]: [https://vueschool.io/lessons/how-to-install-vitest?friend=vueuse](https://vueschool.io/lessons/how-to-install-vitest?friend=vueuse)

