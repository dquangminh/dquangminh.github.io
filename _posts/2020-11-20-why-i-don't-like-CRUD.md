---
layout: post
title: Why I dont't like CRUD

tags: [CRUD]
comments: true
---
### CRUD
Khi xây dựng các API, các mô hình thường cung cấp bốn loại chức năng cơ bản đó là Create, Read, Update, và Delete tài nguyên. Các nhà khoa học máy tính thường gọi tên các các chức năng này bằng từ viết tắt CRUD.

CRUD paradigm rất phổ biến trong việc xây dựng các ứng dụng web, bởi vì nó cung cấp một framework để nhắc nhở các developer về cách xây dựng các mô hình đầy đủ và hữu ích.

### Why I dont't like CRUD
Bản chất CRUD paradigm không hề xấu khi đóng vai trò như một framework để nhắc nhở các developer về cách xây dựng các mô hình đầy đủ và hữu ích.

Lý do cho phản ứng của tôi xuất phát từ cách tôi thấy các nhà phát triển sử dụng Datasets sai cách.

Ở đó, CRUD được sử dụng để làm việc với các thực thể (entities) và thay đổi các thực thể đó bằng cách truy xuất dữ liệu từ một kho dữ liệu (data store), thực hiện sửa đổi dữ liệu bằng cách đặt lại các thuộc tính và sau đó gửi dữ liệu trở lại dịch vụ để cập nhật trạng thái mới nhất của dữ liệu với một giá trị mới vào kho dữ liệu sử dụng transactions và lock data.

Kể từ giờ khi sử dụng dùng cụm từ CRUD ý tôi muốn đề cập đến là cách bạn cung cấp giao diện dịch vụ thông qua CRUD.

Nhà phát triển như vậy sẽ viết một dịch vụ lấy cho bạn một Dataset, giao diện người dùng sau đó sẽ sửa đổi Dataset và gửi nó trở lại. Dịch vụ hoặc thậm chí là cả stored-proceduce để kiểm tra dữ liệu (chẳng hạn như timestamp hay version). Kết quả của việc này dẫn đến những hạn chế của CRUD bị bộc lộ trong khá nhiều kiến trúc đó là
* Hệ thống CRUD thực hiện các hoạt động cập nhật trực tiếp đối với kho dữ liệu, điều này có thể làm chậm hiệu suất và khả năng phản hồi, đồng thời hạn chế khả năng mở rộng do chi phí xử lý mà nó yêu cầu.
* Khả năng xảy ra xung đột cập nhật dữ liệu diễn ra thường xuyên hơn vì các hoạt động cập nhật diễn ra trên một mục dữ liệu.

Đa số các các dịch vụ xử lý đơn đặt hàng (order processing) thường không phải là CRUD. Một đơn đặt hàng có thể được tạo ngoại tuyến và sau đó được gửi (sao chép nếu bạn muốn) đến một dịch vụ để xử lý. 

Việc xử lý đơn đặt hàng đó sẽ ảnh hưởng đến nhiều thực thể có liên quan. Thông tin khách hàng có thể được cập nhật, các thuộc tính được sử dụng để tính toán giá và chiết khấu có thể được cập nhật vì khách hàng đã đạt đến khối lượng đơn đặt hàng quan trọng và được nâng cấp, sản phẩm có thể có hoặc không, ...

Lúc này, nếu sử dụng CRUD có nghĩa là các thực thể được đọc, tạo, cập nhật và xóa trực tiếp trong kho dữ liệu. Một đơn đặt hàng sẽ là một yêu cầu phức tạp cho một business transaction. Và bạn sẽ phải đối mặt với một loạt những cơn đau đầu mới

* Bỏ sót bối cảnh thay đổi (ví dụ: thông tin liên hệ đã thay đổi do liên hệ có chức năng khác trong tổ chức hoặc thông tin liên hệ đã thay đổi do bộ phận được đổi tên)
* Những phụ thuộc ngầm mà bạn có thể không muốn (ví dụ: hệ thống bán hàng đã từ chối đơn đặt hàng vì giá không còn như cũ nữa, mặc dù nó đã được giảm)
* Bỏ qua các phụ thuộc bạn có thể cần (ví dụ: chỉ đặt hàng nếu thời gian giao hàng dưới 10 ngày làm việc)

### When CRUD
Như đã nói t không thích CRUD vì cách tôi thấy các nhà phát triển sử dụng Datasets sai cách. Kiến trúc và thiết kế là tất cả nhằm tạo ra sự đánh đổi nhằm tạo ra sự phù hợp nhất khi xem xét tổng thể. Vì vậy CRUD sẽ là phù hợp nhất nếu nó gặp những điều kiện

* Cập nhật hiếm khi hoặc chỉ được thực hiện bởi một nhóm nhỏ người dùng.
* Nếu cập nhật được thực hiện bằng cách bổ sung thông tin (phương pháp compensate) thay vì thay đổi thông tin hiện có thì không có nhiều vấn đề. 
* Xung đột do update conflict xảy ra ít ví dụ thông tin liên hệ trong hệ thống CRM sẽ không được thay đổi thường xuyên. Các cuộc hẹn trong lịch của tôi thường chỉ do tôi thay đổi. Ngay cả khi quản trị viên của chúng tôi hoàn toàn có quyền thay đổi lịch của tôi, cơ hội để cả hai chúng tôi thay đổi cùng một cuộc hẹn là rất mong manh, vì thông lệ kinh doanh phổ biến là cô ấy không thay đổi lịch của tôi trừ khi cô ấy phải làm vậy. Trong nhiều trường hợp, các vấn đề đồng thời được bảo vệ bởi thực tiễn kinh doanh hoặc thói quen, chính sách và quy trình hơn là bởi phần mềm.

### Các cách tiếp cận tương tự để thay thế CRUD
Khi làm việc với các bài toán liên quan tới xử lý đơn hàng như trên, hoặc bài toán yêu cầu khả năng về hiệu năng cao, khả năng phản hồi và khả năng mở rộng thì chúng ta có thể xem xét các cách tiếp cận khác như

* Command and Query Responsibility Segregation (CQRS) để phân tách các thao tác đọc và cập nhật cho một kho dữ liệu. Việc triển khai CQRS trong ứng dụng của bạn có thể tối đa hóa hiệu suất, khả năng mở rộng và bảo mật của nó
* Event Sourcing sử dụng kho lưu trữ chỉ phần phụ để ghi lại toàn bộ chuỗi hành động được thực hiện trên dữ liệu đó. Lưu trữ hoạt động như một hệ thống hồ sơ và có thể được sử dụng để hiện thực hóa các đối tượng miền. Điều này có thể đơn giản hóa các tác vụ trong các miền phức tạp, bằng cách tránh sự cần thiết phải đồng bộ hóa mô hình dữ liệu và miền kinh doanh, đồng thời cải thiện hiệu suất, khả năng mở rộng và khả năng đáp ứng.

Ngoài ra còn khá nhiều cách tiếp cận khác phù hợp với những bài toán cụ thể. Không có cách tiếp cận nào có thể phù hợp với mọi bài toán (there is no one-size-fits-all), CQRS, Event Sourcing hay CRUD đều chỉ phù hợp với khi gặp những điều kiện của nó. Do đó cần xác định thật rõ business bạn đang xây dựng, các yêu cầu phi chức năng về hiệu năng, khả năng mở rộng, chi phí trước khi lựa chọn một thiết kế và kiến trúc phù hợp.



