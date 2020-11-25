# JSON WEB TOKEN
## JWT là gì ?
- nó là phương tiện đại diện cho các yêu cầu chuyển giao giữa hai bên client - server, các thông tin trong chuỗi JWT được định dạng bằng json. Trong chuỗi token phải có 3 phần là Header, payload, signature được ngăn bằng dấu chấm.

![picture](https://topdev.vn/blog/wp-content/uploads/2017/12/jwt-la-gi.jpeg)

##  Cấu trúc?
- Header.payload.signature
1. Header.
2. Payload.
3. Signature.
#### Header 
- Chứa kiểu dữ liệu, và thuật toán sử dụng để mã hóa ra chuỗi JWT
Vd :
>  {<br>
    "typ": "JWT",<br>
    "alg": "HS256"<br>
}. <br>"typ" chỉ ra đối tượng là một JWT,<br> "alg"(algorithm)
xác định mã thuật toán cho chuỗi là HS256.
#### Payload
- Chứa thông tin mình muốn đặt trong chuỗi token như username, userId, author, ... 
Vd : 
> {<br>
    "user_name": "admin",<br>
    "user_id": "1513717410",<br>
    "authorities": "ADMIN_USER",<br>
    "jti": "474cb37f-2c9c-44e4-8f5c-1ea5e4cc4d18" <br>
}
#### Signature
- Phần xử lí này sẽ được tạo ra bằng cách mã hóa header và payload kèm theo chuỗi secret . Ví dụ : 
> data = base64urlEncode( header ) + "." + base64urlEncode( payload )
signature = Hash( data, secret );
- base64urlEncode : thuật toán mã hóa header và payload<br>

đoạn code trên mã hóa header và payload bằng thuật toán base64urlEncode ta có : 
> // header<br>
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9<br>
// payload<br>
eyJhdWQiOlsidGVzdGp3dHJlc291cmNlaWQiXSwidXNlcl9uYW1lIjoiYWRtaW4iLCJzY29wZSI6WyJyZWFkIiwid3JpdGUiXSwiZXhwIjoxNTEzNzE

sau đó ta có chuỗi secret kèm theo bằng thuật toán HS256 :
> 9nRhBWiRoryc8fV5xRpTmw9iyJ6EM7WTGTjvCM1e36Q

kết hợp 3 chuỗi trên ta được một chuỗi JWT hoàn chỉnh : 
> eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhdWQiOlsidGVzdGp3dHJlc291cmNlaWQiXSwidXNlcl9uYW1lIjoiYWRtaW4iLCJzY29wZSI6WyJyZWFkIiwid3JpdGUiXSwiZXhwIjoxNTEzNzE.9nRhBWiRoryc8fV5xRpTmw9iyJ6EM7WTGTjvCM1e36Q

## Khi nào nên dùng JSON WEB TOKEN?
- Authentication : Khi người dùng đăng nhập vào hệ thống thì những request tiếp theo từ phía người dùng sẽ chứa thêm mã JWT. Cho phép người dùng truy cập vào url, service, resource mà JWT đó cho phép. Không bị ảnh hưởng bỏi CORS do nó không sử dụng cookies.
- Trao đổi thông tin : 1 cách khá hay để truyền thông tin an toàn giữa các thành viên với nhau, nhờ vào phần signature. Phía người nhận có thể biết được người gửi thông tin là ai thông qua phần Signature. Chữ kí được tạo ta thông qua kết hợp giữa header và payload lại nên thông qua việc đó ta có thể xác nhận được chữ kí có bị giả mạo hay không.
