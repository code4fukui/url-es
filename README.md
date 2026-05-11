# url-es

> 日本語のREADMEはこちらです: [README.ja.md](README.ja.md)

[
![deno-test](https://github.com/code4fukui/url-es/actions/workflows/test.yml/badge.svg)
](https://github.com/code4fukui/url-es/actions/workflows/test.yml)

An ES module for URL resolution and parsing, providing feature parity with the Node.js core [`url` module](http://nodejs.org/api/url.html). Designed for Deno and modern JavaScript environments.

## Usage

Import the module directly from its URL:

```js
import * as url from "https://code4fukui.github.io/url-es/url.js";

// Resolve a relative URL
console.log(url.resolve('/one/two/three', 'four'));
//> '/one/two/four'

// Parse a URL string into an object
const myUrl = url.parse('http://user:pass@host.com:8080/p/a/t/h?query=string#hash');
console.log(myUrl.hostname);
//> 'host.com'

// Format a URL object back into a string
console.log(url.format(myUrl));
//> 'http://user:pass@host.com:8080/p/a/t/h?query=string#hash'
```

## API

### Parsed URL Object

The `url.parse()` method returns a URL object. Its properties depend on the components present in the URL string.

Example for the URL `'http://user:pass@host.com:8080/p/a/t/h?query=string#hash'`:

*   `href`: The full URL that was originally parsed. The protocol and host are lowercased.
    *   Example: `'http://user:pass@host.com:8080/p/a/t/h?query=string#hash'`

*   `protocol`: The request protocol, lowercased.
    *   Example: `'http:'`

*   `host`: The full lowercased host portion of the URL, including port.
    *   Example: `'host.com:8080'`

*   `auth`: The authentication information portion of a URL.
    *   Example: `'user:pass'`

*   `hostname`: The lowercased hostname portion of the host, without the port.
    *   Example: `'host.com'`

*   `port`: The port number portion of the host.
    *   Example: `'8080'`

*   `pathname`: The path section of the URL, which comes after the host and before the query.
    *   Example: `'/p/a/t/h'`

*   `search`: The 'query string' portion of the URL, including the leading question mark.
    *   Example: `'?query=string'`

*   `path`: Concatenation of `pathname` and `search`.
    *   Example: `'/p/a/t/h?query=string'`

*   `query`: The 'params' portion of the query string. If `parseQueryString` is `true`, it will be an object.
    *   Example: `'query=string'` or `{'query':'string'}`

*   `hash`: The 'fragment' portion of the URL, including the leading pound sign.
    *   Example: `'#hash'`

### `url.parse(urlStr, [parseQueryString], [slashesDenoteHost])`

Takes a URL string and returns a URL object.

*   `urlStr` `<string>` The URL string to parse.
*   `parseQueryString` `<boolean>` If `true`, the `query` property will be an object parsed by the `querystring` module. **Default:** `false`.
*   `slashesDenoteHost` `<boolean>` If `true`, treat `//foo/bar` as `{ host: 'foo', pathname: '/bar' }` rather than `{ pathname: '//foo/bar' }`. **Default:** `false`.

### `url.format(urlObj)`

Takes a parsed URL object and returns a formatted URL string. The formatting logic follows these rules:

*   `href` is ignored.
*   `protocol` is postfixed with `://` for `http`, `https`, `ftp`, `gopher`, and `file`. All other protocols are postfixed with `:`.
*   `auth` will be used if present.
*   `host` will be used in place of `hostname` and `port`.
*   `hostname` and `port` are only used if `host` is absent.
*   `search` will be used in place of `query`.
*   `query` (if it's an object) will only be used if `search` is absent.

### `url.resolve(from, to)`

Resolves a target URL (`to`) relative to a base URL (`from`) in the same way a browser resolves an anchor tag's `href`.

```js
url.resolve('/one/two/three', 'four');         // '/one/two/four'
url.resolve('http://example.com/', '/one');    // 'http://example.com/one'
url.resolve('http://example.com/one', '/two'); // 'http://example.com/two'
```

## Attribution

This module is a port of the Node.js core `url` module. Copyright Joyent, Inc. and other Node contributors.

## License

[MIT](https://github.com/code4fukui/url-es/blob/main/LICENSE)