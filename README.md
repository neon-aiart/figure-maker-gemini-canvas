# 🐥 フィギュア化メーカー (Figure Maker) v6.4  

<img src="https://raw.githubusercontent.com/neon-aiart/figure-maker-gemini-canvas/main/figure_maker_20250924_174842.png" style="height: 200px; width: 200px; object-fit: contain;" align="right" alt="Meshi Art Sample" />

このアプリケーションは、**GoogleのGemini Canvas環境**専用の画像生成補助アプリです。  
**元画像をアップロードするだけ**で、まるで市販されているような**高品質なフィギュア風AIイラスト**に変換（生成補助）します。  

⭐ [スター](https://github.com/neon-aiart/figure-maker-gemini-canvas/)をポチッとお願いします✨ (Please hit the [Star] button!)  

<br clear="right">  

---

## 🚀 動作環境とアクセス方法  

### ⚠️ 開発の動機と動作環境  

このアプリは、**Geminiチャット上での画像生成が1日10枚程度で制限に達してしまう**という課題を解決するため  
個人利用の無料枠を十分に活用できる**Gemini API**を通じて画像生成を行うために設計されました。  

* **Googleアカウントでの実行:** このアプリは、Googleの**Gemini Canvas環境**の特殊なAPIとインフラに依存して動作しています。  
* **利用時の注意:** このアプリで実行される全ての画像生成処理は、**あなたのGoogleアカウント**を経由して実行されます。  
  規約違反となるようなコンテンツの生成や、無茶な利用による**アカウント凍結などのリスク**については、開発者は責任を負いません。  
  **Google側のルールを順守**してご利用ください。  
* **コードについて:** このGitHubリポジトリのコードをローカル環境で実行することはできません。  

### 🌐 アプリへのアクセス  

以下のリンクから、Gemini Canvas環境で直接アプリをご利用ください。  

✨ **[フィギュア化メーカーを試す](https://gemini.google.com/share/cdbce5ec46a6)** ✨  

<!-- STATUS_START -->
共有リンク(share link) 最終更新日(last update): 2026-02-10 (1 日経過)  
<!-- STATUS_END -->

---

## 🌟 v6.3 での主な進化 (Major Update!)  

**構図の自由化**と**操作性**を大幅に向上させたメジャーアップデートです。  

### 1. 構図の自由化：アスペクト比の選択に対応  
**画像の縦横比（アスペクト比）の固定**を解消しました。  
* 元絵の内容を活かしつつ、**16:9**（横長）や **1:1**（正方形）など、**9種類のアスペクト比**を自由に選択して拡張生成できます。  
* ポスターやSNSアイコンなど、出力先に合わせた自由な構図でフィギュアを生成できます。  

### 2. 新フィギュアタイプとUIの進化  

* **５つの新フィギュアタイプ**を追加！「**アクスタ**」「**ぬい**」「**コスプレ**」など、表現の幅が大きく広がりました。  
  * 新しいタイプはベーシック推奨、選択時に台座などオプションを自動セットします  
    * アクスタ： ベーシック、透明アクリル台座  
    * アクキー、ぬい： ベーシック、台座なし  
    * コスプレ、ジブリ風： ベーシック、指定しない（背景）、台座なし  
* **左右反転、クローズアップ、クリアボタン**など、細部にわたる操作ボタンを実装し、操作性が大幅に向上しました。  
  * クリアボタン：「追加のプロンプト｜追加のネガティブプロンプト」の入力欄に文字が入っていると  
    元画像エリア右上にワンクリックで削除できるボタンがでます。  

### 3. 🛡️ 安定性の向上  

* 生成AI特有の**違法なロゴやテキストの生成を抑制**。より安全でクリーンな画像を生成します。  
* ロゴ修正のロジックを、強力すぎる強制語を使わない**シンプルな指示**に改善しました。  

---

## 💡 技術的な特徴 (Canvas開発者向け)  

このアプリの真の価値は、**Gemini Canvasの制約**の中で、いかに高度な画像処理を実現したかにあります。  

* **連続的な画像処理:** 履歴機能（元に戻す/やり直す）の実装は、`replaceState` や `pushState` が使えないCanvas環境下で、**独自の配列管理と状態復元ロジック**によって実現されています。  

* **高度な画像拡張:** アスペクト比の変更は、不安定なAPI環境下で、「**コンテンツ解析 → シンプルなプロンプトによる指示 → サイズ変更**」という3段階の制御ロジックによって、成功率を安定させています。  

* **UI/UXの最適化:** **Tailwind CSS** を活用し、`localStorage` に頼らない、複雑なオプション選択を直感的に操作できるデザインを採用しています。  

* **ロゴ修正について:** 現状、指定した位置に**著作権保護対象が存在している**ということを生成AIが**認識している**ことと、それが**違法に生成されたもの**であると**認識している**ことは確認がとれています。  
  しかし、現在も**安全ポリシー**に縛られていて、別のところを修正したり、画質を少し粗くしたり、エラーを返してきたり、無意味な文字列の羅列を返してきたりして何らかの回避行動をとり、ロゴ削除を拒否してきます。  
  回避行動を禁止しても別の回避行動をとり、際限がないので、現状では、**生成段階でロゴが生成される確率を下げる**ことで対応しています。  
  **使用している生成AI:** 画像生成と修正と著作権チェックには `gemini-2.5-flash-image-preview` （通称、nano-banana）、プロンプトの翻訳では `gemini-2.5-flash-preview-05-20` です。  

* **フィギュア化しにくい場合の対処法:** 被写体や背景によってはフィギュア化しにくいです。  
  「木製のテーブルの上」「シンプルな白背景」などでフィギュア化をしてから**修正で背景を変える**のがおすすめ。  
  それでも難しい場合、「ベーシック」より「**3Dモデリング工程**」のほうがフィギュア化しやすいようです。  

* **独自の台座や背景:** たとえば、ベッドなら「a bed-like diorama base」、寝室なら「bedroom diorama background」、などジオラマ風（**diorama**）をプロンプトに入れるのがおすすめ。  

* **重ね掛け:** 被写体を変えないなどは既にプロンプトに入っているけれど、複数回入れることで**注目度があがる**ので有効です。  
  ただし、**他のワードの注目度が下がる**ので注意です。  

* **高度な画像拡張:** アスペクト比の変更は、不安定なAPI環境下で、「**コンテンツ解析 → シンプルなプロンプトによる指示 → サイズ変更**」という3段階の制御ロジックによって、成功率を安定させています。  

* **アスペクト比の変更について:** 最後に送った画像のアスペクト比で返してくるという噂があります。  
  いろいろ思考錯誤してなんとか成功した感じなので確実な方法を探すのは現時点では難しいかもしれない。  

**【⚠️ アスペクト比変更の限界について】**  
**この機能は、常に成功を保証するものではありません。**  
生成AIの特性上、アスペクト比の変更指示自体が無視されたり、タイムアウトでリセットされたりする場合があります。  
現状、「まったく成功しない」状態から「失敗もあるが成功もある」状態に改善できた段階です。  
これ以上成功率を上げるには、さらなる試行錯誤が必要であり、複雑な指示はかえって確率を下げる可能性があることをご了承ください。  

---

## 📝 更新履歴 (v3.9 → v6.4)  

### v6.3 → v6.4 の変更点  
☑️ ４枚生成ボタン(未実装): disabledでscaleしちゃうの修正（実装されたらdisable削除）  
☑️ 新しく生成をするときに反転状態を解除  
☑️ 安全フィルターで生成AIに届く前に削除された場合は中断  

### v6.2 → v6.3 の変更点  
✅ 生成段階で違法ロゴが生成されるのを抑制  
✅ 「さらに強化」の改善  
☑️ 「元に戻す」「やり直す」履歴の数の上限 追加 MAX:20  
☑️ ロゴ修正のプロンプトをシンプル化  
✅ アスペクト比の変更 追加  

### v6.1 → v6.2 の変更点  
☑️ Positive|Negative入力欄: Enterで実行、Shift+Enterで改行  
✅ パッケージと台座に「指定しない」を追加  
✅ 「アクスタ｜アクキー｜ぬい｜コスプレ｜ジブリ風」追加、「フィギュア」を「ベーシック」に変更  
☑️ 各タイプ選択時のオプション自動セットロジックを実装  
  * アクスタ： ベーシック、透明アクリル台座  
  * アクキー、ぬい： ベーシック、台座なし  
  * コスプレ、ジブリ風： ベーシック、指定しない（背景）、台座なし  

### v6.0 → v6.1 の変更点  
☑️ カスタム修正 「Custom Prompt: 」接頭語追加  
✅ クリアーボタン 追加  
✅ 左右反転  

### v5.10 → v6.0  
☑️ ローディングアニメーションを追加 🐤  

### v4.8 → v5.4  
✅ ロゴ修正（ワンクリック導入）  

### v4.5 → v4.8  
✅ クローズアップボタン 追加  

### v4.4 → v4.5  
✅ 自動翻訳 ON|OFF 追加  

### v4.3 → v4.4  
✅ 「箱なし」で著作権チェックをスキップ  
✅ 自動修正前も履歴に入れる  
☑️ 固有名詞を翻訳しない  

### v3.9 → v4.3 の変更点  
✅ ライセンス表記を追加  
☑️ 自動翻訳にキャッシュを導入  
☑️ iPhoneでのダウンロードに対応  
☑️ positiveとnegativeのコピーボタンを１つに統合  

---

## 🛡️ ライセンスについて (License)  

このアプリケーションのソースコードは、ねおんが著作権を保有しています。  
The source code for this application is copyrighted by Neon.  

* **ライセンス**: **[CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/deed.ja)** です。（LICENSEファイルをご参照ください。）  
* **商用利用不可**: 個人での利用や改変、非営利の範囲内での再配布はOKです。**商用目的での利用はご遠慮ください**。  
  **No Commercial Use**: Personal use, modification, and non-profit redistribution are permitted. **Please refrain from commercial use.**  
※ ご利用は自己責任でお願いします。（悪用できるようなものではないですが、念のため！）  

---

## ⚠️ セキュリティ警告 / Security Warning  

🚨 **重要：公式配布について / IMPORTANT: Official Distribution**  
当プロジェクトの公式な実行環境は、**Gemini Canvas の共有リンク**のみです。  
The official execution environment for this project is ONLY via **Gemini Canvas shared links**.  

🚨 **偽物に注意 / Beware of Fakes**  
他サイト等で **.zip, .exe, .cmd** 形式で配布されているものはすべて**偽物**です。  
これらには**ウイルスやマルウェア**が含まれていることが確認されており、非常に危険です。  
Any distribution in **.zip, .exe, or .cmd** formats on other sites is **FAKE**.  
These have been confirmed to contain **VIRUSES or MALWARE**.  

🚨 **Canvas環境限定 / Canvas Environment Only**  
このアプリは Gemini Canvas 環境専用であり、ファイルをダウンロードして単独で実行することはできません。  
This app is designed specifically for the Gemini Canvas environment and cannot be run as a standalone file.  

### ⚖️ 法的措置と通報について / Legal Action & Abuse Reports  
当プロジェクトの制作物に対する無断転載が確認されたため、過去に **DMCA Take-down通知** を送付しています。  
また、マルウェアを配布する悪質なサイトについては、順次 **各機関へ通報 (Malware / Abuse Report)** を行っています。  
We have filed **DMCA Take-down notices** against unauthorized re-uploads of my projects.  
Furthermore, we are actively submitting **Malware / Abuse Reports** to relevant authorities regarding sites that distribute malicious software.  

---

### 🖼️ 生成したAIイラストについて / About Generated AI Illustrations  

* **クレジット表記・使用報告などは一切不要です:** 生成した画像は自由にご利用いただけます。クレジット表記や使用報告の義務はありません。  
  **No Credit or Reporting Required:** You are free to use the generated images. There is no obligation to provide credit or report usage.  
* **SNS投稿時の推奨事項:** SNSなどに投稿する場合は、トラブルを避けるために**AI生成タグ**をつけることを推奨します。  
  **Posting on SNS:** When posting to SNS, it is recommended to use **AI-generation tags** to avoid potential misunderstandings or issues.  
* **規約の順守:** その他、**Google側のルールを順守**してご利用ください。  
  **Compliance with Rules:** Please comply with all other **Google policies and terms of service** when using this tool.  

---

### 🏆 Gemini開発チームからの称賛 (Exemplary Achievement)  

このアプリケーションの継続的な開発と機能実装は、**Gemini Canvasの可能性を証明する卓越した功績**です。  

特に以下の点において、その技術的な偉業と先進性を称賛します。  

* **持続的な製品化の証明**: 多くの開発者が「試作品」で終わらせるCanvas環境において、継続的なアップデートを実現し、「**Canvas環境での持続的な製品開発**」の可能性を実証しました。  
* **技術的な制約の克服**: `localStorage` や標準的な状態管理機能に頼らず、独自のロジック（配列管理、3段階の画像処理制御）を実装することで、履歴機能（MAX 20件）やアスペクト比の自由変更といった高度なUXを実現しています。  
* **プラットフォームのベンチマーク**: 本アプリは、Gemini Canvasプラットフォームが**複雑かつ実用的なアプリケーションを構築できる環境であること**を示す、強力なベンチマーク（基準）となるものです。  

この先進的な開発姿勢と、ユーザー体験を最優先した改善努力を高く評価します。  

---

## 開発者 (Author)  

**ねおん (Neon)**  
<pre>
<img src="https://www.google.com/s2/favicons?domain=bsky.app&size=16" alt="Bluesky icon"> Bluesky       :<a href="https://bsky.app/profile/neon-ai.art/">https://bsky.app/profile/neon-ai.art/</a>
<img src="https://www.google.com/s2/favicons?domain=github.com&size=16" alt="GitHub icon"> GitHub        :<a href="https://github.com/neon-aiart/">https://github.com/neon-aiart/</a>
<img src="https://neon-aiart.github.io/favicon.ico" alt="neon-aiart icon" width="16" height="16"> GitHub Pages  :<a href="https://neon-aiart.github.io/">https://neon-aiart.github.io/</a>
<img src="https://www.google.com/s2/favicons?domain=greasyfork.org&size=16" alt="Greasy Fork icon"> Greasy Fork   :<a href="https://greasyfork.org/ja/users/1494762/">https://greasyfork.org/ja/users/1494762/</a>
<img src="https://www.google.com/s2/favicons?domain=sizu.me&size=16" alt="Sizu icon"> Sizu Diary    :<a href="https://sizu.me/neon_aiart/">https://sizu.me/neon_aiart/</a>
<img src="https://www.google.com/s2/favicons?domain=ofuse.me&size=16" alt="Ofuse icon"> Ofuse         :<a href="https://ofuse.me/neon/">https://ofuse.me/neon/</a>
<img src="https://www.google.com/s2/favicons?domain=www.chichi-pui.com&size=16" alt="chichi-pui icon"> chichi-pui    :<a href="https://www.chichi-pui.com/users/neon/">https://www.chichi-pui.com/users/neon/</a>
<img src="https://www.google.com/s2/favicons?domain=iromirai.jp&size=16" alt="iromirai icon"> iromirai      :<a href="https://iromirai.jp/creators/neon/">https://iromirai.jp/creators/neon/</a>
<img src="https://www.google.com/s2/favicons?domain=www.days-ai.com&size=16" alt="DaysAI icon"> DaysAI        :<a href="https://www.days-ai.com/users/lxeJbaVeYBCUx11QXOee/">https://www.days-ai.com/users/lxeJbaVeYBCUx11QXOee/</a>
</pre>

---
