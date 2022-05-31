# twitter-clone

![Laravel](https://www.unagino-nedoko.com/wp-content/uploads/2021/10/logo_Laravel.png)

Hajimari 内定者用の twitter-clone テンプレート

## 目的

この文書は Twitter Clone を作るにあたり、コーディングについて留意すべき事項をまとめたものである。

## 環境

![Docker](https://prtimes.jp/i/87890/2/resize/d87890-2-d4d26778877735a3722d-0.png)

### 環境構築に関しては、Docker にて行う。

Docker のインストールは、以下のリンクから行なってください。

→ [Docker Desktop for Mac](https://docs.docker.com/desktop/mac/install/)

## 命名規則

### 原則、クラス・メソッド・変数はキャメルケースで記述すること。

- ローマ字の使用を避け、英単語を使用すること。
- クラス名はアッパーキャメルケース（頭文字が大文字）で記述すること。
- 変数・メソッド名はローワーキャメルケース（頭文字が小文字）で記述すること。

キャメルケースの種類に関しては、以下を参考にしてください。

→ [アッパーキャメルケース](https://wa3.i-3-i.info/word13954.html)

#### Bad Case

```php
/*
 * これはスネークケース。
 * Twitter Cloneでは避けるように（他の案件では採用される場合もある）。
 */
public function follow_user()
{
    //処理
}
```

#### Good Case

```php
//こっちのキャメルケース推奨！
public function followUser()
{
    //処理
}
```

### 名前を読んで内容が分かるようにすること。

- クラスやメソッドがどんな機能なのかが一目で分かる命名を心がける。
- 変数名を読んで、何が格納されているのかが一目で分かる命名を心がける。

#### Bad Case

```php
//dataって何の？
$data = $user->id;
```

#### Good Case

```php
//この変数にはユーザーのIDが入ってる！
$userId = $user->id;
```

### 命名は、基本的に英語の文法に則って記述すること。

#### Bad Case

```php
//日本語の文法（目的語→動詞）
public function userDataGet()
{
    //処理
}

//get と edit と動詞が連続している。
public function getEditTweet()
{
    //処理
}
```

#### Good Case

```php
//英語の文法（動詞→目的語）
public function getUserData()
{
    //処理
}

/*
 * 動詞 edit を Editing にして名詞化する。
 * または他の命名が出来ないか再考してみる。
 */
public function getEdittingTweet()
{
    //処理
}
```

### その他、他の人が読みやすい命名を心がけること。

PHP のコーディングは、原則以下の記事を参考にしてください。

→ [PSR-2 コーディングガイド（日本語）](https://www.infiniteloop.co.jp/docs/psr/psr-2-coding-style-guide.php)

---

## GitHub

![GitHub](https://assets.st-note.com/production/uploads/images/24127642/rectangle_large_type_2_802007386bb75d9db15a6dd2880e2584.jpg)

**GitHub** というバージョン管理ツールを使用して、開発を行なってください。

### Git コマンドについて

Git の操作はターミナルで行います。

Git コマンドに関しては以下の記事を参考にしてください。

→ [Git コマンド早見表](https://qiita.com/kohga/items/dccf135b0af395f69144)

### ブランチについて

基本的に **Gitflow** を利用してください。

Gitflow とは、以下の図解のように

- main（本番環境）
- develop（テスト環境）
- feature, hotfix, release（開発環境）

3 種類のブランチを使い分ける方法です。

![Gitflow](https://image.itmedia.co.jp/ait/articles/1708/01/at-it-git-15-001.jpg)

Twitter Clone では、 **main, develop, feature** のブランチを利用してください。

また、Gitflow については以下の記事を参考にしてください。

→ [Gitflow ワークフロー](https://www.atlassian.com/ja/git/tutorials/comparing-workflows/gitflow-workflow)

### プルリクエストについて

GitHub に push したコードをレビューして貰い、問題がないようであれば merge します。

このコードをレビューしてもらうために出すのがプルリクエストです。

（プルリク、PR とも呼ばれます）

#### プルリクエストを出すタイミング

- 各章が完了したタイミング
- 新機能を追加したタイミング
- その他、修正が完了したなどキリの良いタイミング

一度に多くの変更があると、レビュワーに多くの負担がかかってしまいます。

それを避けるため、 **こまめにキリのいいタイミング** でプルリクエストを出しましょう。

## コーディングスタイル

### マジックナンバーは避ける。

ただの数字（マジックナンバー）を避け、定数として設定しましょう。

また、`Config`ディレクトリに`const.php`を作成して呼び出すという方法を使っても大丈夫です。

定数の場合、全て大文字で命名しましょう。

#### Bad Case

```php
if($follower == 0){
  echo 'フォロワーはいません。';
}
```

#### Good Case

```php
public const NONE = 0;

if($follower == NONE){
  echo 'フォロワーはいません。';
}
```

詳しくは以下を参考にしてください。

→ [マジックナンバーの概要解説とLaravelで対策する方法](https://yutaro-blog.net/2021/05/01/magic-number-laravel/)

### バリデーションは Form Request を使用すること。

Controllerではなく、Form Requestを利用したバリデーションを行なってください。

Form Requestを利用すると、リクエストを受けた瞬間にバリデートされる上、ソースコードが見やすくなります。

#### Bad Case（Requestでのバリデーション）
```php
public function tweet(Request $request)
{
    $data = $request->all();
    $validator = Validate::make($data[
        'text' => ['required', 'string', 'max:140']
    ]);
    $validator->validate();
}
```

#### Good Case（Form Requestでのバリデーション）
```php
//前略
public function rules()
{
    return [
        'text' => ['required', 'string', 'max:140']
    ];
}
```

詳しくは以下を参考にしてください。

→ [LaravelのFormRequestでバリデーションする！](https://codelikes.com/laravel-form-request/)

### セキュリティ対策

開発において、セキュリティ対策は最も重要な要素のひとつです。

|攻撃|内容|
|----|----|
|XSS|ユーザーがWebページにアクセスすることで不正なスクリプトが実行されてしまう脆弱性または攻撃手法|
|CSRF|『罠サイト』から『標的サイト』へ HTTP リクエストを送信することで『標的サイト』を操作してしまおうという攻撃手法|
|SQLインジェクション|悪意のあるユーザーが不正なクエリ( データベースへの命令文 )を書き、データベースへアクセスしてデータの漏洩・改ざんを行う攻撃|

対策方法やそれぞれの攻撃の概要については、以下の記事を参考にしてください。

→ [XSS,CSRF,SQLインジェクションまとめ](https://qiita.com/4649rixxxz/items/d31ecb33fcba1bffcb90)

### 認可

他人の投稿などを編集出来ないようにしましょう。

認可を設定するにはいくつか方法があります。

- Gate
- Policy
- 以下のようなコードを書く。

```php
public function editTweet(Tweet $tweet, Request $request)
{
    //投稿者と編集者が違った場合、403エラー。
    if($request->user()isNot($tweet->user)){
        abort(403);
    }
}
```

GateとPolicyはLaravelの認可機能です。

→ [Gate、Policyを完全理解](https://reffect.co.jp/laravel/laravel-gate-policy-understand)

どれを使っても大丈夫ですが、認可は必ず設定しておきましょう。
