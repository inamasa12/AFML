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
