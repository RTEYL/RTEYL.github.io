---
layout: post
title:      "What Makes Next.js Great!"
date:       2021-03-21 21:13:33 -0400
permalink:  what_makes_next_js_great
---

  Next is a framework built for optimizing React’s client-side rendering SPA library into a server-side rending framework.

​	One of Next's primary optimization features, Automatic static optimization, makes static parts of webpages render immediately then updates dynamic content after, whereas other frameworks will only render static pages if the entire page contains static content and dynamic pages take a longer time to render. A quote from [Nex.js docs](https://nextjs.org/docs/advanced-features/automatic-static-optimization) says “If `getServerSideProps` or `getInitialProps` is present in a page, Next.js will switch to render the page on-demand, per-request. If the above is not the case, Next.js will statically optimize your page automatically by prerendering the page to static HTML.”

​	Next uses incremental static generation witch allows the application to generate new pages on-demand at runtime which also allows static regeneration . This mean that instead of a user viewing a blank screen wile loading a new page, Next will render the old page while building the new version behind the scenes. 

​	Next uses automatic code splitting in which code for each page is loaded on request to that page, only the necessary code for a specific page reducing load times. Additionally, Next offers further optimization by allowing dynamic imports to load optimally on the component level.

​	With React good SEO can be difficult to achieve because rendering is dynamic and crawlers have a hard time crawling through asynchronous content. Since Next offers prerendering and server-side rendering crawlers have a much easier time. If paired with other popular SEO packages it becomes much easier to optimize.

​	In conclusion, Next offers many optimizations out of the box and makes it easy to customize them. I recommend viewing their extensive list of [examples](https://github.com/vercel/next.js/tree/canary/examples) to help you work with many popular packages, libraries, and databases. It solves a lot of the reservations of React and with spectacular documentation and resources, has a low learning curve especially if you have a React background.



Resources:

[Next.js Examples - Docs - GitHub](https://github.com/vercel/next.js/tree/canary/examples)

[SEO - Michael Poirier-Ginter - Snipcart](https://snipcart.com/blog/react-seo-nextjs-tutorial)

[Nex.js - ASO - Docs](https://nextjs.org/docs/advanced-features/automatic-static-optimization)

[Next.js - Brandon Duffany - Medium](https://medium.com/swlh/the-hitchhikers-guide-to-next-js-fd7aa14ae8d0)

[Performance Metrics - Sean Connolly - Logrocket](https://blog.logrocket.com/next-js-vs-create-react-app/)

[Lighthouse - Daniel Phiri - Logrocket](
