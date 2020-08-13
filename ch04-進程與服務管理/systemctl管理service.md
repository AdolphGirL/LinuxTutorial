#### service
- systemctl 指令管理 Systemd 的系統服務
- 在 Systemd 中每一個系統服務就稱為一個服務單位（unit），而服務單位又可以區分為 service、socket、target、path、snapshot、timer 等多種不同的類型（type），我們可以從設定檔的附檔名來判斷該服務單位所屬的類型，最常見的就是以 .service 結尾的系統服務，大部分的伺服器都是屬於這種。
- systemctl 操作指令 服務名稱.service(當我們在指定服務名稱時，可以將結尾的 .service 省略)
- systemctl 的 start 與 stop 兩個操作指令是用來控制目前服務的狀態，如果想要設定開機自動啟動服務的話，就要改用 enable 與 disable  
  ```
  # 設定開機自動啟動 nginx 網頁伺服器
  systemctl enable nginx

  # 取消開機自動啟動 nginx 網頁伺服器
  sudo systemctl disable nginx
  ```
- 檢測系統服務狀態
  ```
  # 檢查 nginx 服務是否正在運行
  systemctl is-active nginx.service

  # 檢查 nginx 服務是否有設定開機自動啟動
  systemctl is-enabled nginx.service

  # 檢查 nginx 服務是否啟動失敗
  systemctl is-failed nginx.service
  ```
- 列出 Systemd 所有服務  
  ```
  systemctl list-units
  ```
- 如果想要列出系統上所有的服務（包含已啟動與未啟動的），可以加上 --all 參數：  
  ```
  systemctl list-units --all

  # 列出系統上所有未啟動的服務
  systemctl list-units --all --state=inactive

  # 只列出系統上所有 service 類型的服務
  systemctl list-units --type=service
  ```
- 服務內部設定與狀態   
  ```
  systemctl cat adt.service

  # 查看特定服務的相依服務
  systemctl list-dependencies sshd.service
  ```
  