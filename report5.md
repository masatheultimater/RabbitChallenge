# ディープラーニング Day3(ラビットチャレンジ)

# Day3
# 本日のテーマ　＝ RNN　
## RNN
自然言語処理への応用に使用されてきたNN<br>
RNN　＝　時系列データに対応できるNNのこと<br>
ここでの時系列データとは、時系列軸で各データに相関関係があるデータ<br>
##### Section1) RNNの概念
- RNNの全体像
[![Image](https://gyazo.com/db46a8e7b3241a8535922d01c3aacc49/thumb/1000)](https://gyazo.com/db46a8e7b3241a8535922d01c3aacc49)<br>
#### ポイント
- 連鎖的に後のｙの値に影響を与えていく
- 重みはW,W(in),W(out)W, W_{(in)}, W_{(out)}W,W(in)​,W(out)​ となっている
- u ：前層総出力　z : 中間層活性化関数　v :出力層に与えられる出力　g : 出力層活性化関数
- 初期の状態と過去の時間t−1t-1t−1の状態を保持し、そこから次の時間でのtを再帰的に求める
[![Image](https://gyazo.com/efbdcbaf02aa7da2c4928ab7614304ce/thumb/1000)](https://gyazo.com/efbdcbaf02aa7da2c4928ab7614304ce)<br>
[![Image](https://gyazo.com/a3a3daef2df586315ac2a95613189dca/thumb/1000)](https://gyazo.com/a3a3daef2df586315ac2a95613189dca)<br>


### BPTT
 - RNNにおけるパラメータ最適化法　誤差逆伝播法の一種
 - 誤差をパラメータで微分
- 誤差の微分部の実装
[![Image](https://gyazo.com/50868d74e43c445bf633417119f1b56b/thumb/1000)](https://gyazo.com/50868d74e43c445bf633417119f1b56b)<br>
[![Image](https://gyazo.com/84d6b2a13723cb610135b894d8045b2e/thumb/1000)](https://gyazo.com/84d6b2a13723cb610135b894d8045b2e)<br>
- 活性化関数の実装
[![Image](https://gyazo.com/54df91fdd65dcb18d02d986df3c75ae8/thumb/1000)](https://gyazo.com/54df91fdd65dcb18d02d986df3c75ae8)<br>
[![Image](https://gyazo.com/694a0acc29b931fad75964e9292620a6/thumb/1000)](https://gyazo.com/694a0acc29b931fad75964e9292620a6)<br>
- BPTTでの微分部実装
[![Image](https://gyazo.com/d7ce65a4bd10c089f91592b70a694d95/thumb/1000)](https://gyazo.com/d7ce65a4bd10c089f91592b70a694d95)<br>
- 上記微分で求めたデルタを利用したパラメータ更新式
- 勾配降下法で最適化
[![Image](https://gyazo.com/7bf70b9d76c3bef5ab70acf7a7db9732/thumb/1000)](https://gyazo.com/7bf70b9d76c3bef5ab70acf7a7db9732)<br>
再帰的に誤差を決定する<br>
[![Image](https://gyazo.com/701c2438dd2a9a6683e3a2e89eccd9dc/thumb/1000)](https://gyazo.com/701c2438dd2a9a6683e3a2e89eccd9dc)<br>


##### Section2) LSTM
#### ポイント
 - LSTMでは、勾配消失問題を解消するため、長い時系列での学習が可能となる
 - BPTTとは異なる、構造からの勾配消失問題へのアプローチ
 - 誤差逆伝播法によるパラメータ最適化では長い時系列の学習が困難(時系列をさかのぼるほど勾配が消失する)
 - 勾配消失問題：誤差逆伝播法が下位層に進むにつれ、下位層では更新が行われなくなり学習しなくなる
 - 勾配爆発　⇔　勾配消失
[![Image](https://gyazo.com/539d13f3d738c6c6131ee143e2202651/thumb/1000)](https://gyazo.com/539d13f3d738c6c6131ee143e2202651)<br>
 - CEC
  - 役割：過去の出力情報を保持　渡す勾配は１とし、勾配消失・爆発を防ぐ
  - 課題：入力データに時間依存度を影響させないためNNの学習特性がなくなる
  - →入力ゲート・出力ゲートで課題を解決する
 - 入力ゲートと出力ゲート
  - 役割：ゲートを追加することでそれぞれのゲートへの入力値の重みを重み行列として可変化する
  - →順伝播では勾配あり、逆伝播では勾配一律
 - 忘却ゲート
  - 役割：データはすべて保管されているため、忘却ゲートを利用して情報を破棄する
 - 覗き穴結合
  - CECに保持する値を入力ゲート・忘却ゲート・出力ゲートに反映させ、ゲート制御にCECが関与していなかった課題を解決する
##### Section3) GRU
 - LSTMの課題を解決するNN構造
 - LSTMの課題：パラメータが煩雑　計算負荷が大きい　→　GRU(NN構造)で解決
 - GRUの特徴：
  - パラメータを大幅削除(計算負荷↓)
  - 精度は同等またはそれ以上だが上位互換ではない
[![Image](https://gyazo.com/4ac914128ddc1a8a19f3a731b732072e/thumb/1000)](https://gyazo.com/4ac914128ddc1a8a19f3a731b732072e)<br>

##### Section4) 双方向RNN
 - 過去と未来の情報共に加味することで精度を向上するモデル
 - 文章推敲・機械翻訳において使用されることがある

## RNN での自然言語処理
##### Section5) Seq2Seq
- Encoder‐Decoderモデルの一種
[![Image](https://gyazo.com/60fa6578964b26066efd4cd159b240fc/thumb/1000)](https://gyazo.com/60fa6578964b26066efd4cd159b240fc)<br>

 - Encoder RNN
  - 構造：入力データをトークンに区切って渡す構造
  - 特徴
   - 文章をトークンに分割し、トークンIDを付与
   - IDから、そのトークンを表す分散表現ベクトルに変換
   - ベクトルを順番にRNNへ入力していく
    - RNN　ベクトル入力　→　hidden state出力　→　hidden state + 前のベクトルを入力　→　hidden state出力　→　・・・
    - 最後のhidden state をfinal stateとして取っておく。thought vectorと呼ばれ、入力した分の意味を表すベクトルとして表現される　→　decoderに渡される
 - Decoder RNN
  - 受け取ったデータを、単語等のトークン毎に文書を構築する構造
  - 特徴
   - RNN
    - thought vectorから各トークンの生成確率を出力し、RNNのinitial stateとして設定、Embeddingを入力
   - Sampling
    - 生成確率に基づいてtokenをランダムに選ぶ
   - Embedding
    - tokenをEmbeddingしてRNNの次の入力とする
   - Detokenize
    - 上記を繰り返し、tokenを文字列として直す
 - HRED
  - Seq2Seqの課題を解決する
  - Seq2Seqの課題：一問一答でしか対応できない(答えが一つしかない問のみ)
  - 特徴
   - 前の文章の文脈を捉えて答えを出力する　→　人間らしい
   - 過去t-1個の発話から次の発話を生成する
   - 構造：Seq2Seq + Context RNN
    - Context RNN: 各文章の時系列をまとめて、これまでの文脈全体をベクトルとして変換する構造
  - 課題
   - 確率的な多様性は字面にのみあり、会話の流れの多様性はない
    - →同じコンテキストを与えられても毎回同じ出力しかできない
 - VHRED
  - HREDの課題を解決する
  - 特徴
   - HREDにVAEの潜在変数の概念を追加
 - VAE
  - オートエンコーダー
   - 教師なし学習の一種
   - 学習入力データは、訓練データのみ、で教師データはない
   - 特徴
    - Encoder　入力データ　→　潜在変数ｚ
    - Decoder　ｚ　→　元画像の復元
    - メリット：次元削除が行える
  - VAE
   - オートエンコーダの課題：潜在変数ｚにどのように圧縮されているかわからない
   - 特徴
    - 潜在変数ｚに確率分布を仮定する
    - ある確率分布に即した次元削除を行うことができる
##### Section6) Word2vec
 - RNNの課題を解決する
 - RNNの課題
  - 固定長形式でしか単語をNNに与えることができない
  - ボキャブラリ*ボキャブラリだけの重み行列を生成しており、大規模データに未対応
 - 特徴
  - 大規模データの分散表現の学習が現実的にできるようになった
  - ボキャブラリ*任意の単語ベクトル次元で重み行列を生成
  - 分散表現として情報量が少なくなったものに、任意の重みをかけられるようになったことがポイント
##### Section4) AttentionMechanism
 - Seq2Seqの課題を解決する
 - Seq2Seqの課題
  - 長文の対応が難しい
  - 2単語でも、100単語でも固定次元ベクトルに入力しなくてはならない
 - 特徴
  - 文章が長くなるほどその内部表現の次元も大きくなっていく
  - スケーラブルなベクトル生成を実現する
  - 翻訳前後の単語毎の関連度に注目するため対応が可能
　
