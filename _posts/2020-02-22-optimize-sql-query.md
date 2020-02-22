---
layout: post
title: Ghi chú SQL query tháng 2
tags: [sql, optimize]
comments: false
---

Trong thực tế, những nỗ lực tối ưu câu truy vấn đem lại hiệu quả thấp hơn nhiều so với việc thiết kế lược đồ (shema) tốt. Lược đồ càng tốt thì càng dễ dàng để viết truy vấn. 
Bên cạnh đó đa phần công việc đều được thực hiện bởi những logic hết sức đơn giản (không cần suy nghĩ quá nhiều về vấn đề hiệu năng). Thêm nữa bản thân các hệ quản trị cơ sở dữ liệu cũng thực hiện tối ưu câu truy vấn dựa vào kiến trúc bên dưới của nó thông qua việc xem xét câu truy vấn đó và dữ liệu thống kê về cơ sở dữ liệu để quyết định cách tốt nhất thực hiện câu truy vấn chủ yếu là liên quan tới việc quyết định có nên sử dụng chỉ mục (indexes) không hay hashing, loại bảng nào sẽ được lưu trữ trong main storage, sử dụng kỹ thuật sort nào, ...

Những điều trên không có nghĩa là việc chú ý tối ưu hiệu năng khi viết một câu truy vấn là thừa thãi, không đem lại hiệu quả. Muốn tối ưu hiệu năng khi viết một câu truy vấn là hãy giữ câu truy vấn đó **đơn giản nhất có thể** (as simple as possible). Khi viết một câu truy vấn quá dài sẽ đồng nghĩa nó có khả năng tối ưu càng cao. Hơn thế nữa việc giữ cho một câu truy vấn đơn giản nhất có thể còn giúp nó càng dễ đọc, dễ hiểu và **đẹp** (simple is beautiful).

## Tips viết câu truy vấn hiệu quả hơn

### 1. Sử dụng điều kiện tìm kiếm hợp lý
**Cố gắng tránh phải thực hiện phép JOIN nếu có thể**
Làm việc với cơ sở dữ liệu quan hệ là làm việc với các bảng. Dữ liệu cần truy vấn có thể nằm ở các bảng riêng biệt và thông qua mệnh đề FROM để JOIN các bảng tạo thành bảng làm việc (working table). Nhưng chúng ta không sử dụng toàn bộ dữ liệu của bảng này do đó có thêm mệnh đề WHERE để loại bỏ bớt các bản ghi không thỏa mãn điều kiện đặt ra và nhiều khi dữ liệu từ bảng làm việc nhỏ hơn rất nhiều so với dữ liệu thỏa mãn điều kiện của mệnh đề WHERE.Chẳng hạn, một cơ sở dữ liệu lưu trữ thông tin của tất cả người dùng và cả những shops mà nó quản lý. Để tìm kiếm tất cả users và shops đặt tại cùng thành phố H. Ta có thể thực hiện như sau
~~~sql
SELECT * 
FROM users, shops
WHERE users.city = shops.city
AND shops.city = 'H';
~~~
Xem xét câu truy vấn, có thể thấy nếu mệnh đề được thực hiện trước, phép JOIN 2 bảng sẽ được thực hiện và ta có tất cả N*M rows (Với N và M là số bản ghi của bảng shops và users, giả sử N = 100 M = 10000) như vậy ta sẽ phải xem xét tất cả 1 000. 000 bản ghi sau khi thực hiện mệnh đề JOIN để kiểm tra users.city = shops.city bất kể số lượng thực tế các shops tại thành phố H và user tại thành phố H khá nhỏ (giả sử có khoảng 5 shops và 10100 users tại thành phố H)
Khi cần tìm một cách giải quyết mới cho việc viết truy vấn SQL, kinh nghiệm hồi còn đi học là thử viết lại câu hỏi của câu truy vấn đó sang một dạng khác mà không thay đổi logic xem sao? (Bởi tôi thường có xu hướng viết câu truy vấn dựa trên yêu cầu được đặt ra ở ví dụ trên đó là viết điều kiện users và shops đặt tại cùng thành phố và thành phố đó là thành phố H). Câu này thì dễ dàng viết lại thành "Các shops tại thành phố H và các users tại thành phố H".
~~~sql
SELECT * 
FROM users, shops
WHERE users.city = 'H'
AND shops.city = 'H';
~~~
Khi thực hiện câu truy vấn này, nó sẽ chỉ kiểm tra các shops tại H có khoảng 5 shops để tạo thành một bảng làm việc và CROSS JOIN với bảng làm việc khác khi kiểm tra các users tại thành phố H có 100 users, tổng số bản ghi cần xem xét là 5*100 = 500.

**Lựa chọn index sử dụng phù hợp cho SQL engine**
Xem xét 2 bảng gồm 1 bảng lớn chứa các biên bản sửa chữa xexe (audits), mỗi audit đều được đánh một mã trạng thái được đánh index để biết biên bản đó đang ở khâu nào (tiếp nhận xe, chờ xác nhận, đang sửa chữa, đã sữa chữa xong, ...). Bảng status cho biết tên trạng thái đó (ví dụ 1 là tiếp nhận xe, 2 là chờ xác nhận). Để truy vấn các audit kèm theo status của nó có thể thực hiện bằng cách
~~~sql
SELECT * 
FROM Audits as A, Status as S
WHERE A.status = S.status
~~~ 
Giả sử A có M = 1000 .000 hàng và S có N = 10 hàng, M lớn hơn N rất nhiều lần
Ghi nhớ: B-tree indexes cung cấp 3 lợi ích chính đó là

1. Tìm kiếm một giá trị cụ thể hoặc một khoảng giá trị.
2. Trả về các hàng theo thứ tự index.
3. Kích thước hàng nhỏ hơn bảng cơ sở (clustered index hoặc heap).

Ở câu truy vấn này không có giá trí cụ thể nào được tìm kiếm do vậy nó sẽ sử dụng lợi ích thứ 2 đó là trả về các hàng theo thứ tự index (do cả 2 cột của 2 bảng đều được đánh index). Do tận dụng được lợi ích thứ 2 đó là các hàng theo thứ tự index do đó thay vì thực hiện CROSS JOIN 2 bảng tốn kém chi phí hiệu năng hơn nó chỉ cần thực hiện MERGE JOIN (merge chỉ thực hiện được các tập hợp có thứ tự). So sánh phép CROSS JOIN với N*M = 10 triệu và MERGE JOIN với N+M ~ 1 triệu có thể thấy lợi ích của index trong TH này.

Tuy nhiên, nếu ta viết truy vấn để ép thực hiện một vòng lặp qua index của bảng nhỏ hơn (bảng S) thì sao ? Để ép thực hiện 1 vòng lặp qua index của bảng nhỏ hơn ta chỉ cần thêm một điều kiện luôn luôn đúng (TRUE) đối với index của bảng nhỏ hơn như sau
~~~sql
SELECT * 
FROM Audits as A, Status as S
WHERE S.status > 0 
AND A.status = S.status
~~~ 
Nhận thấy mọi giá trị của cột status của bảng S đều lớn hơn 0 (bắt đầu từ 1) nên điều kiện này luôn đúng. Việc thực hiện điều kiện này ép buộc SQL engine phải thực hiện một vòng lặp đi qua index của bảng S, đọc giá trị từ bảng S trước và thực hiện tìm kiếm một giá trị cụ thể (ở đây là giá trị vừa đọc được từ bảng S) trên bảng A tận dụng lợi ích thứ 1 của B-tree index. Với lợi thế này nó sẽ không đọc (scan) toàn bộ các hàng của bảng A vốn rất lớn mà truy xuất thông qua index status_index của bảng A đem lại hiệu năng tốt hơn nhiều.
 
**BETWEEN**

Các trình tối ưu của SQL thường không hoạt động hoặc hoạt động rất kém đối với chuỗi ký tự. Do vậy khi viết truy vấn với điều kiện tìm kiếm chuối hãy xem xét kỹ càng hơn yêu cầu đặt ra và lựa chọn cách tiếp cận khôn ngoan hơn. Chẳng hạn, để tìm kiếm các em học sinh đang theo học khối 12 mà ở Việt Nam thường được đánh theo mã 12A0, 12A2, .. 12A9. Có thể viết truy vấn
~~~sql
SELECT *  
FROM Students 
WHERE class LIKE '12A%';
 ~~~
Đây là cách viết gây tiêu hao hiệu năng nhất, khi xem xét yêu cầu trên có thể thấy mã lớp chỉ có thể dừng lại ở 10 lớp, ta có thể viết lại câu truy vấn với hiệu năng tốt hơn như sau
~~~sql
SELECT *  
FROM Students 
WHERE class LIKE '12A_';
 ~~~
Tuy nhiên việc sử dụng LIKE predicate vẫn gây tốn hiệu năng, thay vào đó ta có thể sử dụng BETWEEN trong TH này
~~~sql
SELECT *  
FROM Students 
WHERE class BETWEEN '12A0' AND '12A9';
~~~

### 2. Đưa thêm các thông tin JOIN bảng vào câu truy vấn

Như ghi chú ở phần đầu, hầu hết mọi hệ quản trị cơ sở dữ liệu đều có trình tối ưu optimizer đi kèm. Cách tối ưu giữa các hệ quản trị CSDL cũng hoàn toàn khác nhau tuy nhiên chúng đều phải dựa thông tin mà câu truy vấn gửi xuống và phân tích, thông tin càng đơn giản (các câu truy vấn lồng nhau quá sâu thường gây khó khăn cho optimizer phân tích chúng) và càng nhiều thì optimizer càng có nhiều cơ sỡ để phân tích và thực hiện câu truy vấn đó theo cách tối ưu nhất. Bởi những optimzer này tối ưu cách thực hiện câu truy vấn dựa trên cả kiến trúc bên dưới của nó nên đôi khi nó thực sự khá hữu ích. 

Không cần lo lắng về việc viết càng nhiều thông tin không cần thiết (không cần thiết cho kết quả của câu truy vấn) sẽ làm cho SQL engine phải thực hiện theo nhiều việc hơn nữa bởi bản phân các optimizer sẽ phân tích toàn bộ câu truy vấn nó lấy ra và ghi nhận toàn các thông tin thu được, cố gắng hiểu mục tiêu của câu truy vấn cần đạt kết quả gì rồi đưa ra phương án thực hiện do vậy nó sẽ không thực hiện các công việc thừa thãi so với kết quả cần truy vấn nhưng lại rất cần thông tin để phân tích.

Chẳng hạn, nếu muốn JOIN 3 bảng với nhau trên cùng một cột.
 
 ~~~sql
 SELECT *  
 FROM Table1, Table2, Table3 
 WHERE Table3.col1 = Table2.col1   
 AND Table2.col1 = Table1.col1   
 ~~~
Giả sử bảng Table1 là một bảng rất nhỏ, nếu thực hiện equi-JOIN trên bảng Table1 với Table2 sẽ giúp loại bỏ các bản ghi không matched của bẳng 2 với bảng 1 sẽ giúp thu nhỏ tập kết quả của phép JOIN này. Do đó nó cần được đặt lên điều kiện đầu tiên để thực hiện trước. May mắn là các optimzer đều làm vậy sau khi phân tích size của từng bảng. Tuy nhiên, với một cách vẫn đúng logic, ta có thể viết như sau
~~~sql
 SELECT *  
 FROM Table1, Table2, Table3 
 WHERE Table1.col1 = Table2.col1   
 AND Table1.col1 = Table3.col1 
 ~~~
Bảng 1 sẽ được sử dụng loại bỏ các bản ghi không matched của bẳng 2 và 3 với bảng 1.

Tuy nhiên vẫn còn 1 option tốt hơn nữa như đã nêu ở đề mục này **Đưa thêm các thông tin JOIN bảng vào câu truy vấn** bằng cách này optimizer sẽ có thêm thông tin và đưa ra phương án thực hiện tốt nhất 
~~~sql
 SELECT *  
 FROM Table1, Table2, Table3 
 WHERE Table1.col1 = Table2.col1   
 AND Table2.col1 = Table3.col1   
 AND Table3.col1 = Table1.col1; 
 ~~~

### 3. Đánh index một cách cẩn trọng
Khi xem xét về hiệu năng truy vấn, sự đơn giản rõ ràng đánh index cần phải đưa lên vị trí đầu tiên bởi hiệu quả ngay tức khắc của nó. Tuy nhiên tôi thường tập trung xem xét việc đánh index một cách kỹ lương ở bước tạo lược đồ bởi hiệu năng có được này là một sự đánh đổi, hiệu năng truy vấn sẽ phải đánh đổi cho hiệu năng update/insert/delete. Do đó cần phải xem xét cân bằng điều này nếu hiệu năng update/insert/delete không phải là vấn đề lớn ở đây thì index chắc chắn là tối ưu hàng đầu. Khi mà lược đồ vẫn còn được đánh giá là tốt thì việc sửa đổi hay cập nhật index sẽ cần phải được xem xét kỹ lưỡng hơn. Hơn thế nữa khi có quá nhiều index xuất hiện nó có thể đánh lừa optimizer sử dụng nó khi không nên sử dụng bởi trình tối ưu hóa có xu hướng dựa vào index rất nhiều.

 ### 4. Cập nhật các thống kê (Update statistics)

Trong SQL Server và rất nhiều hệ quản trị CSDL khác số liệu thống kê rất cần thiết cho trình tối ưu hóa truy vấn để chuẩn bị một kế hoạch thực hiện được tối ưu hóa.
Các thống kê này cung cấp phân phối các giá trị cột cho trình tối ưu hóa truy vấn và nó giúp SQL Server ước tính số lượng hàng (còn được gọi là cardinality). 
Trình tối ưu hóa truy vấn nên được cập nhật thường xuyên. Số liệu thống kê không phù hợp có thể đánh lừa trình tối ưu hóa truy vấn để chọn các toán tử tốn kém như quét chỉ mục qua chỉ mục tìm kiếm và nó có thể gây ra các vấn đề về CPU, bộ nhớ và IO cao trong SQL Server.

Có nhiều cách thực hiện cập nhật thống kê bao gồm thủ công và tự động. Sau đây là một số dấu hiện báo hiệu rằng bạn nên thực hiện update statistics

1. Nếu hai câu truy vấn có cùng cấu trúc cơ bản nhưng có thời gian thực hiện khác nhau. 
2. Bạn vừa thực hiện tải rất nhiều dữ liệu mới.
3. Bạn vừa thực hiện xóa rất nhiều dữ liệu cũ.
----
