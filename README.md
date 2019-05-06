# ItemManager API v1 ドキュメント
[ItemManager](https://www.alesteq.com/itemmanager/)の開発者向けAPIのドキュメントです。




## 概要

ItemManagerで登録した商品データを、外部のプログラムから簡単にご利用いただけます。

+ ECサイトの商品ページや注文フローなど
+ 社内システムとのAPI連携



### リソース

以下のデータを取得します。

+ 商品一覧
+ 商品詳細
  + 基本情報
  + カラー展開
  + サイズと単価
  + 寸法表
  + タグ指定
  + 対応プリント方法
+ 商品カテゴリー
+ サイズ
+ サイズ分類
+ メーカー
+ 寸法箇所
+ タグ
+ タグ分類
+ プリント方法




## 認証

HTTPリクエストヘッダーで、Access-TokenとClient-Secretを送信します。

```
Authorization: Bearer {Access-Token}
X-Client-Secret: {Client-Secret}
```

Access-TokenとClient-Secretの発行には、アカウントの作成が必要です。

ログインした後に、マイアカウントでいつでも取得できます。



## リクエスト

以下のサンプルコード中の{Access-Token}と{Client-Secret}には、マイアカウントで取得したコードを使用します。

###### cURL

````cURL
curl -X GET \
  https://www.alesteq.com/itemmanager/api/categories/1 \
  -H 'authorization: Bearer {Acess-Token}' \
  -H 'x-client-secret: {Client-Secret}'
````

###### Node

```javascript
const https = require("https");
let options = {
  "protocol": "https:",
  "host": "www.alesteq.com",
  "path": "/itemmanager/api/categories/1",
  "method": "GET",
  "headers": {
    "Authorization": "Bearer {Acess-Token}",
    "X-Client-Secret": "{Client-Secret}"
  }
};
const req = https.request(options, (res) => {
  let data = "";
  res.on("data", (chunk) => {
    data += chunk;
  });
  res.on("end", () => {
    console.log(JSON.parse(data));
  });
});
req.end();
```

###### PHP

```php
$curl = curl_init();
curl_setopt_array($curl, array(
  CURLOPT_URL => "https://www.alesteq.com/itemmanager/api/categories/1",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_CUSTOMREQUEST => "GET",
  CURLOPT_HTTPHEADER => array(
    "authorization: Bearer {Access-Token}",
    "x-client-secret: {Client-Secret}"
  ),
));
$response = curl_exec($curl);
$err = curl_error($curl);
curl_close($curl);
if ($err) {
  echo "cURL Error #:" . $err;
} else {
  echo $response;
}
```

###### Python

```python
import http.client
conn = http.client.HTTPSConnection("www.alesteq.com")
headers = {
  "authorization": "Bearer {Access-Token}",
  "x-client-secret": "{Client-Secret}",
}
conn.request("GET", "/itemmanager/api/categories/1", headers=headers)
res = conn.getresponse()
data = res.read()
print(data.decode("utf-8"))
```

###### Ruby

```ruby
require "uri"
require "net/https"
url = URI("https://www.alesteq.com/itemmanager/api/categories/1")
http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE
request = Net::HTTP::Get.new(url)
request["authorization"] = "Bearer {Access-Token}"
request["x-client-secret"] = "{Client-Secret}"
response = http.request(request)
puts response.read_body
```

###### Javascript

```javascript
let data = null,
    xhr = new XMLHttpRequest();	
xhr.withCredentials = true;
xhr.addEventListener("readystatechange", function () {
  if (this.readyState === 4) {
    console.log(this.response);
  }
});
xhr.open("GET", "https://www.alesteq.com/itemmanager/api/categories/1");
xhr.setRequestHeader("authorization", "Bearer {Access-Token}");
xhr.setRequestHeader("x-client-secret", "{Client-Secret}");
xhr.send(data);
```

###### jQuery

```javascript
$(function(){
  let settings = {
    "async": true,
    "crossDomain": true,
    "url": "https://www.alesteq.com/itemmanager/api/categories/1",
    "method": "GET",
    "headers": {
      "authorization": "Bearer {Access-Token}",
      "x-client-secret": "{Client-Secret}",
    }
  };
  $.ajax(settings).done(function (response) {
    console.log(response);
  });
});
```



## レスポンス

JSON形式で返します。

```json
{
  "name":"test"
}
```

エラーの場合は、HTTPステータスコードとエラー内容のメッセージを返します。

```json
{
  "error":"エラーメッセージ",
  "code":"400"
}
```



## 商品情報の一覧を取得

指定された商品カテゴリーの商品一覧を返します。



### リクエスト

###### エンドポイント

+ `https://www.alesteq.com/itemmanager/api/categories/:id?tag[]={タグID}`

+ リクエストメソッド
  + GET

###### パラメーター

| 場所     | 名称  | 説明                                 | 必須 |
| -------- | ----- | ------------------------------------ | :--: |
| パス     | id    | 商品カテゴリID                       |  ◯   |
| クエリー | tag[] | タグIDの配列（e.g. tag[]=1&tag[]=2） |      |



### レスポンス

JSON形式

```json
[
  {
    "id":1,
    "code":"abc",
    "name":"アイテム名",
    "categoryCode":"t-shirts",
    "colorCode":"001",
    "price":9999,
    "caption":"見出し",
    "maker":"メーカー名"
  }
]
```

| キー         | 説明                                                         |
| ------------ | ------------------------------------------------------------ |
| id           | 商品ID                                                       |
| code         | 商品コード                                                   |
| name         | 商品名                                                       |
| categoryCode | 商品カテゴリーコード                                         |
| colorCode    | Web表示する際のデフォルトカラーコード                        |
| price        | 最低単価                                                     |
| caption      | 商品の説明文の見出しや、商品画像のタイトル等として使用するテキスト |
| maker        | メーカー名                                                   |



## 商品の詳細情報を取得

指定された商品IDの商品情報を返します。



### リクエスト

###### エンドポイント

- `https://www.alesteq.com/itemmanager/api/items/:id?opt[]={リソース}`
- リクエストメソッド
  - GET

###### パラメーター

| 場所   | 名称  | 説明                                                         | 必須 |
| ------ | ----- | ------------------------------------------------------------ | :--: |
| パス   | id    | 商品ID                                                       |  ◯   |
| クエリ | opt[] | 取得するリソースを指定、複数指定可。<span style="color:red">※</span> 省略した場合は全てを取得 |      |

###### クエリで指定できるリソース

| リソース名 | 説明                                    |
| ---------- | --------------------------------------- |
| color      | カラー展開（opt[]=color）               |
| size       | サイズ一覧と単価（opt[]=size）          |
| dimension  | 寸法表（opt[]=dimension）               |
| tag        | タグ指定（opt[]=tag）                   |
| print      | 対応しているプリント方法（opt[]=print） |



### レスポンス

JSON形式

```json
{
  "code":"abc",
  "name":"商品名",
  "category":"カテゴリー名",
  "maker":"メーカー名",
  "specific":{
    "caption":"見出し",
    "def_color_code":"001",
    "description":"商品説明",
    "material":"コットン100％"
  },
  "color":[
    {
      "code":"000",
      "name":"ホワイト",
      "pattern":1
    }
  ],
  "dimension":{
    "着丈":{
      "S":"67",
      "M":"70",
      "L":"73"
    },
    "身幅":{
      "S":"47",
      "M":"50",
      "L":"53"
    }
  },
  "printing":[
    {
      "code":"silk",
      "name":"シルクスクリーン"
    }
  ],
  "size":{
    "1":[
      {
        "size":"S",
        "group":"Men's",
        "price":999
      }
    ]
  },
  "tag":[
    {
      "id":1,
      "name":"タグ名"
    }
  ]
}
```

| キー                 | 説明                                                         |
| -------------------- | ------------------------------------------------------------ |
| code                 | 商品コード |
| name                 | 商品名 |
| category             | 商品カテゴリー名 |
| maker                | メーカー名 |
| specific | 商品詳細情報のハッシュ |
| specific.caption | 商品の説明文の見出しや、商品画像のタイトル等として使用するテキスト |
| specific.def_color_code | Web表示する際のデフォルトカラーコード |
| specific.description | 商品の説明文 |
| specific.material | 素材の説明文 |
| color                | 商品カラー毎の配列                                           |
| color.0.code | 商品カラーコード                                             |
| color.0.name         | 商品カラー名                                                 |
| color.0.pattern      | 商品カラーによってサイズ展開が異なる場合のサイズパターン番号。size.パターン番号に対応する。 |
| dimension            | 寸法箇所名をキーにした寸法のハッシュ（寸法表データ） |
| dimension.寸法箇所名 | 商品サイズをキーにした寸法のハッシュ                         |
| printing             | プリント方法の配列                                           |
| printing.0.code | プリント方法のコード |
| printing.0.name | プリント方法の名称 |
| size | サイズパターン番号をキーにしたサイズ展開のハッシュ |
| size.パターン番号.size | サイズ名 |
| size.パターン番号.price | 単価 |
| size.パターン番号.group | サイズ分類（e.g. Men's, Ladies） |
| tag | 商品タグ情報の配列 |
| tag.id | タグID |
| tag.name | タグ名 |



## マスターデータを取得

### リクエスト

###### エンドポイント

- `https://www.alesteq.com/itemmanager/api/masters/:resource`
- リクエストメソッド
  - GET

###### パラメーター

| 場所 | 名称     | 説明                           | 必須 |
| ---- | -------- | ------------------------------ | :--: |
| パス | resource | マスターデータを指定するコード |  ◯   |

###### resoueceで指定するコード

+ 商品カテゴリーのマスターデータを取得する場合
  + `https://www.alesteq.com/itemmanager/api/masters/category`
  + リクエストメソッド
    - GET

| マスターデータ | コード      |
| -------------- | ----------- |
| 商品カテゴリー | category    |
| サイズ         | size        |
| サイズ分類     | sizegroup   |
| メーカー       | maker       |
| 寸法箇所       | dimension   |
| タグ           | tag         |
| タグ分類       | tagtype     |
| プリント方法   | printmethod |



### レスポンス

各マスターデータをJSON形式で返します。



###### 商品カテゴリー

```json
[
  {
    "id":1,
    "code":"t-shirts",
    "name":"Tシャツ",
    "seq":1,
    "apply":"10000-01-01",
    "stop":"9999-12-31"
  }
]
```

| キー  | 説明                                 |
| ----- | ------------------------------------ |
| id    | 商品カテゴリーID                     |
| code  | 商品カテゴリーコード                 |
| name  | 商品カテゴリー名                     |
| seq   | Web表示順                            |
| apply | 取扱開始日、この日から適用されます。 |
| stop  | 取扱最終日、この日まで適用されます。 |



###### サイズ

 ```json
[
  {
    "id":1,
    "name":"M",
    "group":"Men's",
    "seq":1,
  }
]
 ```

| キー  | 説明                             |
| ----- | -------------------------------- |
| id    | サイズID                         |
| name  | サイズ名                         |
| group | サイズ分類（e.g. Men's, Ladies） |
| seq   | Web表示順                        |



###### サイズ分類

```json
[
  {
    "id":1,
    "name":"Men's",
    "seq":1
  }
]
```

| キー | 説明         |
| ---- | ------------ |
| id   | サイズ分類ID |
| name | サイズ分類名 |
| seq  | Web表示順    |



+ メーカー

```json
[
  {
    "id":1,
    "name":"メーカー名",
    "seq":1
  }
]
```

| キー | 説明       |
| ---- | ---------- |
| id   | メーカーID |
| name | メーカー名 |
| seq  | Web表示順  |



###### 寸法箇所

```json
[
  {
    "id":1,
    "name":"着丈",
    "seq":1
  }
]
```

| キー | 説明       |
| ---- | ---------- |
| id   | 寸法箇所ID |
| name | 寸法箇所名 |
| seq  | Web表示順  |



###### タグ

```json
[
  {
    "id":1,
    "name":"半袖",
    "type":"シルエット",
    "seq":1
  }
]
```

| キー | 説明       |
| ---- | ---------- |
| id   | タグID     |
| name | タグ名     |
| type | タグ分類名 |
| seq  | Web表示順  |



###### タグ分類

```json
[
  {
    "id":1,
    "name":"ブランド",
  }
]
```

| キー | 説明       |
| ---- | ---------- |
| id   | タグ分類ID |
| name | タグ分類名 |



###### プリント方法

```json
[
  {
    "id":1,
    "code":"silk",
    "name":"シルクプリント"
  }
]
```

| キー | 説明               |
| ---- | ------------------ |
| id   | プリント方法ID     |
| code | プリント方法コード |
| name | プリント方法名     |



## お問い合わせ

ItemManagerについての[お問い合わせはこちらから](https://www.alesteq.com/itemmanager/contact)。

