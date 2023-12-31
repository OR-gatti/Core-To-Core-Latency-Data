# CPUのコア間レイテンシのベンチマーク結果
## はじめに
　コア間レイテンシとはコアとコアの通信の遅延のことです。特殊なソフトウェアを使うことで可視化が可能です。それぞれのCPUの設計によって挙動がぜんぜん違うので面白いですよ。コア数が多くなればなるほど挙動に差がはっきり出ます。今回はMicroBenchXというツールを使いました。私が測定したデータには限りがありますが、それぞれなぜそのような挙動になるのかを簡単に説明します。

## Ryzen3 2200G
　ツールを使うことでレイテンシがヒートマップとして現れます。4コア4スレッドなので縦横4ずつデータがありますね。表を見ると隣り合うコア(例:0コアと1コア、1コアと2コア)のレイテンシが小さいことがわかると思います。これは隣り合うコア同士なので接続の配線が短いからレイテンシが小さいのです。4コアなので差が見えにくいですが、コア同士が離れるとレイテンシが大きくなります。

## Core-i5 1035G1
こちらは4コア8スレッドのモバイル用CPUです。CPUには物理コアと仮想コアの概念があります。物理コアを仮想的に分割して処理を効率化することができます。この場合4コア8スレッドなので一つのコアを2つに分割しています。(厳密には若干違いますが)先程のRyzen3 2200Gは4コア4スレッドなので分割はされていません。  
  例えると人間を物理コアとします。1つのコアに1つのスレッドだと1本の手を使い作業します。2つのスレッドだと2本の手を使うことができ、作業の効率が上がります。  
  表を見ると先程は隣り合うコアのレイテンシ同士はすべて小さく、差がないように見られましたが、今回は隣り合うコア同士でもレイテンシに差がありますね。(例：0と1はレイテンシが小さいが1と2はレイテンシが大きい)これは0と1が同じ物理コア内の仮想コア同士だからです。同じ物理コア内の通信なのでレイテンシが小さいです。一方1と2はそれぞれ別の物理コアの中の仮想コアなのでレイテンシが大きくなります。同様に2と3は同じ物理コア内で完結するのでレイテンシが小さくなり、3と4はそれぞれ別の物理コア同士なのでレイテンシが大きくなります。
![Core-i5 1035G1](https://github.com/OR-gatti/Core-To-Core-Latency-Data/blob/main/Core_i5_1035G1_heatmap.png)
