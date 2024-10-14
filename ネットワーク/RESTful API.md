### RESTful API 設計の6原則

#### 統一インターフェース（Uniform Interface)
全APIの操作において、HTTPのGET, POST, PUT, DELETEで情報を操作する。

#### アドレス可能性(Addressability)
取り扱うデータをリソースを一意な識別子:URI(Uniform Resource Identifier)で表現すること。
出来ればURLを見て接続している情報を判別できるのが一番理想的。
例えば、商品データを扱うシステムで、食品のカテゴリを指すURLには「category=foods」など。

#### ステートレス(Stateless)
サーバー側でセッションなどの状態管理をサーバ側でせず、処理に必要な情報を全てクライアントのリクエストに含める。

サーバ側がクライアントの情報を持ち続け、その情報で出力に反映するするのがステートレスとは反対のステートフルと呼ばれる。
#### 接続性(Connectability)
APIでやりとりする情報の内部に、別情報へのリンクを含む

#### クライアントとサーバの分離(Client-Server)
クライアントとサーバは互いに独立し、完全に分離していること。
互いに独立することを要求する目的は、サーバーとクライアントそれぞれ独立して進化（evolve)できるようにすることが目的
The Client-Server principle in RESTful APIs serves several important purposes:

1. Separation of Concerns: 
   - It divides the system into two main components: the client (user interface) and the server (data storage and business logic).
   - This separation allows each part to evolve independently without affecting the other.

2. Improved Scalability:
   - Servers can be scaled independently of clients to handle increased load.
   - Multiple client types (web, mobile, IoT devices) can interact with the same server.

3. Simplified Architecture:
   - By clearly defining roles, it simplifies the overall system design.
   - Reduces dependencies between client and server components.

4. Enhanced Portability:
   - Clients can be developed for different platforms without changing the server.
   - Servers can be replaced or upgraded without impacting clients, as long as the interface remains consistent.

5. Improved Maintainability:
   - Changes to the server's internal logic don't affect clients as long as the API remains stable.
   - Client-side code can be updated without server modifications.

6. Better Security:
   - Sensitive operations and data storage can be isolated on the server.
   - Clients only interact with the server through well-defined interfaces.

### オンデマンドコード
クライアント側が必要に応じてサーバにコードを要求しダウンロードする、ダウンロードしたコードをローカルで実行する。


