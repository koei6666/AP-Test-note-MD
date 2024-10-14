### \<ul> 
順序なしリストを表す要素です。この要素を使うとWebページ上に行頭記号を伴うリストが描画されます。順序なしリストを記述するときに ul要素と一緒に用いられる要素が li です。ul要素がリスト全体を表すのに対して、li要素は個々のリスト項目を示します。  

```html
//HTML文書  
<ul>  
<li>りんご</li>  
<li>みかん</li>  
<li>バナナ</li>  
</ul>  
  
//Webページの表示

- りんご
- みかん
- バナナ
```

### **DHTML**(Dynamic HTML)
静的なHTMLの内容をCSS(スタイルシート)とJavaScript等のクライアントサイドスクリプト言語を用いて動的に変更するウェブ技術を指す抽象概念である。

### Refererヘッダ
HTTPリクエストの一部として送信されるヘッダーの1つです。以下にRefererヘッダーの主要な特徴と用途を説明します：

1. 定義: Refererヘッダーは、現在のリクエストを発生させた元のWebページのURLを示します。
2. 機能:

- ユーザーがどこからそのページに来たかを示します。
- リンク元のトラッキングに使用されます。

3. 形式:

`Referer: https://example.com/page.html`

4. 主な用途:

- アクセス解析：サイトの訪問者がどこから来たかを追跡します。
- コンテンツ保護：特定のページからのアクセスのみを許可するなど。
- ナビゲーション：「戻る」機能の実装に利用されることがあります。

5. セキュリティ上の考慮事項:

- Refererヘッダーは機密情報を含む可能性があるため、セキュリティ上のリスクとなる場合があります。
- HTTPS→HTTPの遷移では、Refererヘッダーが送信されないことがあります。

6. プライバシーの問題:

- ユーザーの閲覧履歴の一部が露出する可能性があります。
- 一部のブラウザやプラグインでは、Refererヘッダーの送信を制限または無効化できます。

7. Referrer-Policyヘッダー: サーバー側でRefererヘッダーの送信をより細かく制御するためのヘッダーです。例えば：

`Referrer-Policy: no-referrer`

8. JavaScriptでの利用: `document.referrer`プロパティを使用して、クライアント側でRefererを取得できます。

Refererヘッダーは有用なツールですが、セキュリティとプライバシーの観点から慎重に扱う必要があります。適切に使用すれば、ウェブサイトのナビゲーションの改善やセキュリティの強化に役立ちます。

### Contentパラメータ
Html headのmeta tagにあるcontentパラメータは、該当のページがsearch engineとcrawlersに対応する時の動きを定義している。

主なオプションは以下の6種類がある：

#### 1. `nofollow`
This directive instructs search engines not to follow links on the page. It can be added as follows:

```html
<meta name="robots" content="nofollow">
```

#### 2. `noarchive`
This option prevents search engines from storing a cached copy of the page. Use it like this:

```html
<meta name="robots" content="noarchive">
```

#### 3. `nosnippet`
This directive tells search engines not to show snippets or previews of the page in search results, including text snippets and video previews:

```html
<meta name="robots" content="nosnippet">
```

#### 4. `noodp`
This option prevents search engines from using information from the Open Directory Project (ODP) for the page's description in search results:

```html
<meta name="robots" content="noodp">
```

#### 5. `notranslate`
This directive indicates that search engines should not offer translation options for the page:

```html
<meta name="robots" content="notranslate">
```

#### 6. Combination of Directives
You can combine multiple directives in a single meta tag. For example, to prevent indexing and following links, you can write:

```html
<meta name="robots" content="noindex, nofollow">
```

These directives provide granular control over how your content is treated by search engines, allowing you to manage visibility and indexing according to your needs.
