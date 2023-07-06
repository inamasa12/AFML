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
一次モデルによるサイドを前提に、正答とそれ以外の2つのラベル（メタラベリング）でサイズを予測する二次モデルを学習（二段階予測）  
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











