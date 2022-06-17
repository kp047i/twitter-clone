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

### マジックナンバーは避け、定数として設定する（Consts ファイルを作成して呼び出しても良い）。また、定数の場合は全て大文字で命名しても良い。

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

### バリデーションは Form Request を使用すること。

バリデーションはForm Requestを使用して行いましょう。

Form Requestを使用すると、Controllerに行く前にバリデーションを実施できます。

#### Bad Case
```php
public function store(Request $request, Tweet $tweet)
{
    $data = $request->all();
    $validator = Validator::make($data, [
        'text' => ['required', 'string', 'max:140']
    ]);
    $validator->validate();
}
```

#### Good Case
```php
public function authorize()
{
    return true;
}

public function rules()
{
    return [
        'text' => ['required', 'string', 'max:140'],
    ];
}
```

Form Requestに関しては、以下の記事を参考にしてください。

→ [FormRequestによるバリデーション](https://qiita.com/gone0021/items/249e99338ff414fc5737)

## メンターへの質問について

### ・自走時間の定量化（前提として自らできることはやった状態）
例)自走に1時間かけて分からなければ質問に切り替える。

### ・問題の細分化
細分化のプロセスが分からない人は以下の記事を参考にしてください。  
→ [問題の細分化](https://qiita.com/hirossyi73/items/7785cdd367c774da43df)

### ・質問の明確化
まず問題を細分化できたら、次に自分がわからない点だけを列挙し、それを以下のような質問フォーマットにまとめましょう。
>【質問フォーマット】</br>
■ 今、起きている事（必要に応じて、画面やエラー文などのキャプチャを付与)  
（例）ユーザー新規登録をしようと、登録ボタンを押したらエラーになった。  
■ 解決したい事  
（例）ユーザー新規登録を実装し、データベースに保存したい。  
■ 調べた事、考えた仮説  
（例）公式リファレンスやQiitaで似たような記事をみた。（記事のURLなども載せる）
deviseのgemがインストールされているかは確認済み。作成したキー名も間違っていないので、データベースが怪しいと思いテーブル名やカラム名を調べてみた。  
■ 仮説を元に行った結果   
（例）データベースにもテーブル、カラムもきちんと反映されており、スペルミスも見当たらなかった。再度、実行してみたが同じくエラーで解決できなかった。
