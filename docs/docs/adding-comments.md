---
title: コメントの追加
---

Gatsby でブログを動かしていて、いくつかのコンテンツを追加した場合、次に考えるのは訪問者のエンゲージメントを高めることです。それを実現する素晴らしい方法は、あなたの記事に対して訪問者が質問したり、意見したりできるようにすることです。これにより、あなたのブログは訪問者にとってより活気のあるものになります。

コメントを追加する機能にはたくさんのオプションがありますが、その中のいくつかは特に静的サイトを対象にしています。このリストは網羅的ではありませんが、何が利用可能なのかを説明する出発点として役に立ちます。

- [Disqus](https://disqus.com)
- [Commento](https://commento.io)
- [Facebook Comments](https://www.npmjs.com/package/react-facebook)
- [Staticman](https://staticman.net)
- [JustComments](https://just-comments.com) \([Gatsby の公式プラグイン](https://www.gatsbyjs.org/packages/gatsby-plugin-just-comments/)\)
- [TalkYard](https://www.talkyard.io)
- [Gitalk](https://gitalk.github.io)

Tania Rascia が Gatsby ブログで書いたように[独自のコメントシステムを展開する](/blog/2019-08-27-roll-your-own-comment-system/)こともできます。

## コメントに Disqus を使用する

このガイドでは、ブログに Disqus を実装する方法を学びます。Disqus には多くの優れた機能があるためです。

- It is low maintenance, meaning [moderating your comments and maintaining your forum](https://help.disqus.com/moderation/moderating-101) less hassle.
- 公式の[React サポート](https://github.com/disqus/disqus-react)を提供します。
- [寛大な無料枠](https://disqus.com/pricing)を提供します。
- [最も広く使用されているサービス](https://www.datanyze.com/market-share/comment-systems/disqus-market-share)のようです。
- コメントが簡単です。 Disqus には大規模なユーザー基盤があり、新規ユーザーに慣れさせるのが非常に早いです。Google、Facebook、Twitter のアカウントを登録することができ、これらのチャンネルを介して書いたコメントをシームレスに連携できます。
- Disqus のユーザーインターフェースは多くのユーザーが認識できる落ち着いた外観をしています。
- すべての Disqus コンポーネントは遅延読み込みされるので、投稿の読み込みに時間に悪影響を与えません。

Bear in mind, however, that choosing Disqus also incurs a tradeoff. Your site is no longer entirely static but depends on an external platform to deliver your comments through embedded `iframe`s on the fly. Moreover, you should consider the privacy implications of letting a third party store your visitors' comments and potentially track their browsing behavior. You may consult the [Disqus privacy policy](https://help.disqus.com/terms-and-policies/disqus-privacy-policy), the [privacy FAQs](https://help.disqus.com/terms-and-policies/privacy-faq) (specifically the last question on GDPR compliance) and inform your users [how to edit their data sharing settings](https://help.disqus.com/terms-and-policies/how-to-edit-your-data-sharing-settings).

If these concerns outweigh the benefits of Disqus, you may want to look into some of the other options above. We welcome Pull Requests to expand this guide with setup instructions for other services.

## Implementing Disqus

![Disqus logo](./images/disqus-logo.svg)

Here are the steps for adding Disqus comments to your own blog:

1. [Sign-up to Disqus](https://disqus.com/profile/signup). During the process you'll have to choose a shortname for your site. This is how Disqus will identify comments coming from your site. Copy that for later.
2. Install the Disqus React package

```shell
npm install disqus-react
```

3. Add the shortname from step 1 as something like `GATSBY_DISQUS_NAME` to your `.env` and `.env.example` files so that people forking your repo will know that they need to supply this value to get comments to work. (You need to prefix the environment variable with `GATSBY_` in order to [make it available to client-side code](https://www.gatsbyjs.org/docs/environment-variables/#client-side-javascript).)

```text:title=.env.example
# enables Disqus comments for blog posts
GATSBY_DISQUS_NAME=insertValue
```

```text:title=.env
GATSBY_DISQUS_NAME=yourSiteShortname
```

4. In your blog post template (usually `src/templates/post.js`) import the `DiscussionEmbed` component.

```js:title=src/templates/post.js
import React from "react"
import { graphql } from "gatsby"
// highlight-next-line
import { DiscussionEmbed } from "disqus-react"
```

Then define your Disqus configuration object

```js
const disqusConfig = {
  shortname: process.env.GATSBY_DISQUS_NAME,
  config: { identifier: slug, title },
}
```

Where `identifier` must be a string or number that uniquely identifies the post. It could be the post's スラッグ、 title or some ID. Finally, add `DiscussionEmbed` to the JSX of your post template.

```jsx:title=src/templates/post.js
return (
  <Global>
    <PageBody>
      {/* highlight-next-line */}
      <DiscussionEmbed {...disqusConfig} />
    </PageBody>
  </Global>
)
```

And you're done. You should now see the Disqus comment form appear beneath your blog post [looking like this](https://janosh.io/blog/disqus-comments#disqus_thread). Happy blogging!

[![Disqus comments](./images/disqus-comments.png)](https://janosh.io/blog/disqus-comments#disqus_thread)
