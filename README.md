# JavaScript Interview Partice

##### Web Render Step
<p>

**Steps**
1. HTML => DOM Tree
2. CSS ⇒ CSSOM Tree
3. DOM Tree and CSSOM Tree ⇒ Render Tree
4. Render Tree ⇒ Layout
5. Paint Browser

</p>

##### URL Content
<p>
example: http://www.example.com:80/path/to/myPage?key=value1&key2=value2#someDocument

1. Protocol: 瀏覽器協議，通常都是HTTP or HTTPS ⇒ http
2. DomainName: 域名，對哪個Web進行請求，也可以是IP Address ⇒ www.exapmle.com
3. Port: 連線埠，就像一間公司下的某個部門(分機)
4. Path: 資源路徑，索取資源的地方
5. Parameters: 額外參數，用這些參數來進行額外操作
6. Anchor: 資源下的某一個書籤，定位資源的內容和方向，例如滾動到該anchor


**輸入網址到渲染畫面的過程**
1. 瀏覽器透過作業系統去找IP網址
2. 去DNS(查找伺服器)找相對應的IP Address
3. 把IP Address丟回Browser


**傳輸協定**
1. HTTPS(超文本傳輸安全協定): 透過HTTP進行通訊且使用SSL/TLS進行加密，S為security，再加上CA證書認證取得安全性，用非對稱加密進行互相溝通，但還是有可能會有中間人攻擊的風險。
2. 中間人攻擊: A與B之間會有一個C假裝成對方(A或B)，對其中資訊進行攔截與控制，然而雙方卻都不知情。
3. 非對稱加密: 發送者A與收發者B都會有一組鑰匙，分別是公鑰與私鑰，用其中一把鑰匙所加密的東西只能用另一把鑰匙去解密，舉例，A用B的公鑰加密只有B的私鑰能解密，反之亦然。
5. TCP協議: 彼此都會有接收和發送功能，三次握手確認彼此功能正常

**三次握手**
1. 你有收到嗎?
2. 我有收到?你有收到嗎?
3. 我也有收到

**TCP/IP四層模型(由上到下)**
1. 應用層(Application): HTTP、FTP，以上兩個應用層都是使用TCP傳輸模式
2. 傳輸層(Transport): TCP(保證收發正常)、UDP(不保證收發正常、傳輸速度快、效率佳，應用在即時性的訊息)
3. 網路互連層(Internet): IP Address
4. 網路存取層(Network Access): 海底電纜
</p>


