## CORSについて

---
### CORSとは
CORS = Cross-Origin Resource Sharing(オリジン間リソース共有)  
リソース共有->データ共有的な  

オリジン-> URLみたいなもの  
https:<- スキーム  
youtube.com<- ドメイン  
:443<- ポート  
オリジン = スキーム＋ドメイン＋ポート番号  
(始まり、みたいな英単語らしい)  

---
### そもそもwebは
同じオリジン間でしかリソースのやり取りを許可していない  
(同一生成元ポリシー)<- これがデフォルトの設定らしい  
許可されていないオリジン間でのデータのやり取りが発生すると「blocked by CORS policy」が発生する  

---
### 開発者にとっては不便
localhost:5500(CL)とlocalhost:5000(SV)とかだと許可されてなくてエラーになっちゃう

---
### 解決方法
SV側で許可してあげる


---
### 参考
- https://www.youtube.com/watch?v=8fE2TmbPqlU