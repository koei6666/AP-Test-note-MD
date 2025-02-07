
#AI 
### 過適合/過学習
　**過学習**(オーバフィッティング)とは、機械学習においてコンピュータに訓練データを学習させすぎてしまった結果、訓練データ範囲の傾向に過剰に適合してしまうことです。モデルが過学習に陥ると、未知のデータに対する予測精度が低くなってしまいます（汎用性がなくなる）。
　![[Pasted image 20231202022723.png]]

- #機械学習 の学習仕方
　　- 教師ありの学習
　　　入力と正解がセットとなった訓練データを与え、未知なデータに対して正解を導き出せるようにトレーニングする。
　　　用途:過去のデータから未来を予測する回帰(線型回帰)、与えてられたデータの分類と判別をする(ロジスティック曲線)
　　- 教師なしの学習
　　　膨大な入力データを与えて、コンピュータ自身にデータのパターンと特徴を発見させる。
　　　用途:似たような性質を持つデータをクラスタリングや、データの意味を残しながら、より少ない次元の情報に圧縮する次元削減(データの圧縮、可視化など)
　　- #強化学習
　　　試行錯誤を通じて、どの行動が価値を最大化にできるを学習する。この学習方法には:環境、エージェント(学習者)、方法の三つの要素が存在する。三要素の関連性は:ある環境において、エージェントはどういう行動を取れば、価値を最大化にできると学習する。
　　　環境に使用される最も基本的なモデルは #マルコフ決定過程　(確率モデル)である。
　　　用途:将棋や碁などのソフトウェアや、株の売買など。
　　　　>[!マルコフ決定過程]
　　　　　>マルコフ決定過程とは、マルコフ過程をもとに、各状態で報酬を与えられるようにした確率モデル。 ^e6a380

　#ディープラーニング
　　機械学習をさらに発展させたものはディープラーニングと言い、ディープラーニングでは人間の脳の神経回路を模したニューラルネットワークが用いられる。 ^ac87da

### **ROC曲線**
与えられたデータを「はい・いいえ」「陽性・陰性」などの2つのクラスに分類する機械学習モデルにおける判定結果は、AIが予測したクラスと実際のクラス分類の関係から、真陽性、偽陽性、真陰性、偽陰性の4つに分けることができます。
![[Pasted image 20231127203546.png]]
この4つの値を使って2値分類モデルの精度を示す指標として、正解率、再現率(真陽性率)、適合率、特異度、偽陽性率などがあります。

正解率

実際のクラスと同じクラスを予測した割合：$\frac{TP＋TN}{TP＋FN＋FP＋TN}$

適合率

陽性と予測したものうち実際に陽性だった割合：$\frac{TP}{TP＋FP}$

真陽性率（再現率、感度）

実際に陽性のものを正しく陽性と予測した割合：$\frac{TP}{TP＋FN}$

特異度

実際に陰性のものを正しく陰性と予測した割合：$\frac{TN}{FP＋TN}$

偽陽性率

実際に陰性のものを誤って陽性と予測した割合：$\frac{FP}{FP＋TN}$

**ROC曲線**は、縦軸に_真陽性率_、横軸に_偽陽性率_にとったグラフ上で、陽性・陰性の判断基準となるしきい値を動かしながら、各点での真陽性率と偽陽性率を打点していくことで描かれる曲線です。真陽性率と偽陽性率はトレードオフの関係にありますが、高い真陽性率を維持しつつ、偽陽性率をどれだけ抑えたモデルとなっているかを可視化することができます。
![[Pasted image 20231127203805.png]]

### ディープラーニング
システムに学習をさせるときに行列演算を含む大量の並列演算が必要なので、同じ種類の計算を大量に行うことに特化したGPUが活用されています。
[[並列処理]]

