---
layout: post
title: Good Software Development

tags: [Good Software Development]
comments: true
---
### Good Software Development
Làm thế nào để phát triển phần mềm chất lượng?

### Making software development as simple as possible, but not simpler
Đừng cố gắng xây dựng ứng dụng phức tạp để giải quyết nhiều vấn đề nhất có thể mà hãy đảm bảo ứng dụng của bạn có thể giải quyết từng vấn đề của mọi người một cách tốt nhất có thể.

Xây dựng phần mềm chất lượng đòi hỏi phải tập trung bắt đầu với những giải pháp đơn giản nhất có thể để giải quyết vấn đề và tập trung vào những vẫn đề cốt lõi nhất. Hãy cố gắng xây dựng một ứng dụng đơn giản nhưng ít gặp vấn đề khi thêm các chức năng cần thiết về sau. Một hệ thống CNTT lớn hoạt động kém hiệu quả thường không thể đơn giản hóa và dễ dàng sửa chữa khiến nó ngày càng trở nên tồi tệ hơn.

Ngay cả những ứng dụng supper application (những ứng dụng tích hợp rất nhiều dịch vụ trở thành siêu ứng dụng) thành công như Grab, Momo, và Zalo cũng bắt đầu với chức năng rất cụ thể, cốt lỗi và chỉ được bổ sung thêm chức năng sau khi chúng đã đảm bảo vị trí của mình với khả năng mở rộng dịch vụ để đáp ứng nhu cầu ngày càng tăng cao từ người dùng. 

Tuy nhiên, chúng ta thường ít khi nhận ra thất bại của ứng dụng phần mềm khi chúng quá nhỏ; chúng ta gặp thất bại khi chúng dần phát triển. Điều đó khiến chúng ta thường lờ đi những ảnh hưởng tiêu cực một khi nó phát triển đủ lớn để tập trung phát triển quá nhiều tính năng. Khi ứng dụng đủ lớn cơn ác mộng ập đến và chúng ta không có sự chuẩn bị tốt cũng như khả năng để giải quyết những bài toán ngày càng phức tạp theo thời gian dẫn đến sự thật bại của ứng dụng phần mềm.

Và thật không may, giữ cho một dự án tập trung để hoàn thiện các tính năng cốt lỗi và khả năng mở rộng là rất khó trong thực tế: chỉ cần thu thập các yêu cầu từ tất cả các stakeholders là đã tạo ra một danh sách lớn các tính năng.
Việc hoàn thiện chức năng chắc chắn là yêu cầu cấp thiết cần giải quyết sớm để kịp thu hút người dùng và giải quyết vấn đề của họ. Tuy nhiên không phải bất kỳ tính năng nào cũng nên được xem xét giải quyết ưu tiên sớm, những tính năng quan trọng khi dự án đã thành hình thường không quá nhiều và gấp gáp thay vào đó là nhưng chức năng kém ưu tiên hơn và gây ra sự phức tạp cho hệ thống.

Một cách để quản lý sự cồng kềnh này là sử dụng danh sách ưu tiên. Tất cả các yêu cầu vẫn được thu thập, nhưng mỗi yêu cầu đều được đánh độ ưu tiên tùy theo việc chúng là các tính năng quan trọng tuyệt đối, hay có giá trị cao trong ứng dụng. Cách tiếp cận này cũng cho thấy rõ ràng sự đánh đổi của việc có nhiều tính năng hơn hay dành nỗ lực đó cho các công việc phân tích, thiết kế và cải tiến hệ thống. Các bên liên quan muốn tăng mức độ ưu tiên cho một tính năng cũng phải xem xét những tính năng nào họ sẵn sàng tước bỏ.

Thông thường các yêu cầu trong dự án thường được chia ra làm 2 nhóm đó là functional requirements (những yêu cầu về chức năng liên quan tới nghiệp vụ) và non-functional requirements (những yều cầu về hiệu năng, bảo mật liên quan tới kỹ thuật). Những yêu cầu về chức năng thường được đánh nhãn urgent do nó cung cấp giải pháp tới người dùng còn những yêu cầu phi chức năng thường được đánh nhãn important do nó không thực sự urgent với người dùng nhưng rất quan trọng với ứng dụng để đảm bảo ứng dụng hoạt động hiệu quả.

Dễ thấy, sau khi chia chức năng làm 2 nhóm và đánh độ ưu tiên của nó ta có thể chia nó vào matrix rất nổi tiếng sau

|1. Các chứng năng urgent và important | 2. Các chức năng không urgent và important|
|---|---|
|3. Các chứng năng urgent nhưng không important | 4. Các chức năng không urgent và không important|

Nhìn vào đây có thể thấy thứ tự ưu tiên cần thực hiện đó là 1->2->3->4. Tuy nhiên, sai lầm ở đây đó là PM, PO không đánh giá chính xác được những chức năng ở vùng 1 (vốn không quá nhiều) và 3 (có vẻ nhiều hơn) hoặc có thể là họ không cần đánh giá và thực hiện gộp chung vùng 1 và 3, dẫn đến sự quá tải khi phải làm những chức năng vùng 1 do số lượng quá nhiều dẫn đến tăng chi phí và sự phức tạp trước khi thực hiện chức năng ở vùng 2 dẫn đến những yêu cầu về hiệu năng và bảo mật không được đáp ứng kịp thời.

Khi làm dự án tôi thường thích được làm cùng với những PM say "NO" và TL say "YES". Để làm hài lòng KH hoặc người dùng thường có nhiều PM luôn say "YES" với mọi vấn đề cần giải quyết sớm thay vì phải phân tích và say "NO" với những yêu cầu không quá cần thiết, điều này thường mang tới kết quả ngay lập tức đối với họ khi sự hài lòng và kỳ vọng của KH càng lên cao. Tuy nhiên, càng về sau nó càng bộc lộ những điểm chết chóc khi dự án phát triển và trở lên phức tạp, ứng dụng nhiều bugs hơn, hiệu năng kém hơn, deadline chậm hơn và nhiều vấn đề không thể giải quyết nhiều hơn. Do đó những PM say "NO" mà vẫn đạt được sự hài lòng của KH thường làm việc hiệu quả hơn.

Tuy nhiên để PM có thể say "NO" họ cần những TL say "YES", họ sẽ say "NO" với những chức năng không quá quan trọng và tập trung vào những chức năng quan trọng và cấp bách của dự án và cần sự hỗ trợ nhiệt thành từ những TL luôn say "YES" với mọi thử thách từ đó mới tạo nên ấn tượng với KH. Và ngay cả những TL say "YES" cũng luôn cần có những PM say "NO" bên cạnh bởi họ không thể say "YES" và thực hiện quá nhiều thứ trong đó bao gồm cả những thứ không quá quan trọng.

### Tìm ra các vấn đề và tái giải quyết
Trên thực tế, phần mềm hiện đại rất phức tạp và thay đổi nhanh chóng đến mức không có quy hoạch nào có thể loại bỏ tất cả các thiếu sót (Nhiều dự án thực hiện Agile/SM mô hình được công nhận có nhiều ưu điểm với yêu cầu hiện tại song vẫn dẫn đến thất bại).

Để xây dựng phần mềm tốt, trước tiên bạn cần xây dựng phần mềm không tốt như 1 bản nháp khi làm toán, viết nhạc, ngâm thơ, sau đó chủ động tìm ra các vấn đề để cải tiến giải pháp của mình.

Hãy đầu bằng một việc đơn giản như nói chuyện với những người dùng thực tế mà bạn đang cố gắng đưa giải pháp tới họ. Mục đích là để hiểu vấn đề gốc rễ mà bạn muốn giải quyết và tránh chuyển sang một giải pháp chỉ dựa trên những thành kiến ​​đã định trước (những điều bạn cho là hợp lý từ quan điểm cá nhân nhưng bất hợp lý với nhiều người khác) và không ngừng nêu lên quan điểm, góp ý về mọi thứ bởi mọi thứ đưa ra trước khi có sự tham khảo đều là những thành kiến tư duy.

Khi hiểu rõ về giải pháp phù hợp, bạn ngừng khám phá những ý tưởng mới và có thể thu hẹp phạm vi xác định các vấn đề với những triển khai cụ thể để có thể bắt đầu công việc xây dựng sản phẩm thực tế.

Ngay sau khi ra mắt là lúc các vấn đề với một sản phẩm xảy ra bất thường nhất. Một vấn đề chỉ xảy ra 0,1% có thể không được chú ý trong quá trình thử nghiệm. Nhưng một khi bạn có một triệu người dùng, mỗi ngày vấn đề không được giải quyết là bạn phải đối phó với hàng nghìn người dùng đang tỏ ra khó chịu. Bạn cần khắc phục các sự cố do thiết bị di động mới, sự cố mạng hoặc các cuộc tấn công bảo mật gây ra trước khi chúng gây ra tổn hại đáng kể cho người dùng của bạn. hãy xây dựng "hệ thống miễn dịch" theo thời gian cho phép bạn tránh bị choáng ngợp khi các vấn đề mới chắc chắn xuất hiện.

Tuy nhiên có một vấn đề vô cùng lớn ở đây đó là sự sợ hãi thất bại, và tạo ra thay đổi từ con người. Ngay cả khi họ nhận ra những vấn đề nội tại, đôi khi họ vẫn run sợ việc phá bỏ và tạo ra những thay đổi rõ rệt. Các vấn đề thay vì được tái giải quyết để trở nên tốt hơn thì rất nhiều trong số chúng được thỏa hiệp để tồn tại với lý do những ảnh hưởng của nó tới hệ thống. Điều đó tạo ra những khối ung nhọt khó bị loại bỏ trong dự án và việc ở lại của chúng thay vì tránh tạo ra những ảnh hưởng tiêu cực tới chất lương dự án, lại tạo ra những ảnh hưởng tiêu cực với những tính năng và giải pháp được bổ sung trong tương lai.

Những thay đổi, cải tiến không thể được tạo ra từ ý chí của 1 cá nhân riêng lẻ mà nó cần ý chí chung của mọi người để vượt qua những rào cản. Do đó, vì nhiều lý do, nó vẫn tồn tại như gốc rễ vấn đề của vấn đề.

### Thuê những kỹ sư giỏi nhất bạn có thể
"Đừng coi những SE là những bản vá tạm thời và là những mắt xích có khả năng thay thế trong dự án" và coi việc "FULL-TIME" của họ là gắn với 8h trong ngày mà hãy xây dựng những SE FULL-TIME với vòng đời dự án.

* Hãy thuê những kỹ sư giỏi nhất bạn có thể.

Chìa khóa để có kỹ thuật tốt là có kỹ sư giỏi và nhiệt thành với công việc.
Google, Facebook, Amazon, Netflix và Microsoft đều điều hành một số lượng chóng mặt các hệ thống công nghệ lớn nhất trên thế giới. Có một lý do mà mức lương cho ngay cả những sinh viên mới tốt nghiệp đã tăng lên rất nhiều khi các công ty này phát triển (tất nhiễn không phải là vì họ thích cho đi tiền) đó là những kỹ sư giỏi nhất có năng suất cao hơn ít nhất 10 lần so với một kỹ sư bình thường. Điều này không phải do các kỹ sư giỏi viết mã nhanh hơn 10 lần. Mà do họ có khả năng đưa ra quyết định sáng suốt hơn nên tiết kiệm gấp 10 lần công việc.

Một kỹ sư giỏi nắm bắt tốt hơn phần mềm hiện có mà họ có thể sử dụng lại, do đó giảm thiểu các phần của hệ thống mà họ phải xây dựng từ đầu. Họ nắm bắt tốt hơn các công cụ kỹ thuật, tự động hóa hầu hết các khía cạnh thường ngày trong công việc của họ. Tự động hóa cũng có nghĩa là giải phóng con người để giải quyết các lỗi không mong muốn, điều mà các kỹ sư giỏi nhất lại giỏi hơn một cách tương xứng. Các kỹ sư giỏi tự mình thiết kế các hệ thống mạnh mẽ hơn và dễ hiểu hơn đối với những người khác.

* Hãy bắt đầu bằng một nhóm nhỏ chất lượng.

Điều gì giúp các startup có thể đánh bại những ông lớn giàu có và thực lực lẫn kinh nghiệm hơn gấp nhiều lần. Đó là vì họ "nhỏ" dẫn đến rất nhiều ưu thế về quản lý và giao tiếp lẫn quy trình

Điều này cũng có nghĩa là các nhóm nhỏ gồm các kỹ sư giỏi nhất thường có thể xây dựng mọi thứ nhanh hơn so với các nhóm kỹ sư trung bình thậm chí rất lớn. Họ sử dụng tốt mã nguồn mở có sẵn và các dịch vụ đám mây phức tạp, đồng thời có khả năng tự động hóa quy trình và sử dụng các công cụ khác, để thể tập trung giải quyết các khía cạnh về vấn đề sáng tạo của công việc. Họ nhanh chóng thử nghiệm các ý tưởng khác nhau với người dùng bằng cách ưu tiên các tính năng chính và cắt bỏ những công việc không quan trọng.

Các nhóm kỹ sư giỏi quy mô nhỏ hơn cũng sẽ tạo ra ít lỗi và vấn đề bảo mật hơn các nhóm kỹ sư trung bình quy mô lớn hơn. Bởi quy mô nhóm càng lớn càng có nhiều tác giả, thì càng có nhiều phong cách viết code, giả định, hướng giải pháp và những điều kỳ quặc xảy ra trong sản phẩm cuối cùng. Ngược lại, một hệ thống được xây dựng bởi một nhóm nhỏ các kỹ sư giỏi sẽ ngắn gọn, mạch lạc hơn và được người tạo ra nó hiểu rõ hơn. Bạn không thể đảm bảo bảo mật bằng checklist mà cần sự hiểu biết và quan tâm tới nó từ chính những thành viên trong dự án.












