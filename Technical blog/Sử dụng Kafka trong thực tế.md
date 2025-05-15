# Vấn đề chia partition
Như đã biết việc message được gửi đến 1 topic và được broker phân bổ đến partition phù hợp dựa vào partition key hashing để route đến partition phù hợp.
Vậy nếu chúng ta chia topic thành 10 partitions nhưng thực tế key hashing sẽ quyết định topic tới partition nào. Vậy sẽ xảy ra hiện tượng là có partition có nhiều message có partition không. Cái này sẽ quyết định do key hashing algo quyết định tức là ví dụ gửi với số lượng key ít thì khiến xảy ra tình trạng mất cân bằng do phân bổ không đều. Tương tự như partition trong database vậy.
=> theo best practice thì nên tạo số lượng partition của 1 topic = 2N (với N là broker trong cluster)
