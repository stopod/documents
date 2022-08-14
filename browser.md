### LocalStorage（ローカルストレージ）
html5から導入された。  
ユーザーのデータをwebブラウザ(ローカル環境)に保存することができる仕組み。  
ブラウザにデータを保存できる  
保存容量が大きい（5MB）  
データは永続的に保存される -> 削除する処理を書かないとデータが永続的に残り続ける  
javascriptから自由にアクセスできる -> 個人情報、機密性の高い情報は扱わないようにしよう
>> https://qiita.com/masuda-sankosc/items/cff6131efd6e1b5138e6


>localStorage プロパティはローカルの Storage オブジェクトにアクセスすることができます。  localStorage は sessionStorage (en-US) によく似ています。唯一の違いは、localStorage に保存されたデータには保持期間の制限はなく、sessionStorage に保存されたデータはセッションが終わると同時に（ブラウザが閉じられたときに）クリアされてしまうことです。  
localStorageまたはsessionStorageに保存されるデータはそのページのプロトコル固有であることに注意する必要があります。  
>> https://developer.mozilla.org/ja/docs/Web/API/Window/localStorage

### Cookie（クッキー）
localStorageとやれることはほぼ同じ。  
保存期間とデータ容量(4KB)に差がある。ローカルストレージであれば半永久的に保存できるが、  
Cookieの場合は保存期間が制限される。  