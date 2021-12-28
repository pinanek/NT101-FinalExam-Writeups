# Kevin Machine (Windows)

## Local

- Scan machine với nmap, có được các port, theo hint của đề thì sẽ kiểm tra port 53, ngoài ra còn port 80, ta sẽ kiểm tra sau

  ![nmap.png](images/nmap.png)

- Thêm domain `cnsc.local` vào file `/etc/hosts`, Kiểm tra zone transfer với `dig`, có được các subdomain `kevin.cnsc.local` và `bl4kduk.cnsc.local`, sau đó thêm các subdomain này vào `/etc/hosts`

  ![dig-axfr.png](images/dig-axfr.png)

  ![nano-hosts.png](images/nano-hosts.png)

- `dirsearch` với subdomain `kevin.cnsc.local`, không có gì đặc biệt

  ![dirsearch-kevin.png](images/dirsearch-kevin.png)

- `dirsearch` với subdomain `bl4kduk.cnsc.local`, phát hiện 1 file config `/assets/sftp-config.json`, ngoài ra còn 2 folder `assets` và `tiki`, sẽ kiểm tra sau

  ![dirsearch-bl4kduk.png](images/dirsearch-bl4kduk.png)

- Truy cập vào file `sftp-config.json`, có được username và password nhưng chưa rõ là ở đâu

  ```
  username: steve
  password: M@t1<h4uCuaSt3vEdaynha@192!
  ```

  ![sftp-config-json.png](images/sftp-config-json.png)

- Truy cập vào folder `assets` không thành công, truy cập vào folder `tiki` thì thấy có sử dụng login, thử đăng nhập bằng tài khoản được tìm thấy và thành công

  ![login-tiki.png](images/login-tiki.png)

  ![login-tiki-success.png](images/login-tiki-success.png)

- Quét folder `tiki` với tài khoản chứng thực trên, thấy có file `CHANGELOG.TXT`, ta có thể tìm thấy version của `tiki` ở đây, và version là `15.1`

  ![dirseach-tiki.png](images/dirseach-tiki.png)

  ![tiki-version.png](images/tiki-version.png)

- Kiểm tra với `searchsploit`, thấy rằng ta có thể upload file lên tiki

  ![searchploit-tiki.png](images/searchploit-tiki.png)

- Tìm và thấy được cách để exploit machine local: https://github.com/ivanitlearning/Tiki-Wiki-15.1-unrestricted-file-upload/blob/master/tikiwiki_15.1_RCE.py

  - Đầu tiên ta sẽ truy cập vào file 

  ![upload-url.png](images/upload-url.png)

  ![upload-url-success.png](images/upload-url-success.png)

- Kiểm tra rằng là có thực thi các câu lệnh được không, và có thể thực hiện được, code: https://www.php.net/manual/en/function.exec.php#refsect1-function.exec-examples

  ![check-command.png](images/check-command.png)

  ![check-command-success.png](images/check-command-success.png)


- Vậy ta sẽ tìm file local.txt, như hình trên thì đang ở ổ `C:`, ta sẽ trở về root tại `C:` và tìm với câu lệnh:

  ```console
  cd \ && dir /s local.txt
  ```

  ![find-command.png](images/find-command.png)

  ![find-command-success.png](images/find-command-success.png)

- Sau đó có được flag tại `C:\Users\steve\Desktop\local.txt` bằng câu lệnh:

  ```console
  type C:\Users\steve\Desktop\local.txt
  ```

  ![cat-command.png](images/cat-command.png)

  ![local-flag.png](images/local-flag.png)

> Flag: Wanna.One{c0_b@nh_m1_th0i_h@}