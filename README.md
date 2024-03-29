# アプリ概要
## アプリのURL
https://www.bokunoboki.com/

## 画像
-  トップページ
[![Image from Gyazo](https://i.gyazo.com/c70d37fe811a964321514115b5969dcc.png)](https://gyazo.com/c70d37fe811a964321514115b5969dcc)

- ログイン
[![Image from Gyazo](https://i.gyazo.com/1e8b5d74243aea2602707a558f0380d7.png)](https://gyazo.com/1e8b5d74243aea2602707a558f0380d7)

- ダッシュボード
[![Image from Gyazo](https://i.gyazo.com/d849e76d3675554ae720d6c404928808.png)](https://gyazo.com/d849e76d3675554ae720d6c404928808)

- チャプターの一覧
[![Image from Gyazo](https://i.gyazo.com/f76da80d6ee5b9b639fa0d9ce69b5b1a.png)](https://gyazo.com/f76da80d6ee5b9b639fa0d9ce69b5b1a)

- アウトプットの画面
[![Image from Gyazo](https://i.gyazo.com/80dcc0290b84b6ef307285fb6e860c5b.png)](https://gyazo.com/80dcc0290b84b6ef307285fb6e860c5b)

- 他人のアウトプットのコメント
[![Image from Gyazo](https://i.gyazo.com/f3cb16f26874a21002445adc4d784c7d.png)](https://gyazo.com/f3cb16f26874a21002445adc4d784c7d)

- 管理者画面
[![Image from Gyazo](https://i.gyazo.com/79837716c8b1269c3dd28537042680ca.png)](https://gyazo.com/79837716c8b1269c3dd28537042680ca)
[![Image from Gyazo](https://i.gyazo.com/7fe66253f0c8b93d645022c7d2f75d2e.png)](https://gyazo.com/7fe66253f0c8b93d645022c7d2f75d2e)

##  僕のボキとは
私は現在簿記を勉強しています。  
基本的にはテキストを読んで問題演習をするといったことの繰り返しなのですが、「だれかに学習のサポートをしてほしいな」と感じることがありました。  
そんな経験から取得した後は、簿記学習のサポートをココナラのようなスキル販売サイトで行いたいと考え、今回その時に使うアプリを想定して開発しました。

## 基本的な使い方
参考書の各チャプターを読んでもらい、学んだことをこのアプリにアウトプットしてもらいます。  
参考書には[パブロフ流でみんな合格 日商簿記](https://www.amazon.co.jp/%E7%B0%BF%E8%A8%98%E6%95%99%E7%A7%91%E6%9B%B8-%E3%83%91%E3%83%96%E3%83%AD%E3%83%95%E6%B5%81%E3%81%A7%E3%81%BF%E3%82%93%E3%81%AA%E5%90%88%E6%A0%BC-%E6%97%A5%E5%95%86%E7%B0%BF%E8%A8%982%E7%B4%9A-%E5%95%86%E6%A5%AD%E7%B0%BF%E8%A8%98-2021%E5%B9%B4%E5%BA%A6%E7%89%88/dp/4798168475/?_encoding=UTF8&pd_rd_w=VPQT4&pf_rd_p=0bb824f3-323d-45b0-8a2a-126f1b7fc966&pf_rd_r=1JW3NRQYQV8EH42F11XV&pd_rd_r=4af0a16b-2d83-4cd8-9bc5-de497d3b003b&pd_rd_wg=rhmD9&ref_=pd_gw_ci_mcx_mr_hp_atf_m)
シリーズ（よせだあつこさん著）を使用します。（理由は自分が使用していたため）  
 
それぞれのアウトプットに対してコメントを通してディスカッションすることができ、その都度修正してもらい、管理者（私）が問題ないと判断したら合格点をもらえます。  

（3級は教材が私の手元にないため、現在は2級工業簿記・商業簿記のみ対応としています。）

## プランについて
予定では
- ベーシック: 1週間のお試しコース
- プラス: 1ヶ月980円コース
- プロ:　合格するまでの永久対応の14,980円コース

を考えています。

当初はアプリ自体にStripeやPay.jpなどの外部決済サービスの導入を考えていましたが、（ココナラのような）スキル販売サイト上で取引できること、顧客情報流出のリスクがあることを考慮し、導入しませんでした。

# 技術面
## 言語など
- フロントエンド: Nuxt.js(Vue.js) 2.15.8
- バックエンド: Ruby on Rails 6.1.4.6 APIモード  
- デプロイ先: Heroku
- 画像ストレージはAmazon S3を使用
- 独自ドメインは お名前.com にて取得

## ER図
[![Image from Gyazo](https://i.gyazo.com/30ca8ddf1107143959c09c2ecfb8051f.png)](https://gyazo.com/30ca8ddf1107143959c09c2ecfb8051f)

### 各テーブル、カラムについて
- user  
  ユーザー情報についてのテーブル。  
  devise-token-authという認証系のGemで追加
  - name: 名前
  - email: メールアドレス
  - password, password_confirmation:　パスワード
  - is_admin: 管理者かどうかの情報。デフォルトはfalse
  - level_id: ユーザーがどのレベルの内容に取り組んでいるのかをわかるようにする外部キー
  - plan: 契約しているプランを示す。basic, plus, proのどれか。

- level  
学習コンテンツのレベルを示したテーブル。  
具体的には、3級簿記、2級工業簿記、2級商業簿記のどれか。
  - name: コンテンツの名前

- chapter  
levelの中の、チャプターについてのテーブル  
levelが2級工業簿記の場合は、「材料費」「労務費」「経費」などが当てはまる。
  - name: チャプターの名前
  - level_id: どのレベルのチャプターなのかを示す外部キー

- lesson  
  chapterの中の、レッスンについてのテーブル  
  levelが2級工業簿記、chapterが材料費の場合、「直接材料費と間接材料費」「材料の消費単価」「棚卸減耗費」などが当てはまる。
  - name: レッスンの名前
  - level_id: どのレベルか示す
  - chapter_id: どのチャプターか示す 

- output  
  それぞれのlessonに対してのアウトプットを保存するテーブル
  - post: アウトプットの本文。html形式で保存
  - user_id: 投稿したユーザーを示す
  - lesson_id: どのレッスンに対してのアウトプットかを示す
  - be_finished: デフォルトはfalse。合格したら管理者がtrueにする

- comment  
  アウトプットに対してのコメントを保存するテーブル  
  - body: コメントの内容
  - user_id: コメントを投稿したユーザーを示す
  - output_id: どのoutputか
