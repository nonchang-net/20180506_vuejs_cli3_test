# vue.js test

## 概要

- 20180506日曜日メモ
	- 個人用のvue.js + TypeScript導入テスト環境
	- 発端と初期目標
		- webGLゲームをTypeScriptだけで作ったらDOM生成やCSS適用で死んだ。
		- UIなどhtml/css部分はreactやvueの導入が必要だと感じた。
		- 今回は以前経験のあるvue.jsの導入と復習、これまで触ってなかったvuexやrouterの考え方をまとめていく。
		- ついでに自己流のユニットテストをやめて、Jestによるユニットテスト方法をまとめていく。


## 中期目標

- 20180506日曜日メモ
	- vue+TypeScriptでミニゲームの開発に着手
	- router把握、画面遷移を適用する
	- Jestユニットテストを実装する
	- 過去の自作JavaScriptゲームやmap editorツールなどの移植を考えていく



### 20180506日曜日 vue.js+TypeScript学習メモ

: 導入メモは分ける

- vue.jsをTSで始めるにあたり、まずvue-cli3が出力したデコレータ（@Component/@Prop）がわからないので調べる。
	- [TypeScriptではじめるVueコンポーネント（vue-class-component）](https://qiita.com/koya_kon/items/8d9968d07748d20825f8)
		- 2017年12月24日。
		- `vue-class-component`
		- `vue-property-decorator`
		- この辺りが対応するもののようだ。package.jsonをみても、今回のvue-cli3にはちょうどこの二つをセットアップされたことがわかる。（他はservice-workerとrouter、vuex。）

	- 20180506: 把握が遅れたエラーのメモ:
		- `37:11 Property 'value' does not exist on type 'HelloWorld'.`
		- このvalueはBar.tsのインスタンスなのだけど、Bar.ts中のexport default classの名前を修正し忘れて出ていた。うーん、ファイル名に一致させるassertが欲しい＞＜


	- vue-cli3のテンプレートは、自前でやったTypeScript直のwebpack環境よりも環境整備が整っていて、エラーメッセージやコンソールの行数がちゃんとブラウザのdev tool上の表示とマッチしてくれる。
		- これの解決方法は見つけられなかったな……。
		- トランスパイラを除いても、vue.js相当の環境を自前で作るのは難易度が高い（というか、情報蒐集がキツい）状況であることを把握できたと思う。うーん。


- `TODO`
	- 20180506: 直近で開いてたページ退避
		- [[図解]Vue.js2.x系で親子コンポーネント間でデータの受け渡しをする方法](https://kuroeveryday.blogspot.jp/2016/10/vuejs-components-emit-events.html)
		- [TypeScriptではじめるVueコンポーネント（vue-class-component）](https://qiita.com/koya_kon/items/8d9968d07748d20825f8)
		- [Vue インスタンス](https://jp.vuejs.org/v2/guide/instance.html)
		- [TypeScript のサポート](https://jp.vuejs.org/v2/guide/typescript.html)
		- [クラスとスタイルのバインディング](https://jp.vuejs.org/v2/guide/class-and-style.html)



### 20180506日曜日 vue+TypeScript導入メモ

: 毎度ながらフロントエンド開発環境は細かいトラブルが続く。試しながら状況を随時メモ。
: 後日アップデートした時は新規にゼロベースで手順を書き直す予定。

- vue-cli 3.0(beta9)の導入から調査、手順をメモ。

	- 前提メモ: xcode command line tools/node.js(+「n」によるバージョン管理)/npm/homebrewなどの導入は揃っている想定。

	- 今回セットアップした時点のバージョンのメモ
		- macOS: Sierra 10.12.6（16G1314）
		- npm: 6.0.0
		- node: n 8.11.1で現時点のLTS 8.11.1(carbon)を入れて利用。
			- 注意: 10.xでは`Node Sass could not find a binding for your current environment: OS X 64-bit with Node.js 10.x`というエラーが出てnpm run build/serveが動作せず。深追いしていない。
			- 後日、エラーが解消したら最新版への追従を考えたい。
		- TypeScript(tsc --version): Version 2.8.3
		- 今回入れたvue-cli: 3.0.0-beta.9

	- まずローカルのnodeがおかしいので調査（いきなり別件トラブル……）
		> mini1 $ node --version
		> dyld: Library not loaded: /usr/local/opt/readline/lib/libreadline.6.dylib
		> Referenced from: /usr/local/bin/rlwrap
		- rlwrapがおかしいのではなく、macOSの何処かのタイミングでreadline6が削除されたという話。
			https://qiita.com/kanpou0108/items/0fe67e313c03e87277e7
			- 上記URLの説明そのままに、以下のコマンドを実行。
			`ln -s /usr/local/opt/readline/lib/libreadline.dylib /usr/local/opt/readline/lib/libreadline.6.dylib`
			- これで修正できた。
			- readline.6を要求するアプリにreadline.7を参照させてることになるけど問題ない模様。

	- [Using the New vue-cli 3 to Scaffold Vue.js Apps](https://alligator.io/vuejs/using-new-vue-cli-3/)
		- まずは`npm install -g @vue/cli`
			- `3.0.0-beta.9`がセットアップされた。
		- `vue create vuetest`
			- 把握してないE2E Testing以外を有効に。
			- Unit testing solutionはmocha+chaiではなくJestを選んでみた。
				- [Facebook 製 JavaScript テストツール Jest を使ってテストする ( Babel, TypeScript のサンプル付き )](https://tech.recruit-mp.co.jp/front-end/post-14752/)
			- これでnpm installが始まるみたいだ。しばし放置で任せておこう。
			- セットアップできたようだ。main.tsとかApp.vueが揃ってる。
		- `npm run serve`
			- さて、VSCodeで見るとsyntax errorが出てるな……。
			- .tsから.vueのimportが全部エラーになってる……。

	- [vue-cli 3.0 でTypeScriptのプロジェクトを作ったらVeturがうまく動かなかった](https://qiita.com/h-reader/items/5cada1d7c25b886b26b8)
		- あー、プロジェクト調べてみたらこのエラーも出てるな。
		- これから直そう。yarnがいるのか。

		- [インストール](https://yarnpkg.com/lang/ja/docs/install/#mac-stable)
			> nvm もしくは同様のものを使用している場合は、nvm のバージョンの Node.js を使用するように Node.js を除外してください。
			`brew install yarn --without-node`
			- 拡張機能フォルダはどこだ……。パッケージでもApplication Support/codeでもなさそう？
			- `/Applications/Visual Studio Code.app/Contents/Resources/app/bin/`にextension見つけたけど……該当するファイルは見つからない。うーん。
		
		- https://github.com/vuejs/vetur/issues/682
			- https://github.com/vuejs/vetur/releases/tag/0.11.8
			- 上で案内されていた0.11.8の.vsixを拾った。
			- [Visual Studio Code の拡張機能を VSIX ファイルからインストールする](https://mseeeen.msen.jp/how-to-install-extension-in-visual-studio-code-with-vsix/)

		- よし、治った。混乱したなぁ＞＜;

	- 続いては、.vueのimportがエラーになってる件を直したい……。

		- ぐぐり続けたけどわからない！ stack overflowにも同様の情報はなさげ？
		- これは今回betaのvue-cliを使ったことと、VSCode拡張であるveturの問題なのかを絞り込めず、情報に出会えない感じに陥ってる気がする。。

		- ちょっと考え方を妥協して、「main.ts」「router.ts」でしか発症しないので無視する、という方向性で考えてみたい。
			- まとめ
				- .tsから.vueをimportすると参照エラーになっている。
				- 実アプリでは、以下の想定。
					- `.tsはインハウスなライブラリ用途であり、.vueを参照しない形に閉じる方向で考えるのでこの問題は発症しないはず。`
				- とはいえ目障りなので、今後のバージョンアップや情報蒐集で改善することを期待。

	- npm run buildでエラーが出たので確認したところ、npm run serveも失敗するようになってた？
		- Node Sass could not find a binding for your current environment: OS X 64-bit with Node.js 10.x
		- node.jsを10.x(n latest)にあげたことでエラーになった模様。
		- https://nodejs.org/ja/download/releases/
			- 10.xは出たばかりすぎるのかな……。開発時はLTSにしておくか。。
			- 現在のLTSは8.11.1のようなので、`n 8.11.1`でこのバージョンに切り替える。
			- 解決。

	- テンプレートをあまり触ってない状態でnpm run buildした結果、app.jsとvender.jsの合計は137kb。今時のフロントエンドフレームワークとしては妥当な範囲だろう。
		- どうせ動作物は500kb超えるだろう、という所感。もちろん、リソースが入ってくればローディング処理が必要になる。

`EOF`