---
---

# Abstract

SSL Certificate ở đây là các certificate dùng để chứng thực domain này là chính chủ. Certificate này bao gồm
- Tổ chức chứng thực certificate này.
- domain và subdomain
- Tổ chức sở hữu domain
- Thời gian bắt đầu cũng như kết thúc của cert
- Public key. Key này được client sử dụng để __encrypt__ 1 key ngẫu nhiên vàu gửi lại cho server. Phía server sẽ decrypt bằng private key của mình, và từ đó giao tiếp giữa client và server sẽ được mã hóa bởi key mà client vừa tạo -> đảm bảo tính bảo mật. Quá trình này gọi là [[HTTPS|TLS handshake]].
- Digital signature -> dùng để đảm bảo thông tin của certificate không bị thay đổi.

### Certificate fingerprint

Certificate fingerprint là giá trị hash của chính certificate đó. Nó có thể thay thế CA certificate trong các thiết bị có ít tài nguyên.

## Digital Certificate Creation Process

Các bước để tạo ra 1 digital certificate trong thực tế

1.  Tạo cạp private và public key.
2. Từ private key tạo __certificate signing request__ (CSR). CSR bao gồm các thông tin về location, organization, common name (hay FQDN) vaf car public key.
3. Gửi CSR to CA. CA sẽ có trách nhiệm validate các thông tin có được CSR.
4. Nếu quá trình validation thành công, CA sẽ tạo ra 1 certificate và dùng private key của họ để sign cho certificate vừa tạo đó. Certificate này sẽ được gửi về cho người request.

Sau đó register có thể dùng certificate đó để sử dụng cho [[TLS]]/SSL
![[image-9.png]]
Hình trên mô tả quá trình tạo ra certificate khi sử dụng comercial CA

## Certificate type

Có 3 loại certificate, tương ứng với từng số lượng domain mà nó xác thực

- Single domain
- Wildcard
- Multiple domain

Ngoài ra còn có 1 cách chia khác là theo validation levels:
- **Domain Validation:** Loại xác thực này đơn giản nhất và rẻ nhất. Nó chỉ xác thực tính sở hữu đối với domain.
- **Organization Validation:** Đối với loại validatin này, thì CA sẽ liên hệ trực tiếp với tổ chức để xác thực tinh sở hữu
- **Extended Validation:** Mức độ xác thực cao nhất, yêu cầu rất nhiều các điều kiện để có được certificate.

### Certificate encoding và file extensions

Các file certificate thường được encode dưới 2 dạng
- Binary files (.DER)
- ASCII (base64)files (.PEM)

Các file extension thường thấy của các certificate
- `.DER`
- `.PEM`
- `.CRT`
- `.CERT`

> Không có sự ràng buộc giữa encoding và file extension, chỉ có convention giữa chúng.


## Certificate chain

Khi người dùng nhận được certificate, làm sao để người dùng biết certificate đó không phải là hàng fake. Sử dụng digital signature chỉ giúp đảm bảo content của certificate ko được sửa đổi, tuy nhiên không đảm bảo certificate đó bị giả mạo. Để xác thực nội dung của certificate, ta cần đến sự có mặt của các CA (Certificate authority), tức là các tổ chức bên thứ 3, có nhiệm vụ chứng thực cho certificate mà trang web cung cấp. 
Cụ thể hơn
- Kho trang web gửi cho người dùng certificate của họ, đồng thời trang web cũng sẽ gửi 1 CA certificate của bên thứ 3, có nội dung là xác nhận cho certificate kia là hợp lệ.
- Tuy nhiên, để chứng thực cho 1 certificate, ta lại dùng 1 certificate khác -> 😒😒😒???. Để đảm bảo certificate này là hợp lệ  -> lại cần 1 certificate khác. Để chứng thực cho certificate này, lại cần 1 certificate khác ,.... -> Ta cần root CA, đó là CA mà OS tin tưởng tuyệt đối, và coi nó như là certifiacte hợp lệ mà ko cần kiểm tra nữa
- Cách hoạt động này gọi là __trust model__

	> Người dùng sẽ tin tưởng vào ca root được cung cấp bởi các tổ chức chứng nhận có uy tín trên thế giới.

![[certificate-chain.jpg]]

Hình trên minh họa cách certificate chain hoạt động.

- Các root certificate này được setup sẵn khi cài hệ điểu hành, và để có thể chỉnh sửa được những giá trị này, cần có quyền quản trị cao nhất đối với máy tính đó

### Certificate bundle

- CA bundle là __file__ chứa cả root và intermediate certificates. Nó chính là các certificate cuối cùng trong chain.

### Root certificate in Linux

Mỗi [[Operating System|hệ điều hành]] hay browser đều có 1 list các root certificate mà họ tin tưởng. Các certificate này thường lưu trong 1 file. File này có thể lưu nhiều root certificate nên còn được gọi là: `Root CA Bundle`

Thông thường trên các nền tảng [[Linux]], các root certificate được lưu tại `/etc/ssl/certificates`. Tùy mỗi distro mà các certificate có thể được lữu trong cùng 1 file hay nhiều file, hoặc cũng có thể là link tới file certificate gốc ở thư mục khác,...

![[linux_ssl_dir.jpg]]

Theo hình trên có thể nhận ra:
- fedora based sẽ lưu các certificate trong cùng 1 file là: `ca-bundle.crt`. File `ca-bundle.trust.crt` là file chứa riêng các extended validation certificate, tức là nó chứa các certificate có mức độ tin tưởng cao hơn.
- Trên Ubuntu, các certificate được lưu ra từng file riêng lẻ. và hầu hết các file này là link từ `/usr/share/ca-certificates`

## Self-signed certificate

Self-singed certificate là các certificate được signed bởi key chính là key tạo ra certificate đó -> CA và register dùng chung private key.
Tuy nhiên có thể tự tạo 1 CA với private key riêng, mặc dù nó cũng không khác biệt nhiều lắm do browser vẫn không tin tưởng các certificate này. Các certificate này được tạo ra nhằm mục đích development hoặc tìm hiểm cách thức hoạt động của [[SSL]]. Các certificate này có thể được tạo bởi [[OpenSSL]]

Quy trình tạo ra self-signed certificate.

![[image-8.png]]

# Keyless Certificate

Đây là 1 kỹ thuật mới, nhằm mục đích giảm thiểu việc sử dụng private certificate -> giảm surface attack

# References

1. [SSL and SSL Certificates Explained For Beginners (steves-internet-guide.com)](http://www.steves-internet-guide.com/ssl-certificates-explained/)
2. [Certificate trong Https (viblo.asia)](https://viblo.asia/p/certificate-trong-https-gAm5yx7Vldb)
3. [How does keyless SSL work? | Forward secrecy | Cloudflare](https://www.cloudflare.com/learning/ssl/keyless-ssl/)
4. [Self-signed certificate - Wikipedia](https://en.wikipedia.org/wiki/Self-signed_certificate)
5. [SSL/TLS beginner’s tutorial. This is a beginner’s overview of how… | by German Eduardo Jaber De Lima | Talpor | Medium](https://medium.com/talpor/ssl-tls-authentication-explained-86f00064280)
6. [How To Create Self-Signed Certificates Using OpenSSL (devopscube.com)](https://devopscube.com/create-self-signed-certificates-openssl/)