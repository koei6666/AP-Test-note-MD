### 無線LANの規格
もちろんです、IEEE 802.11の無線LAN（Wi-Fi）の主要な規格を以下の表でまとめます。

| 規格       | 最大転送速度  | 周波数帯       | 特徴                                          |
|------------|--------------|----------------|-----------------------------------------------|
| 802.11a    | 54 Mbps      | 5 GHz          | 早い段階での高速通信、しかし範囲は短い           |
| 802.11b    | 11 Mbps      | 2.4 GHz        | 低速だが範囲が広い、初期のWi-Fi規格の一つ       |
| 802.11g    | 54 Mbps      | 2.4 GHz        | 802.11bと後方互換、速度は802.11aに近い          |
| 802.11n    | 600 Mbps     | 2.4 GHz/5 GHz  | MIMO技術を採用、広い範囲と高速な転送速度を提供  |
| 802.11ac   | 1-6.77 Gbps  | 5 GHz          | MU-MIMO, 高密度の環境での高いパフォーマンス    |
| 802.11ax   | 10 Gbps      | 2.4 GHz/5 GHz  | OFDMA技術、多くのデバイスでの高パフォーマンス   |

#### 説明と実世界の例

- **802.11a**: この規格は高速な転送速度を提供しますが、2.4 GHz帯を使用する他の規格よりも通信範囲が短い傾向にあります。障害物に強い。
    - **実世界の例**: 初期の企業環境でよく用いられましたが、現在はあまり一般的ではありません。

- **802.11b**: 最初の商用Wi-Fi規格の一つで、広い範囲での通信が可能ですが速度が遅い。
    - **実世界の例**: 初期の家庭用Wi-Fiルーターで広く採用されました。

- **802.11g**: この規格は、802.11bと後方互換性がありながら、より高い転送速度を提供します。
    - **実世界の例**: 中小企業や家庭での高速インターネット接続によく使用されました。

- **802.11n**: MIMO（Multiple Input, Multiple Output）技術を採用しており、かなり高速で広い範囲の通信が可能です。
    - **実世界の例**: 多くの企業や家庭で広く採用されています。

- **802.11ac**: 現在主流となっている規格の一つで、非常に高い転送速度を持っています。
    - **実世界の例**: 現代の家庭や企業で一般的に使用されています。

- **802.11ax (Wi-Fi 6)**: 最新の規格で、多くのデバイスを高効率、高パフォーマンスでサポートすることを目的としています。
    - **実世界の例**: 最新のデバイスや高密度な環境（空港、スタジアムなど）での採用が増えています。


### IEEE 802.11a/g/n/acで用いられる多重化方式
デジタルデータをアナログデータに変換する際には、搬送波と呼ばれる信号に変調を加えてデジタルデータの０と1に対応させる。

IEEE 802.11a/g/n/acでは、この変調方式にOFDMが用いられる。

OFDM(Orthogonal Frequency Division Multiplexing:直交周波数分割多元通信)
周波数の異なる複数の搬送波を使って、パケットを並列に送受信することで転送効率を上げる技術である。変調速度を上げずにデータ通信を高速化できる。

### 無線LANのアクセス手順
1. ビーコン信号の受信と接続
   アクセスポイントは常に、自身のSSIDを含むビーコン信号をブロードキャスト送信しているので、無線LANを使うノードはこのビーコンによりアクセスポイントを認識し通信を始める。
   SSIDの発信を停止するステルス機能もある、発信を停止すると、事前にSSIDを知っているノードだけがアクセスできるようになる。
2. SSIDの確認
   同一SSIDを設定したノードにだけ接続を許可するが、どのノードからの接続も受け入れるという場合は、ANY接続を許可し、ノードがSSIDをANYとして接続すると無条件に接続が許可される。
   なお、MACアドレスフィルタリングすることで、事前にMACアドレスを登録したノードのみ接続を許可することもできる。
3. 暗号方式の確認
   送信するパケットを暗号化する手続きを行う。

### 無線LANのアクセス制御方式

#### CSMA/CA方式
無線LANは物理層媒体として電波を使うため、衝突の検出はできなく、なのでデータリンク層のCSMA/CDの衝突検出ではなく、衝突回避の手法を使っている。
手順は以下になる：
- 送信を行うノードは、利用したい周波数が使われていないかを確認後、必ずランダムな時間だけ待ってから送信を開始する。この待ち時間をバックオフ制御時間という。
- 衝突発生や、フレームの破損が発生しても検出できないので、データを受け取ったノードがACKを応答することでデータを正常に受け取ったことを通知する。
- 送信側ノードが、設定時間内にACKを受信しなければ、干渉が発生したと判断して一定時間後に再送信をする。

#### RTS/CTS
無線LANでは、ノード間の距離が離れすぎたり、間に障害物があったりすることで、互いに他のノードがデータ送信していることを感知できなく衝突を引き起こす。
RTS/CTSはこの問題を解決する、ノードが送信する前に、アクセスポイントにRTS(Request To Send)を送信、RTSを受信したアクセスポイントがCTS(Clear To Send)を返信することで、データの送信を開始する。
CTSには他のノードに対する送信抑止時間を記載しており、衝突を抑制する。

### 無線LANチャンネル割り当て
無線LANネットワークでは、近接するネットワークとの電波干渉を避けるために、使用する周波数帯をネットワークごとに微妙にずらすことが可能になっています。このときに設定する値をチャネル（チャンネル）といいます。  
  
2.4GHz帯を使用するIEEE802.11gでは1～13のチャネル（11bでは1～14）を選択できますが、1つのチャネルの周波数帯域は22MHz、各チャネルは5MHzずつ区切られているので、近接するチャネル同士は周波数帯が一部重なっていて電波干渉が起きます。このため、近くの無線LANネットワークで同じまたは近接するチャネルが使用されていると通信が不安定になってしまうという特性があります。なお、5GHz帯を使用する無線LAN規格では各チャネルの周波数帯は完全に独立しているので、近接するチャネルを設定しても2.4GHz帯のような電波干渉は起きません。

![[Pasted image 20230906231251.png]]


### IEEE 802.15.4 
IEEE 802.15.4 is a standard which specifies the operation of low-rate wireless personal area networks (LR-WPANs). It's designed to address the need for wireless connectivity among relatively simple devices that consume minimal power and may be located near each other. Here's a breakdown of some key aspects of this standard along with real-world examples:

1. **Frequency Bands and Data Rates:**
    - IEEE 802.15.4 operates in three unlicensed frequency bands: 2.4 GHz, 915 MHz, and 868 MHz, with data rates of 250 kbps, 40 kbps, and 20 kbps respectively.
    - For instance, home automation systems might utilize these bands to ensure wireless connectivity among devices like smart thermostats, lights, and locks.

2. **Network Topologies:**
    - Supports two types of network topologies: star and peer-to-peer.
    - In a star topology, all devices communicate through a central coordinator, similar to a home automation hub controlling various smart devices.
    - In a peer-to-peer topology, devices can communicate with each other directly, like in a sensor network where each sensor can relay information to its neighbors.

3. **Addressing:**
    - Devices within an IEEE 802.15.4 network can be assigned either a short 16-bit address or an extended 64-bit IEEE address.
    - This flexible addressing allows for both small, simplistic networks and larger, more complex networks.

4. **Low Power:**
    - The standard is designed for low power consumption, which is critical for battery-operated devices.
    - An example might be a battery-powered temperature sensor in a home automation system, which needs to operate for long periods without requiring a battery change.

5. **Security:**
    - Provides fundamental layers of security including AES-128 encryption to protect against eavesdropping, replay attacks, and authentication to ensure data integrity and confidentiality.
    - In a home security system, these features would help keep communication between sensors and the central hub secure.

6. **Application Layer Interfaces:**
    - While IEEE 802.15.4 focuses on the physical and MAC (Medium Access Control) layers, it also provides hooks for upper layer protocols like Zigbee, 6LoWPAN, and Thread which are used to create complete networking stacks.
    - For instance, Zigbee builds on IEEE 802.15.4 to provide a full networking solution for home automation, industrial control, and other low-power, low-data-rate applications.

In sum, IEEE 802.15.4 lays the groundwork for low-rate wireless communication between close-proximity devices, with applications ranging from home automation to industrial control systems. By defining key aspects like frequency bands, network topologies, addressing schemes, and basic security measures, it facilitates the creation of versatile WPANs that meet the needs of various low-power, low-data-rate, and close-proximity wireless communication scenarios.