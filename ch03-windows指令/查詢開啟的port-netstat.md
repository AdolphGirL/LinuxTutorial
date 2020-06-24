#### netstat 
- 在 windows 下使用 netstat 指令來查詢已開啟的 listen port (-a), 以及已建立的連線 (active connection) 是十分方便的工具. 不過若是要知道該 connection 或 listen port 是由哪個 process 建立的, 在 netstat 指令下是無法達成的, 必須藉由另一個 sysinternals (被 ms 併入了)的 tcpview 工具
- 不過, 現在可以不用這麼麻煩了, netstat 工具已經增加了這個功能, 指令是 (-b), 例如 netstat -nb
- 參數
  - -a : 顯示所有活動中的 TCP 連線，及 TCP and UDP ports 上聆聽中的資訊。
  - -e : 顯示網路的統計資訊，如 bytes 數和封包發送和接收的數量.這參數通常和 -s 並用。
  - -n : 顯示活動的TCP連線，但是 ip address和port編號沒有被解釋翻譯成為名稱說明。(通常可以加速顯示的速度因為反解通常需要查詢 dns 的時間)
  - -o : 顯示活動的 TCP 連線並且包含每個連線程序的 ID 編號(PID).你能夠找到應用程式的程序的PID資訊，在 windows 的工作管理員。這個參數通常和 -a, -n, and -p 混合使用.
  - -p 通訊協定 : 顯示指連線的通訊協定.預設的狀況這個通訊協定包含 tcp, udp, tcpv6, or udpv6. 如果配合 -s 參數則是可以顯示統計數量。
  - -s : 顯示統計資訊。預設顯示 TCP, UDP, ICMP, and IP 通訊協定. 如果 IPv6 protocol for Windows XP 被安裝的話, 統計資料顯示 TCP over IPv6, UDP over IPv6, ICMPv6, and IPv6 protocols.
  - -r : 顯示 IP 路由表的內容. 相當於 route print 命令.

- 範例: 
  ```
  查看80被誰占用(findstr-類似linux的grep)
  netstat -ano | findstr 0.0:80

  查看該pid是哪個程式
  tasklist | findstr 1860
  ```