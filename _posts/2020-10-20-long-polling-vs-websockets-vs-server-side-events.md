---
layout: post
title: Long-Polling vs WebSockets vs Server-Sent Events

tags: [Long-Polling, WebSockets, Server-Sent Events]
comments: true
---
## Long-Polling vs WebSockets vs Server-Sent Events
Long-Polling, WebSockets và Server-Sent Events là các giao thức giao tiếp phổ biến giữa một client như trình duyệt và server. Tuy nhiên giữa chúng có khá nhiều điểm khác biệt cần được khai thác để thiết kế, phát triển và xây dựng một hệ thống có sự giao tiếp giữa client và server. Dưới đây là giải thích về từng giao thức trên và cách chúng được sử dụng như thế nào trong thiết kế hệ thống.

Đầu tiên, hãy bắt đầu với việc hiểu một HTTP request trông như thế nào. Sau đây là một chuỗi các sự kiện cho HTTP request

1. Client sẽ mở một kết nối (connection) và yêu cầu dữ liệu từ server
2. Server xử lý và trả về response
3. Server gửi response ngược trở lại cho client

<br>

![CAPTheoremSystemDesign](../images/long-polling-websockets-see/http-request.svg){:class="img-responsive"}

### Ajax Polling
Polling là một kỹ thuật tiêu chuẩn được sử dụng bởi đại đa số các ứng dụng AJAX. Ý tưởng cơ bản là máy khách liên tục Polls (hoặc yêu cầu) máy chủ để lấy dữ liệu. Máy khách đưa ra yêu cầu và đợi máy chủ phản hồi với dữ liệu. Nếu không có sẵn dữ liệu, một phản hồi trống được trả về.

1. Máy khách mở kết nối và yêu cầu dữ liệu từ máy chủ thông qua HTTP. 
2. Trang web được yêu cầu sẽ gửi yêu cầu đến máy chủ theo định kỳ (ví dụ: 0,5 giây). 
3. Máy chủ xử lý và trả về phản hồi và gửi lại phản hồi cho máy client.
4. Máy khách lặp lại ba bước trên theo định kỳ để nhận các bản cập nhật từ máy chủ.

![ajax-polling](../images/long-polling-websockets-see/ajax-polling.svg){:class="img-responsive"}

### HTTP Long-Polling
Đây là một biến thể của kỹ thuật polling truyền thống cho phép máy chủ đẩy thông tin đến máy khách bất cứ khi nào có dữ liệu. Với Long-Polling, khách hàng yêu cầu thông tin từ máy chủ chính xác như trong polling thông thường, nhưng với kỳ vọng rằng máy chủ có thể không phản hồi ngay lập tức. Đó là lý do tại sao kỹ thuật này đôi khi được gọi là "Hanging GET".

* Nếu máy chủ không có sẵn bất kỳ dữ liệu nào cho máy khách, thay vì gửi một phản hồi trống, máy chủ sẽ giữ yêu cầu và đợi cho đến khi một số dữ liệu có sẵn. 
* Khi dữ liệu có sẵn, một phản hồi đầy đủ sẽ được gửi đến máy khách. Sau đó, máy khách ngay lập tức yêu cầu lại thông tin từ máy chủ để máy chủ hầu như luôn có sẵn yêu cầu chờ sẵn mà nó có thể sử dụng để cung cấp dữ liệu đáp ứng một sự kiện

Vòng đời cơ bản của một ứng dụng sử dụng HTTP Long-Polling như sau

1. Máy khách thực hiện yêu cầu ban đầu thông qua HTTP và sau đó chờ phản hồi. 
2. Máy chủ trì hoãn phản hồi cho đến khi có bản cập nhật hoặc hết thời gian chờ đã xảy ra. 
3. Khi có bản cập nhật, máy chủ sẽ gửi phản hồi đầy đủ đến máy khách. 
4. Khách hàng thường gửi một yêu cầu Long-Polling mới, ngay sau khi nhận được phản hồi hoặc sau khi tạm dừng để cho phép một khoảng thời gian chờ có thể chấp nhận được. 
5. Mỗi yêu cầu Long-Polling đều có thời gian chờ. Máy khách phải kết nối lại định kỳ sau khi kết nối bị đóng do hết thời gian.

![Long-polling](../images/long-polling-websockets-see/long-polling.svg)
{:class="img-responsive"}

### WebSockets

WebSocket cung cấp các kênh giao tiếp song công đầy đủ (full-duplex) qua một kết nối TCP duy nhất. Nó cung cấp một kết nối liên tục giữa máy khách và máy chủ mà cả hai bên có thể sử dụng để bắt đầu gửi dữ liệu bất kỳ lúc nào. Máy khách thiết lập kết nối WebSocket thông qua một quá trình được gọi là bắt tay WebSocket. Nếu quá trình thành công, thì máy chủ và máy khách có thể trao đổi dữ liệu theo cả hai hướng bất kỳ lúc nào. Giao thức WebSocket cho phép giao tiếp giữa máy khách và máy chủ với chi phí thấp hơn, tạo điều kiện truyền dữ liệu theo thời gian thực. Điều này có thể thực hiện được bằng cách cung cấp một cách thức tiêu chuẩn hóa để máy chủ có thể gửi nội dung đến trình duyệt mà không cần máy khách yêu cầu và cho phép thông báo được truyền qua lại trong khi vẫn giữ kết nối mở. Bằng cách này, một cuộc trò chuyện đang diễn ra hai chiều có thể diễn ra giữa máy khách và máy chủ
![Websocket](../images/long-polling-websockets-see/websocket.svg)
{:class="img-responsive"}

### Server-Sent Events (SSEs)
Trong SSE, máy khách thiết lập một kết nối liên tục và lâu dài với máy chủ. Máy chủ sử dụng kết nối này để gửi dữ liệu đến máy khách. Nếu máy khách muốn gửi dữ liệu đến máy chủ, nó sẽ yêu cầu sử dụng công nghệ và giao thức khác để làm như vậy

1. Máy khách yêu cầu dữ liệu từ máy chủ thông qua HTTP. 
2. Trang web mở một kết nối với máy chủ. 
3. Máy chủ gửi dữ liệu đến máy khách bất cứ khi nào có thông tin mới.

SSE là tốt nhất khi chúng ta cần lưu lượng truy cập thời gian thực từ máy chủ đến máy khách hoặc nếu máy chủ đang tạo dữ liệu theo vòng lặp và sẽ gửi nhiều sự kiện đến máy khách.

