#### DNS
- /etc/hosts ：這個是最早的 hostname 對應 IP 的檔案；
- /etc/resolv.conf ：這個重要！就是 ISP 的 DNS 伺服器 IP 記錄處；
- /etc/nsswitch.conf：這個檔案則是在『決定』先要使用 /etc/hosts 還是 /etc/resolv.conf 的設定