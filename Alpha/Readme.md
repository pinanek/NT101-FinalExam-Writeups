# Alpha Machine (Linux)

## Local

- Scan machine với nmap, có được các port 22, 80, 443, 8080

  ![nmap.png](images/nmap.png)

- Kiểm tra OpenSSH với `searchsploit`, không có phát hiện gì

  ![searchsploit-openssh.png](images/searchsploit-openssh.png)

- Vậy thì đầu tiên, sẽ kiểm tra port 80 với `dirsearch`, phát hiện ra 1 file config `sftp-config.json`

  ![dirsearch-http.png](images/dirsearch-http.png)

- Truy cập vào file `sftp-config.json`, có được username và password nhưng chưa rõ là ở đâu

  ```
  username: admin
  password: password@123
  ```

  ![sftp-config-json.png](images/sftp-config-json.png)

  ![crack-password.png](images/crack-password.png)

- Sau khi truy cập http trên web browser thì không có gì, truy cập vào https, ta có thể thử username và password

  ![https-login.png](images/https-login.png)

- Đăng nhập thành công, có được local flag

  ![local-flag.png](images/local-flag.png)

> Flag: Wanna.One{@n_kh0ng_du@c_th1_01_r@_ph@i_kh0ng}

## Proof

- Kiểm tra xem các users, commands có quyền sử dụng `sudo`, thấy rằng `/usr/bin/less` và `/etc/profile` không cần password để có thể sử dụng `sudo`

  ![sudo.png](images/sudo.png)

- Vậy sẽ mở file `/etc/profile` để có quyền thực thi câu lệnh dưới quyền root với câu lệnh

  ```console
  sudo /usr/bin/less nano /etc/profile
  ```

- Sau đó trong `nano`, thực hiện câu lệnh find để tìm file `proof.*` với câu lệnh

  ```
  find / -type f name "proof.*"
  ```

  ![find-proof.png](images/find-proof.png)

- Có được proof flag

  ![proof-flag.png](images/proof-flag.png)

> Flag: Wanna.One{nqu0j_cu4_c0nq_ckunq_pk4j_dj_v40_10nq_nqu0j,_cku_dunq_dj_v40_10nq_d4t}