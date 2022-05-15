# Token và Cookie

## Lý do cần dùng Token hoặc Cookie
- Http là stateless, tức  là mỗi request sẽ phải đăng nhập lại
- Để khắc phục => Client cần lưu lại thông tin của session
- Có 2 cách phổ biến là Token và Cookie

## Bảng so sánh
Token | Cookie
:-----|-------:
Stateless | Stateful
Using with CORS -> easy to expose API to different services and domains |Cookie is small -> efficient to store on client side
The data is stored in the JWT, meaning it can contain any type of data giving the flexibility on what information is to be included in the token |Cookies can be Http-only -> making them impossible to read on the client-side. This improves protection against any Cross-site scripting (XSS) attacks
It improves overall system performance |Cookies will be added to the request automatically, so the developer will not have to implement them manually and therefore requires less code
They are easier to maintain and scale horizontally in the distributed systems |It is vulnerable to Cross-site request forgery attack. It often needs other security measures such as CSRF tokens for protection.
Storing a lot of data in the token makes it huge, which slows the requests |You need to store the session data in a database or keep it in memory on the server. This makes it less scalable and increases overhead when the site is having many users.
Tokens cannot be used to authenticate a user in the background on the server since no session exists on the database |Cookies normally work on a single domain. For example, it is impossible to read or send a cookie from a domain like jabs.com to a boo.com domain. This is an issue when the API service is from different domains for mobile and web platforms.
Token storage on the client-side is problematic. When the token is stored in the cookie, they are less efficient when the JWT size is large. You can store the token in the session storage, but it’s cleared when the browser is closed. In the local storage, the JWT will be bound to a specific domain |The client needs to send a cookie on every request, even with the URLs that do not need authentication for access
Token in the client-side might be hijacked by an attacker making it vulnerable to Cross-site scripting (XSS) attacks. This occurs when an outside entity can execute code within your domain by embedding malicious JavaScript on the page to read and compromise the contents of the browser storage | 


Choose Token when:
	- When there is a need to use different domains on the system. For example, if you are on a section.com domain that wants to send an authenticated request to the github.com domain, using tokens becomes handy. This is useful in building distributed systems, particularly microservices on the cloud, where servers are on different domains, yet there is a need for authentication across them
	- Tokens will be useful when the API is being used by different platforms such as the web, IoT, and mobile devices

Choose Cookies when:
	- When the user profile needs personalization. When we need user preferences such as themes, we would use a cookie session in the database. Cookies are also used to help build targeted ads for different users.
	- If the site needs to track the user by analyzing and recording user behavior while navigating on the site. An example in shopping sites is the list of items a user recently viewed.
	- Sessions regarding logins, shopping carts, and game scores may need tracking and saving in a database. Without cookies, you will need to log in every time you leave a site or rebuild your shopping cart if a page is closed.

- Token-based gồm ACESS_TOKEN và REFRESH_TOKEN
	- ACCESS_TOKEN  -> tương đương với session -> không cần đăng nhập lại nhiều lần
	- REFRESH_TOKEN -> Dùng để tạo mới ACCESS_TOKEN khi expired. REFRESH_TOKEN sẽ tương tác với service auth, còn ACCESS_TOKEN sẽ dùng cho các service truy cập vào resource
	- REFRESH_TOKEN cần cho  mục đích security.
- Cookie gồm:
	- Cookie lưu ở client side
	- DB chứa session ở server side
	- Mỗi lần client gửi request -> gửi kèm cookie -> Server sẽ lookup để validate user


###### References
+ [Cookie vs Token authentication | Engineering Education (EngEd) Program | Section](https://www.section.io/engineering-education/cookie-vs-token-authentication/)