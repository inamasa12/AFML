# ファイナンス機械学習メモ

1. ファイナンス機械学習という新分野  
- はじめに  
学術界と実務界の橋渡し  
科学的な投資の普及  
機械学習を投資に活用することの複雑さ  
- ファイナンス機械学習プロジェクトが失敗する主な原因  
ジャッジメンタル戦略は個別性が高く、分業が容易でないため、各人が独立してそれぞれの戦略を構築すべき  
クオンツ戦略は分業が容易なため、それぞれが特定のタスクのエキスパートになるべき  
- FAQ  
機械学習アルゴリズムは意思決定の高速化、正確性、改善可能性を高める  
機械学習アルゴリズムは人間の認識能力を遥かに超えており、かつ高速に進歩している  
ファイナンス機械学習は従来の計量経済学に比べて、圧倒的にモデリングの自由度が高い  

2. 金融データの構造  
情報に応じたサンプリングサイクル  
現実に応じた系列の修正  
- 金融データの基本形式  
保存、加工、取り扱いが難しいデータほど、期待できるデータである  
- バー  
何らかのサイクルで作成されたデータテーブルの各行  
    - 標準バー  
タイムバーで作成されたデータは統計的に望ましい性質を持たないことが多いため、他のバーが考え出された（恐らく最も望ましいのはドルバー）  
タイムバー: 一定時間ごとに作成されたバー  
ティックバー: 一定取引回数ごとに作成されたバー  
ボリュームバー: 一定取引数ごとに作成されたバー  
ドルバー: 一定金額ごとに作成されたバー  
    - 情報ドリブンバー  
一定の情報量ごとにサンプリングしたバー  
価格が均衡水準に達する前に意思決定に必要なデータを収集する目的で作成  
同方向への価格変化が一定以上連続した場合にサンプリングを行う（有意な情報が発生したタイミングでサンプリング）  
インバランスバー、ランバー等の種類がある  
- マルチプロダクトの取り扱い  
正しい分析のためには、コストを反映し、純粋な価格変動のみを抽出した時系列を作成する必要がある（ETFトリック）  
- 特徴量サンプリング  
実際には作成されたバーからサンプリングしたデータを分析に用いる場合が多い（サンプル自体を減らす、有意なサンプルに限定する）  
    - データ量削減のためのサンプリング  
一定バー間隔、ランダムサンプリング  
    - イベントベースサンプリング  
特定の状況をモデル化するためのサンプリング（CUMSUMフィルタ⇒逆方向に価格変化が一定以上進んだ場合にサンプリング）   
3. ラベリング  
動的な変動幅の使用  
実運用を考慮したラベリング（トリプルバリア法）  
二段階予測を行うためのメタラベリング    
- 固定時間ホライズン法  
一定期間の変動に応じてカテゴリ変数を割り当て  
望ましい統計的性質を示さない、ボラティリティのレジーム変化に対応していない等の欠点がある  
現実（ボラティリティの変化、ロスカット等）を考慮した調整が必要  
- トリプルバリア法  
変動に利食い、ストップロス、有効期限を設定    
- サイドとサイズの学習  
上昇、不変、下落の3つのラベルで戦略のサイド（Buy or Sell）とサイズ（ポジション量）を学習  
- メタラベリング  
一次モデルによるサイドの予測を前提に、正答とそれ以外の2つのラベル（メタラベリング）でサイズを予測する二次モデルを学習（二段階予測）  
- メタラベリングの使用方法（二段階予測の利点）  
モデルの説明力が増す  
オーバーフィットが限定的になる  
複数のモデルを高度に組み合わせ  
サイドに特化したモデル構築  
- クオンタメンタルな手法  
メタラベルと実際のポジションを比較することで定性判断の評価や要因分析が可能  
- 不要なラベルの削除  
出現回数が少ないラベルは削除すべき  

4. 標本の重み付け  
観測値がIIDではない場合の対処法  
- 重複した結果  
異なるサイクルでサンプリング（例えばトリプルバリア法）した変数に対するリターンラベルには情報の重複が生じるため、互いに独立にはならない  
一方で固定ホライズン法によるサンプルは統計的に望ましい性質を持たない  
- ラベルの平均独自性  
各サンプルの任意の時点の独自性を重複するサンプル数の逆数で定義する  
独自性の時系列平均を任意のサンプルの平均独自性とする  
- 分類器のバギングと独自性  
バギングを（ブートストラップで得られた標本による複数モデルのアンサンブル）を前提  
平均独自性に応じた発生確率でサンプルを抽出する方法を逐次ブートストラップという  
標準的なブートストラップに比べて標本の平均独自性は有意に高くなる  
- リターンによるサンプルの重み付け  
絶対リターンが大きく独自性の高いサンプルに大きなウェイト  
- 時間減衰
古いデータほどウェイトを小さくする  
- クラスの重み付け  
特定クラスの予測精度に重きを置くべきかどうか

5. 分数次差分をとった特徴量  
データを定常にする過程でシグナルを弱めてしまうことはありうる  
可能な限り情報を残しつつ、データを定常にする方法　⇒ 分数次差分  
分数次差分の系列は長期の系列相関を示し、予測力が高くなる  
- 拡大ウィンドウ  
最長データに対する損失が一定以上となる時点以降の範囲でデータを加重  
ウィンドウの範囲は徐々に広がっていく  
ウェイトは時間を経る毎に小さくなっていく（下方ドリフト）    
- 固定幅ウィンドウ  
絶対値が一定の大きさ以上のウェイトの範囲で加重  
ウィンドウが固定されているためウェイトの下方ドリフトは生じない  
- 最大限メモリーを保存した定常性  
分数次差分の系列について、係数dを元系列との相関を維持しながら定常性を棄却できる水準に設定する  
dを大きくするほど完全に定常な系列に近づくが、元系列との相関も低下する（メモリーを失う）  

6. アンサンブル法  
観測値がIIDでない場合の注意点、対応方法  

- 誤り（予測誤差）の3つの要因  
バイアス: 予測誤差、アンダーフィット  
バリアンス: 予測のばらつき、オーバーフィット  
ノイズ: 真の誤差  

- ブートストラップアグリゲーション  
各分類器の予測の相関が1より小さい限り、分類器を増やすほどバギングのバリアンスは低下する  
各分類器の正解率が低ければ、分類器を増やしてもバギングの正解率はほとんど改善しない（バギングの主目的はバリアンス低下≒オーバーフィット回避）  
バギングの各サンプリングにおいても、データの独自性は考慮すべき（観測値がIIDでない場合はオーバーフィットし易い）

- ランダムフォレスト  
ランダムフォレストは各分類器の特徴量にランダム性を持たせているため、本来オーバーフィットはしにくい  
それでもバギング同様に、データの平均独自性に応じたサンプリングや、逐次ブートストラップの適用は有効  

- ブースティング
金融データはIIDではなくオーバーフィットし易いため、一般的にブースティングよりバギングが望ましい（ブースティングはバイアス削減、アンダーフィット解消に有効）  
ブースティングは逐次実行のため処理時間を要する

- バギングのスケーラビリティ  
最適化に早期の終了条件を課した分類器を多く用意することでバリアンスの削減は可能  

7. ファイナンスにおける交差検証法  
ファイナンス領域で交差検証法が機能しない理由と対応  
- なぜファイナンスでk-分割交差検証方法がうまく機能しないのか  
説明変数がIIDではなく（系列相関がある）、結果としてラベルもIIDではない  
対処方法として、重複したデータの除外、早期終了のバギング、平均独自性に応じたサンプリング、逐次ブートストラップ  
以下のパージング、エンバーゴも有効  
- パージング  
訓練データセットからテストデータに含まれるラベルと時間が重複しているラベルのデータを除く  
- エンバーゴ  
訓練データセットからテストデータのラベル計測終了時刻直後のデータを除く  

8. 特徴量の重要度  
バックテストで良い結果を出すことが重要なのではなく、そこから重要な特徴量とその特性を見出すことが重要  
- 代替効果  
他の特徴量によって置き換えられる特性  
- 平均不純度減少量（MDI）  
ランダムフォレストにおいて、ある特徴量を用いた分割で得られた不純度減少量を全てのケースについて平均する  
各特徴量の不純度減少量平均値の全特徴量の値に対する割合を最終的な平均不純度減少量（MDI）とする  
代替効果のある特徴量が他にある場合には値が薄まる  
- 平均正解率減少量（MDA）  
アウトオブサンプルのテストで任意の特徴量を並べ替えた時の評価値の低下で定義  
代替効果のある特徴量が他にある場合は低下しにくい
- 単一特徴量重要度（SFI）  
各指標を単独で用いた場合の正答率  
代替効果の影響を除き、指標単独の効果を見ることができるが、逆に相互効果等は考慮しない  
- 特徴量の直行化（PCA）  
特徴量を直行化することでも代替効果の影響を低減することができる  
ここの次元削減を加えることもできる  
PCAの重要性（固有値）と特徴量の重要度が一致した場合には当該指標の信頼性が高い  
- 特徴量重要度の並列計算、スタック計算  
並列計算では、銘柄×特徴量×評価尺度の単位で重要度を算出する  
スタック計算では特徴量を全銘柄で集約し、特徴量×評価尺度の単位で重要度を算出する  
スタック計算には、サンプル数が大きい、オーバーフィッティングのリスクが小さい、代替効果の影響が少ない等の利点がある  

9. 交差検証法によるハイパーパラメータの調整  
- グリッドサーチ交差検証法  
交差検証法を用いてグリッドサーチを行う  
- ランダムサーチ交差検証法  
検証するハイパーパラメータのグリッドを設定するのではなく、分布を設定する  
この場合、対数一様分布などが用いられる  
- 評価指標  
ハイパーパラメータの選択においては、クロスエントロピー損失を用いるのが望ましい  
クロスエントロピー損失では、予測確率、リターンの大きさを評価に取り入れることが出来る  

10. ベットサイズ（ポジション量）の決定  
サイドの予測とは別にベッドサイズを決めるアプローチ  
- 戦略とは独立にベットサイズを決定する方法  
ベット数が多い（少ない）ほどベットサイズを大きくする（小さくする）方法 ⇒ ベット数が多いほど、更にベット数が増える確率は減るため、ベットサイズを大きくできる（ベットの追加に備えたポジショニング）  
- ベット1単位を1/最大のベット数とする ⇒ 予算編成アプローチ）  
- メタラベリングを適用する ⇒ 機械学習で予測する  
予測確率の検定量の累積密度関数を-1から1の値に返還する関数を定義（ベットサイズの関数）  
アクティブなベットの平均値を最終的なベットサイズとする（回転率抑制）  
更に回転率を抑えるために離散化すると良い  
- 動的なベットサイズと指値  
ベットサイズを予測価格と市場価格の乖離を変数とした関数で定義する  
この関数にはシグモイド関数やべき乗関数が用いられる  
目標ポジションに到達するために売買する各単位ポジションの想定平均価格が損益分岐点となる  

11. バックテストの危険性  
バックテストを繰り返して行い、最も結果の良かった戦略を採用する危険性  
- バックテストの問題点  
バックテストの目的は、ベットサイズ、売買回転率、コスト、各局面の挙動を確認すること  
再現はない  
ランダムなパターンを正当化するために、事後的にストーリーを作り上げてはならない  
ランダムな事象にフィットしたモデルを作らない  
バックテストの結果を追い求めること自体が誤り  
バックテストの影響下でリサーチをしてはならない  
バックテストが失敗したら、モデルの構築からやり直すべき  
- バックテストに関する問題点の対応方法  
より広い範囲でのモデル構築を心掛ける  
バギングの利用  
バックテストは最後に行う  
シナリオをシミュレーションする  
- 組合せ対象交差検証法（CSCV）
    - 時点を行、各バックテスト結果を列としたデータセットを用意  
    - データセットを偶数の複数期間のサブセットに分割する  
    - サブセットから半数のサブセットを選択する全ての組合せを作成  
    - 任意の組合せデータセットAとその補集合となるデータセットBを用意  
    - Aの各列の評価値を要素としたベクトルaを作成  
    - aの最大要素の列番号をiとする  
    - 同様Bについて作成した評価値ベクトルbのi番目の要素の相対ランクをxとする  
    - 全ての組み合わせについてxを算出し、そのロジット値がゼロを下回る確率をオーバーフィッティング確率（PBO）とする  
    - これはインサンプルで最良の戦略がアウトサンプルでアンダーパフォームする確率である  

12. 交差検証によるバックテスト  
過去データを用いたバックテストは一つの経路しかありえない  
データを組み替えてバックテストの経路を増やす方法  
- ウォークフォワード（WF）法
訓練を行った後の期間のデータを用いてバックテストを行う方法  
単一の経路でしかバックテストできないため、オーバーフィットしやすい  
訓練は過去のデータに限定しているため、活用するデータ数が少なくなってしまう  
各バックテストの違いはトレーニング期間の長さのみ  
- 交差検証法（Cross Validation Method）
バックテストの各期間に対してそれ以外の期間で学習したモデルをシミュレートする  
ウォークフォワード法同様に単一経路しかできない
時間軸の逆転は起こるが、より多くのデータを活用して検証ができる  
- 組合せパージング交差検証法（Combinatorial Purged Cross Validation）  
選択した複数期間の組合せでバックテストを行い、それ以外の期間でトレーニングを行う  
複数経路のバックテストが可能になる  
但し、トレーニングデータが減る（経路の数とトレードオフ）  
- CPCVは何故バックテストオーバーフィット問題を解決するのか  
バックテストにおけるシャープレシオの最良値の期待値は、試行回数をI、シャープレシオをyとして $\sigma\lparen{y}\rparen\sqrt{2log{I}}$  
⇒ 試行回数を増やせば大きくできる  
一方でCPCVのシャープレシオのばらつきは、バックテスト経路を増やすことで減らすことができる  
  
13. 人工データのバックテスト  
- 取引ルール  
予測モデルに応じた取引ルールの作成が必要  
多くの場合、取引ルールの設定はウォークフォワードのバックテストに依存するため、オーバーフィッティングの可能性が高い  
適切な確率過程を仮定し、最適なパラメータを推定する方法もある  
- 問題設定  
ウォークフォワードのバックテストでは、取引ルールは投資機会の数のサンプルに対して最適化されるため、オーバーフィッティングのリスクが極めて高い  
インサンプルで最適とされた取引ルールをアウトサンプルに適用した時に、その評価値がアウトサンプルの中央値を下回った場合はオーバーフィットが発生していると言ってよい  
- フレームワーク  
人口データのバックテストでは価格に確率過程を設定する必要がある  
テキストでは平均回帰のOU過程が想定されている  
- 最適取引ルールの数値決定（テキストの場合）  
① 実現値から確率過程のパラメータを推定  
② 価格の予想値と確率過程の半減期毎にシミュレーションを設定  
③ 検証が必要な取引ルールの組合せを作成  
④ ②と③の全ての組合せに対して○○万回のシミュレーションを実施  
⑤ ②の各組み合わせに対してシミュレーション結果の評価値の値を分布として評価  

14. バックテストの統計値  
戦略の評価指標  
- 一般的な特性  
テスト期間  
平均AUM  
キャパシティ（最大AUM、最小AUM）  
レバレッジ（平均ドルポジション / 平均AUM）  
最大ドルポジション  
ロング比率（平均ロングドルポジション / 平均ドルポジション）  
ベット頻度  
平均保有期間  
回転率（平均取引額 / 平均AUM）  
基本ユニバースとの相関  
- パフォーマンス  
PnL（利益総額）  
ロングポジションからのPnL  
収益率、ヒット率  
利益、損失それぞれの平均リターン  
リターンの集中度、ベットの時間的集中度  
ドローダウン、ハイウォーターマーク、アンダーウォーター期間  
- 執行コスト  
一回転あたりのブローカー手数料、総手数料  
一回転あたりのコスト控除後ドルパフォーマンス  
ドルパフォーマンスと総コストの比率  
- 効率性  
確率的シャープレシオ: 分布の歪みを調整したSRのZ値  
収縮シャープレシオ: 試行回数を調整した目標SR  
- その他  
正答率等の機械学習系評価指標  
リターンの要因分解  

15. 戦略リスクを理解する  
コントロール可能なパラメータ（ベット回数、フロアの設定等）に応じた戦略の評価  
実現するペイアウトに二項過程を想定  
- 戦略のシャープレシオ    
ベット回数をn、戦略の適合率（ベットが当たる確率）をpとするとペイアウトが対称な戦略のシャープレシオは下記となる  
$\theta=\dfrac{2p-1}{2\sqrt{p(1-p)}}\sqrt{n}$  
ペイアウトが非対称であっても、同様の公式は導ける  
- 戦略的失敗の確率  
上式から戦略が目標のシャープレシオを下回る適合率pを逆算することができる  
更にシミュレーションから、この適合率の実現確率を推定できる  
① 実現利益のデータからサンプルを復元抽出し、適合率を計算する  
② ①を繰り返す  
③ カーネル密度推定量を適用し、適合率の確率密度関数を近似推定する  
④ この分布における、上記目標シャープレシオを下回る適合率の実現確率を求める  

16. 機械学習によるアセットアロケーション  
階層的リスクパリティアプローチ（HRP）の紹介  
二次計画法による最適化の課題である不安定性、集中制性、アンダーパフォーマンスに対処  
HRPは共分散行列の正則性を必要としない  
- ポートフォリオの凸最適化に伴う問題  
HRPは予測リターンの小さな変動で結果が大きく変わることが示されている  
一方で通常の二次計画法は正定値共分散行列の逆行列（各変数の全体に対する偏相関）を必要とするが、最適化条件の数が大きいときに、この逆行列の予測誤差は大きくなる  
- マーコウィッツの呪い  
最適化条件の数は最大固有値と最小固有値の差で決まるが、これは相関を持つ資産が投資対象に加わると上昇する  
また、分散共分散行列の推定に必要なデータ数は投資対象の増加に伴って激増する  
- 幾何学的関係から階層的関係へ  
二次計画法では、予測誤差の大きい値に基づき全ての変数が同等に扱われるため、計算結果が極端なものになり易く、変動も大きくなる
- これを回避するため、クラスタリングのテクニックを用いて分散共分散行列を準対角化し、これをウェイトに変換する方法（再帰的二分）  
    - 階層的なクラスタリングであれば方法は何でもよい  
    - クラスタリング結果に基づき、相関のある変数が隣接するように分散共分散行列を並び変える（準対角化）  
    - 再帰的二分  
    ① 二分したグループ毎にリスクパリティポートのリスクを算出し、これに逆比例するように両グループへのウェイトを決定する  
    ② 各グループを更に二分して①を行い、一変数になるまでウェイトを分割していく  
-  結果として、相関が低いグループにウェイトが分散され、分散が大きい（小さい）資産へのウェイトが減る（増える）   
- HRPはウェイトの偏りが大きい最小分散ポートとリスクパリティポートの中間的な性質を持つ  
- 特定資産へのショックは最小分散ポートに大きなマイナスの影響を与え、互いに相関を持つ資産へのショックはリスクパリティポートに大きなマイナスの影響を与える（HRPは相関を考慮し、資産分散もなされている）  
- HRPは、算出過程において、分散共分散行列の逆行列を用いないところがメリット  

17. 構造変化  
構造変化の可能性を測る方法  
- 構造変化の検定の種類  
CUMSUM検定: 累積予測誤差のホワイトノイズからの乖離  
爆発性検定: 指数的な増加もしくは減少  
- CUMSUM検定  
    - 再帰的残差のブラウン・ダービン・エバンズCUMSUM検定  
    時系列線形モデルの回帰係数の正規性を検定  
    - 水準のチュ・スティンクコンベ・ホワイトCUMSUM検定  
    被説明変数の変化の正規性を検定  
- 爆発性検定  
    - チャウタイプ・ディッキー・フラー検定（CTDF）  
    任意の期間における被説明変数のジャンプを検定  
    - 上限拡張ディッキー・フラー検定（SADF）  
    CTDFを多期間に拡張  
    対数価格系列に適用  
    多期間の二重ループになるため計算負荷が大きい  
    異常値の影響を回避するため、分位ADF、条件付きADFの方法がある  
    - 劣/優マルチンゲール検定  
    各種時間トレンドに対する回帰係数の大きさを検定  

18. エントロピー特徴量  
価格に含まれる情報が少ないほど情報の非対称を利用でした運用戦略を構築できる  
価格系列に含まれる情報量を決定する方法を示す  
- シャノンのエントロピー  
レアなイベントに対する情報ほど高く評価される  
$H\lbrack X \rbrack=-\displaystyle\sum_{x \in A}p\lbrack x \rbrack \log_{2}p\lbrack x \rbrack$  
冗長性は平均的に対する情報の多さの逆数  
$R\lbrack X \rbrack=1-\dfrac {H\lbrack x \rbrack} {\log_{2}H\lbrack \lVert A \rVert \rbrack}$  
相互情報量は二変数が全く同じ情報の場合に最小値ゼロとなる  
$MI\lbrack X \rbrack=H\lbrack X \rbrack+H\lbrack Y \rbrack-H\lbrack X,\space Y \rbrack$  
- プラグイン推定量  
エントロピーレート（1文字当りのエントロピー）の推定量は下記（文字列長w）  
$\hat{H}_{n,\space w}=-{\dfrac 1 w}\displaystyle\sum_{y^w_{1} \in A^w}\hat{p}_{w}\lbrack y^w_{1} \rbrack log_{2}\hat{p}_{w}\lbrack y^w_{1} \rbrack$
- レンペル・ジブ推定量  
複雑なデータ系列ほど多くの情報を持ち、エントロピーも高いと考えられる    
データの非冗長部分を系列に現れる要素数や重複の長さで評価することが出来る  
ウィンドウサイズnでデータ系列（メッセージ）を評価した時の最大マッチ長をLとするとエントロピーは以下のように算出できる（各マッチ長k）  
固定ウィンドウ、拡大ウィンドウの順に記載  
$\hat{H}_{n,\space k}=\bigg\lbrack{\dfrac 1 k}\displaystyle\sum_{i=1}^k {\dfrac {L^n_1} {\log_{2}\lbrack n \rbrack}} \bigg\rbrack^{-1}$  
$\hat{H}_{n}=\bigg\lbrack{\dfrac 1 n}\displaystyle\sum_{i=2}^n {\dfrac {L^i_i} {\log_{2}\lbrack i \rbrack}} \bigg\rbrack^{-1}$  
ドーブリン条件を満たさない場合は下記  
$\bar{H}_{n,\space k}={\dfrac 1 k}\displaystyle\sum_{i=1}^k {\dfrac {\log_{2}\lbrack n \rbrack} {L^n_i}} $  
$\bar{H}_n={\dfrac 1 n}\displaystyle\sum_{i=2}^n {\dfrac {\log_{2}\lbrack i \rbrack} {L^i_i}} $  
メッセージ長Nの場合、k+n=Nとなるようにkとnは選択すべきとされている  
この時、Nとnの関係は下記となる  
$N \approx n + \lparen \log_{n}\lbrack i \rbrack \rparen^2$  
メッセージ長Nが短い場合は複数回繰り返したメッセージを評価する、等の実務的取り扱いが幾つか存在する    

- 符号化方式  
エントロピー推定の前にメッセージ（何らかの情報列）を符号化しておく必要がある  
    - 2元符号化  
    リターン系列の正負で分ける（リターンゼロは捨てる）  
    リターンが幅広い値を取る場合、上記の方法では多くの情報を捨てることになるため、価格系列をタイムバーではなく、トレードバーやボリュームバーで作成する等の工夫をする  
    - クオンタイル符号化  
    インサンプルを同数に分ける分位点で分ける  
    - シグマ符号化  
    インサンプルの標準偏差値等、何らかの固定値で分ける  

- ガウス過程のエントロピー  
正規分布に従う過程（メッセージ）のエントロピーは下記  
$H={\dfrac1 2}{\log\lbrack 2 \pi e \sigma^2 \rbrack}$  
特に標準正規分布であれば、H≒1.42  
エントロピーのボラティリティは下記  
$\sigma_{H}={\dfrac {e^{H-1/2}} {\sqrt{2\pi}}}$  
- エントロピーと一般化平均  
シャノンのエントロピーは一般化平均の極限として表すことが出来、メッセージ内の有効な情報の数の対数値が下限になる（全ての情報の発生確率が等しい場合）  
- エントロピーの金融市場への応用  
価格情報のエントロピーが高いほど、冗長性が低く、情報の中身が増えるため、効率的である  
可能性のある価格系列のうち、エントロピーが最大のものが最も利益が得られる  
ポートフォリオの集中度をエントロピーで表現できる（リターン変動要因のばらつき）  
ボリュームクロックのもとで、PIN（インフォームドトレーダーからの注文割合）はオーダーインバランスの関数となる  
ここでオーダーインバランスの予測可能性は、インバランスの時系列からエントロピーとその分布を推定することで求められる  


19. マイクロストラクチャーに基づく特徴量  









  
    
















