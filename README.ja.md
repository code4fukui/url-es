# url-es

[
![deno-test](https://github.com/code4fukui/url-es/actions/workflows/test.yml/badge.svg)
](https://github.com/code4fukui/url-es/actions/workflows/test.yml)

URLの解決と解析を行うためのESモジュールであり、Node.jsのコアである[`url`モジュール](http://nodejs.org/api/url.html)と同等の機能を提供します。DenoおよびモダンなJavaScript環境向けに設計されています。

## 使い方

URLから直接モジュールをインポートします:

```js
import * as url from "https://code4fukui.github.io/url-es/url.js";

// 相対URLを解決
console.log(url.resolve('/one/two/three', 'four'));
//> '/one/two/four'

// URL文字列をオブジェクトに解析
const myUrl = url.parse('http://user:pass@host.com:8080/p/a/t/h?query=string#hash');
console.log(myUrl.hostname);
//> 'host.com'

// URLオブジェクトを文字列にフォーマット
console.log(url.format(myUrl));
//> 'http://user:pass@host.com:8080/p/a/t/h?query=string#hash'
```

## API

### 解析済みURLオブジェクト

`url.parse()`メソッドはURLオブジェクトを返します。そのプロパティはURL文字列に含まれるコンポーネントに依存します。

URL `'http://user:pass@host.com:8080/p/a/t/h?query=string#hash'` の例:

*   `href`: 最初に解析された完全なURLです。プロトコルとホストは小文字になります。
    *   例: `'http://user:pass@host.com:8080/p/a/t/h?query=string#hash'`

*   `protocol`: リクエストのプロトコルです。小文字になります。
    *   例: `'http:'`

*   `host`: URLのホスト部分全体（ポートを含む）です。小文字になります。
    *   例: `'host.com:8080'`

*   `auth`: URLの認証情報の部分です。
    *   例: `'user:pass'`

*   `hostname`: ホストのホスト名部分（ポートを含まない）です。小文字になります。
    *   例: `'host.com'`

*   `port`: ホストのポート番号の部分です。
    *   例: `'8080'`

*   `pathname`: URLのパス部分です。ホストの後、クエリの前に位置します。
    *   例: `'/p/a/t/h'`

*   `search`: URLの「クエリ文字列」部分です。先頭の疑問符（?）を含みます。
    *   例: `'?query=string'`

*   `path`: `pathname`と`search`を連結したものです。
    *   例: `'/p/a/t/h?query=string'`

*   `query`: クエリ文字列の「パラメータ」部分です。`parseQueryString`が`true`の場合、オブジェクトになります。
    *   例: `'query=string'` または `{'query':'string'}`

*   `hash`: URLの「フラグメント」部分です。先頭のハッシュ記号（#）を含みます。
    *   例: `'#hash'`

### `url.parse(urlStr, [parseQueryString], [slashesDenoteHost])`

URL文字列を受け取り、URLオブジェクトを返します。

*   `urlStr` `<string>` 解析するURL文字列。
*   `parseQueryString` `<boolean>` `true`の場合、`query`プロパティは`querystring`モジュールによって解析されたオブジェクトになります。**デフォルト:** `false`。
*   `slashesDenoteHost` `<boolean>` `true`の場合、`//foo/bar`を`{ pathname: '//foo/bar' }`ではなく`{ host: 'foo', pathname: '/bar' }`として扱います。**デフォルト:** `false`。

### `url.format(urlObj)`

解析済みのURLオブジェクトを受け取り、フォーマットされたURL文字列を返します。フォーマットのロジックは以下のルールに従います:

*   `href`は無視されます。
*   `protocol`は、`http`、`https`、`ftp`、`gopher`、`file`の場合、末尾に`://`が付加されます。それ以外のすべてのプロトコルは、末尾に`:`が付加されます。
*   `auth`が存在する場合は使用されます。
*   `host`は、`hostname`と`port`の代わりに使用されます。
*   `hostname`と`port`は、`host`が存在しない場合にのみ使用されます。
*   `search`は、`query`の代わりに使用されます。
*   `query`（オブジェクトの場合）は、`search`が存在しない場合にのみ使用されます。

### `url.resolve(from, to)`

ブラウザがアンカータグの`href`を解決するのと同じ方法で、ベースURL（`from`）に対するターゲットURL（`to`）を解決します。

```js
url.resolve('/one/two/three', 'four');         // '/one/two/four'
url.resolve('http://example.com/', '/one');    // 'http://example.com/one'
url.resolve('http://example.com/one', '/two'); // 'http://example.com/two'
```

## 帰属

このモジュールはNode.jsのコアである`url`モジュールの移植版です。Copyright Joyent, Inc. and other Node contributors.

## ライセンス

[MIT](https://github.com/code4fukui/url-es/blob/main/LICENSE)
