---
layout: post
title: Bảo vệ API bằng cách sử dụng OAuth 2.0 trong Azure AD và API Management

tags: [ azure, API Management, Azure Active Directory, OAuth 2.0]
comments: true
---
## API Management là gì?

Developer Console là gì?

## Tổng quan

### Đăng ký một application trong Azure AD cung cấp API (backend app)

Để bảo vệ một API bằng Azure AD, trước tiên hãy đăng ký một ứng dụng trong Azure AD đại diện cho API và thực hiện expose API.

Đầu tiên cần truy cập Azure portal, search App Registration và tạo mới một registration với Name và Supported Account Types thích hợp chẳng hạn như Name là backend-app và Supported Account Types (Người có thể sử dụng ứng dụng hoặc truy cập API này) là các tài khoản trong một tenant hoặc nhiều tenant (multitenant).

Để trống Redirect URI sẽ được cấu hình sau.
Đây là URI sẽ được trả về sau khi xác thực thành công người dùng. Redirect URI là optional tuy nhiên với các Scenario yêu cầu xác thực nó thường là yêu cầu bắt buộc.

Đăng ký registration và thực hiện Expose API bằng cách truy cập vào App Registration instance và chọn Expose an API ở panel bên trái. Tại đây chúng ta set giá trị Application ID URL với giá trị mặc định chẳng hạn Application (client) ID của registration ở trang Overview là  "7d8c2f24-8ae1-4f3b-b701-5ac6361b9cb0" thì Application ID URL sẽ là "api://
7d8c2f24-8ae1-4f3b-b701-5ac6361b9cb0"

Tiếp đến chúng ta có thể thêm scope để hạn chế quyền truy cập vào dữ liệu cũng như chức năng được bảo vệ bởi API. 


### Đăng ký một application khác trong Azure AD tiêu thụ API (client app)

Bước thứ hai, chúng ta cần đăng ký thêm một ứng dụng nữa đại diện cho bên tiêu thụ API hay client app. Mọi client app thực hiện gọi tới API đều cần được đăng ký như một application trong Azure AD thông qua Azure AD. Trong ví dụ này client app sẽ là Developer Console trong API-M developer portal.

Sau khi đăng ký thành công client app, chúng ta cần thực hiện thêm một client secret trong App Registrion instance mục đích để mỗi lần request token, ứng dụng sẽ sử dụng secret này để chứng minh định danh (identity) tương tự như một mật khẩu của ứng dụng. Tại panel bên trái, chọn Certificates & secrets sau đó cung cấp description và expires cho client secret đó.

### Cấp quyền cho client app có thể gọi tới API của backend app

Bước thứ ba, sau khi đã đăng ký thành công 2 ứng dụng đại diện cho API và client app (Developer Console) chúng ta cần thực hiện cấp quyền cho client app có thể gọi tới backend app.

Từ 

### Bật xác thực người dùng OAuth 2.0 trong Developer Console

### Gọi API từ developer portal

### Cấu hình một JWT validation policy để pre-authorize các requests





