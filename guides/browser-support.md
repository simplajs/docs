# Browser support

Simpla supports all modern browsers.

| Browser | Supported Version | Method |
| --- | --- | --- |
| Google Chrome | Current | Native |
| Safari | Current | Native |
| Opera | Current | Native |
| Internet Explorer | 10+ | Polyfill |
| Microsoft Edge | Current | Polyfill |
| Firefox | Current | Polyfill |
| Safari Mobile | Current | Native |
| Chrome Mobile | Current | Native |

> **Note:** While Simpla supports IE10+, it expects to be in an environment with `Promise` support, and doesn’t bundle its own polyfill. Ensure you’re including a compatible polyfill if you support IE

Simpla relies on an emerging family of web standards called [Web Components](https://webcomponents.org). Support for Web Components is being actively developed by all major vendors. Google Chrome and its derivatives (Opera, Chrome mobile) and Safari (macOS & iOS) currently have full support, while Firefox and Microsoft Edge have partial support and require small polyfills.

See the support grid at the bottom of [webcomponents.org](https://www.webcomponents.org/) for more information on what is supported natively and what is being polyfilled across browsers.

In practice this means Simpla enjoys native-level performance in Chrome and Safari, and performance similar to other leading Javascript libraries in other browsers.