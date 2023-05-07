# cInsightDAO

**Portal Repository for cInsightDAO**

TODO: URL の追加

プロジェクトホームページ  
https://chaininsight.tissis.xyz

Opensea  
https://testnets.opensea.io/collection/bonfire-v4

## プロジェクト概要

勉強会は、管理者に負担が集中しがちな上、質の高い発表に対する報酬や組織拡大への貢献に対する報酬がなく、長期間持続させるのは困難です。

この課題に対して、本プロジェクトは「自律分散的な意思決定機構」、「貢献に対するインセンティブ機構」、「持続可能なリファラル機構」の 3 つを持ち合わせた DAO を実装し、解決します。

意思決定機構については、NounsDAO に倣う形で実装しました。これにより、管理者なしにトランザクションの提案・投票・実行を通じた意思決定が可能になります。

DAO メンバーは、発表や討論において他のメンバーに「いいね」を付与することができ、その数に応じてメンバーの DAO への貢献度を表す「グレード」がアルゴリズムによって決まります。グレードの上昇は、焚き火 SBT の成長という形で可視化されます。グレードの高いメンバーは、DAO 内での影響力を高められる他、焚き火 SBT の色を変化させる火種の役割を持つスキン NFT を入手することができます。

更に、グレードが高いメンバーにはリファラル権が付与され、リファラル者には報酬が与えられます。これにより、勉強会の質を担保しつつ、メンバー間で友好的な関係を築きながら、DAO を拡大できることが期待されます。

## 背景と解決策

**背景**

- 高い質が担保された勉強会を、持続的に運営していくことは困難である。
  - (質担保) 発表・質問・フィードバックの質を担保するような機構がない。
  - (持続性) 管理者の負担が非常に大きい。報酬は与えられないことがしばしば。

**解決策**

1. （持続性）分散的な意思決定機構とアップデート機構
   - Nouns DAO ベースの分散化されたガバナンス機構
   - Proxy コントラクトを使用した Upgradability の担保
2. （質担保）リファラル機構
   - 紹介者 → 報酬、被紹介者 → 会員証購入価格の割引
   - リファラル権はグレードに応じて付与
3. （質担保・持続性）貢献に対するインセンティブ機構
   - 発表・コメントに対していいねを押すことができる
   - 付与されたいいねの数に応じてグレードが上昇し、報酬を得る
     - グレードに応じた SBT の進化
     - グレードに応じた特権・(着せ替え機能を有する) NFT の付与
   - 貢献を客観的に評価するために、オンチェーン情報を元に人間関係バイアスを低減

**※ 補足:** 高品質のコンテンツ・メンバーからなる団体を持続的に運営していく、という試み自体は、勉強会に限らず、任意の団体・組織で目指されるものであり、幅広い応用先を持つと期待される。

## 開発メンバー・役割分担

**前期**

- Frontend: @nisshimo (+@Shion-0014, @hidenoriwakiri)
- Backend: @nisshimo (+@Shion-0014,)
- Smart Contract
  - SBT, NFT: @hiroyaiyori, @Shion-0014
  - Governance: @hidenoriwakiri
- Deploy: @hiroyaiyori, @Shion-0014

**後期**

- @hidenoriwakiri, @Shion-0014, @katoatsushi (+@hiroyaiyori)

## 技術スタック

- Frontend: React / TypeScript

- Middle: Ethers.js

- Backend: Django

- Smart Contract: Solidity, Foundry (+ Anvil)

- Deploy: AWS, Nginx, Mumbai Network

## 各機能の詳細な解説

**(焚き火) SBT**

- SBT は譲渡不可能なトークンであり、一般に、会員証や保有者のステータスの証明書として使用されます。
- Chain Insight DAO では、会員証として 焚き火 SBT を発行します。さらに、この SBT は、DAO への貢献度に応じて動的に変化します。
- より詳細には、発表やコメントへのいいね、リファラルの回数を元にアルゴリズミックに貢献度 (薪の付与量) が決定され、そこからグレード (SBT の炎の大きさ) が決まります。
- SBT のグレードが高くなると、保持投票数やリファラル数の上限が増えるといった恩恵を受けることができます。

<img src="https://user-images.githubusercontent.com/61929751/236633485-da3db095-47fe-4381-a996-074b2c252301.png" width=８00px>

**(スキン) NFT**

- さらに、グレードの高いメンバーには、月初めに スキン NFT が付与されます。
- スキン NFT は言わば色付きの火種のようなものであり、保持している NFT の色に合わせて、アイコンとして表示される SBT の色を変化させることができます。
- NFT は譲渡可能であることから、OpenSea を始めとするマーケットプレイスで二次流通させることができ、金銭的なインセンティブとして機能することが期待されます。また、NFT の価値は DAO の価値・影響力に依存することから、DAO の拡大を目指すような動きが生まれることが期待されます。


<img src="https://user-images.githubusercontent.com/61929751/236633491-b29b2695-643e-498e-a15c-1899f0c399f5.png" width=800px>


**ガバナンス**

- [Nouns DAO](https://nouns.wtf/) のガバナンス機構をフォークする形で実装しており、トランザクションの提案・投票・実行の全てがスマートコントラクトを介して行われます。
- ここでいう「トランザクションの提案」とは、あるスマートコントラクトの関数を実行することに相当します。具体的には、例えば、以下のような提案を行うことができます:
  - DAO の運営に関わるロジック・パラメータの変更
  - DAO の金庫からの出資
- 提案の状態及び、それによって決まる実行可能な操作は全てスマートコントラクトで管理されており、非中央集権的な統治が実現されます。

<img src="https://user-images.githubusercontent.com/61929751/236634122-8c6b8288-59c7-425d-ad95-7bc679b72db5.png" width=900px>


**Proxy コントラクト**

- 移行性 (Upgradability) を高めるため、Proxy コントラクトを採用しています。下図のように Logic コントラクトを付け替えることで、柔軟に SBT, NFT, ガバナンス等の仕組みを変更することができます。
- Logic コントラクトの付け替えは、ガバナンスの仕組みを経て承認された投票をもってのみ行うことができます。

<img src="https://user-images.githubusercontent.com/61929751/236633507-5193d82d-698e-422d-a927-404d802a7dd5.png" width=800px>

**リファラル**

- DAO のメンバーには、毎月、グレードに応じて定まるリファラル権が付与されます。
- 紹介者には、リファラルを行うごとに薪が付与され、被紹介者は、会員証である SBT を通常より安い価格で購入することができます。
- これにより、メンバーの質を高く保ちながら、DAO を拡大できることが期待されます。

**人間関係バイアス軽減**

- 上述した他己評価に基づく貢献度の定量化が抱える問題点として、権力が一部の集団 (e.g. インフルエンサーとその取り巻き) に集中してしまうことで、正確・公平な評価がなされない危険性があることが挙げられます。
- そこで、Chain Insight DAO では、メンバーの行動履歴が全てオンチェーンに記録されていることに着目し、この記録から人間関係によるバイアスを軽減することを目指しています。
- 現在の実装では、リファラルの記録を元にグラフ (ネットワーク) を構築し、そこから計算された人物間の距離を元に、近い人物への評価の重みを小さくしています。

## 今後の展望

- いいね、リファラルの関係から構築されるネットワークの解析
- SBT データ・オンチェーンデータの拡充・解析
  - 勉強会に付与されるタグ及び、勉強会やコメントへのいいねの情報から、メンバーの各分野に対する専門性を定量化
  - 実装済みの勉強会への参加機能と組み合わせることで、聴衆のレベルの事前把握も可能に
- 貢献度・グレード算出アルゴリズムの改善
  - 貢献度算出の対象を広げる
    - いいねを押した側にも薪を付与
    - ロールを追加し (e.g. ファシリテーター) 、各ロールに対し、遂行完了に合わせて薪を付与
  - 薪を元にグレードを定義するアルゴリズムのブラッシュアップ
    - ネットワーク解析の結果を元にした、より良い人間関係バイアス軽減
    - SBT データを元にしたより正確な勉強会・コメントの貢献度の評価

## スマートコントラクト全体構成

<img src="https://user-images.githubusercontent.com/34847784/200167730-a41fe5da-6881-4b02-8164-95ff33bb1cc1.png" width=800px>

## 用語解説

- 焚き火 SBT  
  勉強会 DAO の会員証の役割を持つ SBT。焚き火から着想を得ており、コミュニケーションの活性化を図る様々な工夫が実装されている。
  DAO に対しての貢献に応じて SBT のグレードが決まり、グレードが高くなると火の勢いが増す。
  後述するスキン NFT を獲得すると、SBT の色や形態が変化する。

- いいね  
  感謝や賞賛を伝え他のメンバーの DAO への貢献を評価する手段として SBT 保有者は他の SBT 保有者に対していいねをすることができる。
  いいねは無料で行うことができるが、一月で使えるいいねの数には上限がある。
  上限を全てのメンバーについて一律に設定することによって組織内の富の分配を行なっている。無料でいいねできることによって価値の流動性が向上する。

- 薪  
  前月の獲得いいね数やコミュニティへの貢献に従って毎月初めに薪が獲得できる。この薪の数によって DAO 内でのグレードが決定される。
  関係性が遠い人物から獲得したいいねはより多くの薪に変換され、これによって新規参入を活性化している。この仕組みはシビルアタックへの対抗策にもなっている。
  所有している薪の数は月初めに一定の割合で減っていくため、初期メンバーに権力が集中しにくく新規参加者でも確かな貢献をしている人が高いグレードを得やすい構造となっている。

- グレード  
  所持している薪の数に応じて、グレードが決定される。グレードが高いメンバーには後述するリファラル権やスキン NFT などが付与される。また、グレードが高くなると SBT の火の勢いが増す。

- リファラル  
  この DAO はリファラル(紹介制度)を推進して組織を拡大していくことを目指している。
  グレードに応じてリファラルできる人数が決まる。リファラルにより参加する人は通常よりも安い値段で SBT(会員証)を購入できる。
  優秀な DAO メンバーの信頼する人脈から優秀な人を組織に参加させることができるため、勉強会の質を落とさずに DAO を拡大することができる。

- スキン NFT（新提案）  
  グレードが高いメンバーにはスキン NFT が付与される。保持しているスキン NFT によって SBT の見た目が変化する。
  このスキン NFT は二次流通させることもでき、DAO が拡大していくにつれてこのスキン NFT の価格も高騰していく。
  つまり、スキン NFT を保持しているメンバーにとっては DAO を拡大することがインセンティブにつながる。

TODO: コントラクトアドレスの更新

## 使用した BlockChain

Polygon テストネット Mumbai を使用

- Governance

  - Proxy.sol
    https://mumbai.polygonscan.com/address/0x6573330b11f30a3583bd4c15f7cbbbea912e0727
  - LogicV1.sol
    https://mumbai.polygonscan.com/address/0xd96ce135c3b83cec2a112e9b7aaaf41b68cdd159

- Executor

  - Executor.sol
    https://mumbai.polygonscan.com/address/0xb9bd0fe4a48c591fe69c6a322960b2841d5b7a52

- Bonfire

  - BonfireProxy.sol
    https://mumbai.polygonscan.com/address/0x3a76707e7e789fed03c01282d90d5e0a7d13fc1d

  - BonfireLogic.sol
    https://mumbai.polygonscan.com/address/0x81d391eee3aeef9438f66fe489569e5145d8d366

- SkinNFT
  ー SkinNft.sol
  https://mumbai.polygonscan.com/address/0x9abf727cbb849aa5077a00db29bf7495fee5e9ac
