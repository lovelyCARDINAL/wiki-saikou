<div align="center">

# Wiki Saikou

SUPER COOL api package for MediaWiki

**- 同时兼容浏览器&Node.js 环境 -**<br>
**- Support both browser and Node.js environment -**

本库提供了与原版 `new mw.Api()` 非常相似的 api 请求封装。让你在非 MediaWiki 环境中轻松实现各种 wiki 操作。使用 TypeScript 编写~<br>
The library provides the out of box accessing to MediaWiki API in both browsers & Node.js, and the syntax is very similar to vanilla `new mw.Api()`. TypeScript definition included~

</div>

## 特色功能 Features

- Similar API to the vanilla `new mw.Api()`
- Parameter Schema automatic compliance: `{ "foo": ["bar", "baz"], watch: false }` → `{ "foo": "bar|baz" }`
- Token caching and retry mechanism
- TypeScript support
- With unit tests
- User authentication supports out of the box \*(also applicable to Node.js!)

## 开箱即用/Out of box

**安装/installation**

```sh
# Via pnpm:
pnpm add wiki-saikou
# Yarn? sure:
yarn add wiki-saikou
# Or just npm:
npm install wiki-saikou
```

Then, import it to your project:

```ts
import { MediaWikiApi } from 'wiki-saikou'
const api = new MediaWikiApi('https://zh.moegirl.org.cn/api.php')
// ...
```

**在浏览器中直接使用/Use directly in the browser**

```js
import('https://unpkg.com/wiki-saikou').then(() => {
  const { MediaWikiApi } = globalThis.WikiSaikou
  const api = new MediaWikiApi('https://zh.moegirl.org.cn/api.php')
  // ...
})
```

Then use it just like the `new mw.Api()`

## 使用方法/Usage

You can find some sample code snippets [here](test/).

Below is the documentation of MediaWikiApi.

---

### `MediaWikiApi` {class MediaWikiApi}

**Main methods**:

#### `new MediaWikiApi(baseURL?: string, options?: AxiosRequestConfig)`

- `baseURL`: API endpoint of your target wiki site (e.g. https://mediawiki.org/w/api.php)
  - **Not required but with conditions**: If you are using it in the browser environment, and the website runs MediaWiki. The instance will automatically use the API endpoint of current wiki.
- `options`: {AxiosRequestConfig}

#### `login(username: string, password: string): Promise<{ result: 'Success' | 'Failed'; lguserid: number; lgusername: string }>`

Login your account.

#### `get<T = any>(params: MwApiParams, options?: AxiosRequestConfig): Promise<AxiosResponse<T>>`

Make `GET` request

#### `post<T = any>(body: MwApiParams, options?: AxiosRequestConfig): Promise<AxiosResponse<T>>`

Make `POST` request

#### `postWithToken<T = any>(tokenType: MwTokenName, body: MwApiParams, options?: AxiosRequestConfig): Promise<AxiosResponse<T>>`

Make `POST` request with specified token.

```ts
type MwTokenName =
  | 'createaccount'
  | 'csrf'
  | 'login'
  | 'patrol'
  | 'rollback'
  | 'userrights'
  | 'watch'
```

### Auxiliary utilities

#### `get ajax` {AxiosInstance}

Get `AxiosInstance` of current MediaWikiApi instance

#### `MediaWikiApi.adjustParamValue(params: MwApiParams): Record<string: string>` (static)

Adjust input params to standard MediaWiki request params.

- `string[] → string`: `['foo', 'bar', 'baz'] → 'foo|bar|baz`
- `false → undefined`: remove false items

#### `MediaWikiApi.createAxiosInstance(payload: { baseURL: string; params: MwApiParams; options: AxiosRequestConfig })` (static)

Create your own axios instance.

**Warning: The instance created by this method does not include responsive getters/setters (described below) and the out of box cookie controls.**

#### `get/set defaultOptions` {AxiosRequestOptions} (responsive\* getter/setter)

defaults: `{}`

#### `get/set defaultParams` {MwApiParams} (responsive\* getter/setter)

defaults:

```ts
this.defaultParams = {
  action: 'query',
  errorformat: 'plaintext',
  format: 'json',
  formatversion: 2,
}
```

### \*About the responsive getter/setter

Modifying these properties on the instance will automatically recreate the Axios instance of current MediaWikiApi instance. You can modify them directly and safely.

---

> MIT License
>
> Copyright (c) 2022 萌娘百科 User:机智的小鱼君 (A.K.A. Dragon-Fish)
