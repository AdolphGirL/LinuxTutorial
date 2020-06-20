#### 數位簽章說明
```
http://www.ruanyifeng.com/blog/2011/08/what_is_a_digital_signature.html
```

#### ssh原理
- ssh公鑰沒有像https，有CA認證中心
- 過程
  - 遠程主機收到用戶登入請求，把自己的公鑰發給用戶
  - 用戶使用該公鑰，將登入密碼加密後，發送回來
  - 遠程主機使用自己的私鑰，解壓登入密碼，如果密碼正確，同意用戶登入
- 風險
  - 如果有人攔截登入請求，冒充遠程主機，將偽照的公鑰發給用戶，如此用戶發送的密碼，就會到偽照的主機，用其自身的私鑰解密後就可以得到密碼，再用該密碼登入原先用戶想登入的主機，此過程是所謂的中間人攻擊。
- client第一次以ssh登入主機時，系統會出現是否接受遠程主機的公鑰，當接受後，惠要求輸入該遠程主機的登入密碼，用以確認正確性。而當遠程主機的公鑰被接受以後，他會被保存在文件$HOME/.ssh/known_hosts之中，下次在登入該主機時，遠程主機會認出其公鑰，就會跳過警告部分，直接提示輸入密碼。每個ssh用戶都會有自己的known_hosts文件，而客戶端本身的主機的系統也有一個這樣的文件，通常是/etc/ssh/ssh_known_hosts，保存一些對用戶都可以信賴的遠程主機公鑰(windows系統，透過工具，得自行查詢一下該工具的說明)。以上是需要輸入密碼的登入方式
- 公鑰登入，用戶將自己的公鑰儲存在遠端主機上。登入的時候，遠程主機會向用戶隨機發送一段隨機字串，用戶用自己的私鑰加密後再發送回來，遠程主機用之前的公鑰進行解密，如果成功，就證明戶用是可信的，就不會再要求輸入密碼，允許登入shell。
  - 這種方式要求用戶必須要公開自己公鑰，如果沒有現成，可以用ssh-keygen生成一組，一步驟生成，運行結束後會在$HOME/.ssh/目錄下生成兩個新文件，id_rsa.pub、id_rsd，前者是公鑰，後者是私鑰。再將公鑰傳送到遠程主機的host上，``ssh-copy-id user@host``
  - 如果上述過程不行，檢查/etc/ssh/sshd_config
    ```
    RSAAuthentication yes
    PubkeyAuthentication yes
    AuthorizedKeysFile .ssh/authorized_keys
    ```
- authorized_keys文件，遠端主機將用戶的公鑰。保存在登入後的用戶主目錄的$HOME/.ssh/
  authorized_keys文件中。公鑰就是一段字符串，只要將它追加在authorized_keys文件的末尾就行了
- 另一種不使用ssh-copy-id命令的方式，改用
  ```
  ssh user@host 'mkdir -p .ssh && cat >> .ssh/authorized_keys' < ~/.ssh/id_rsa.pub
  ```
  - ssh user@host登入遠程主機
  - mkdir -p .ssh && cat >> .ssh/authorized_keys，表示登入後在遠程主機shell上執行的命令
  - mkdir -p .ssh，表示用戶主目錄不存在.ssh目錄，就創建一個
  - cat >> .ssh/authorized_keys' < ~/.ssh/id_rsa.pub，將本地公鑰文件，重新定向，追加到authorized_keys的末端
- 參考網址
  ```
  https://www.twblogs.net/a/5cccc0f6bd9eee758fc149fd
  ```