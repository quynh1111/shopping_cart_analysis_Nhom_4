👥 Thông tin Nhóm

Nhóm:04
…
Thành viên:
Đinh Trọng Quỳnh
Nguyễn Việt Chung
Nguyễn Mạnh Đông

…

Chủ đề: 7.3.2 Chủ đề 2: Tìm sản phẩm trung tâm (Product Hub)
• Xác định sản phẩm xuất hiện nhiều nhất trong các luật.
• Đánh giá vai trò “hub” trong chiến lược cross-selling.
• Đề xuất cách bố trí hàng hóa.

Dataset: Online Retail (UCI – UK Retail Dataset)

🎯 Mục tiêu

Mục tiêu của nhóm là sử dụng thuật toán Apriori để:

Khai thác các mẫu mua kèm phổ biến trong dữ liệu bán lẻ thực tế.

So sánh ảnh hưởng của việc thay đổi tham số đến số lượng và chất lượng luật.

Xác định sản phẩm trung tâm (Product Hub) nhằm hỗ trợ chiến lược cross-selling và bố trí hàng hóa.

1. Ý tưởng & Feynman Style (Giải thích dễ hiểu)
Apriori dùng làm gì?

Apriori giúp trả lời câu hỏi:
“Khách hàng thường mua những sản phẩm nào cùng nhau?”

Tại sao phù hợp cho bài toán giỏ hàng?

Dữ liệu giỏ hàng gồm nhiều hóa đơn, mỗi hóa đơn chứa nhiều sản phẩm. Apriori giúp tìm ra các quy luật mua chung lặp lại nhiều lần, từ đó hỗ trợ bán chéo và gợi ý sản phẩm.

Ý tưởng thuật toán (ngắn gọn)

Apriori tìm các nhóm sản phẩm thường xuất hiện cùng nhau, sau đó lọc lại bằng các ngưỡng Support, Confidence và Lift để chỉ giữ các luật có ý nghĩa.

2. Quy trình Thực hiện

Load và làm sạch dữ liệu

Tạo ma trận giỏ hàng (basket matrix)

Áp dụng thuật toán Apriori

Sinh luật kết hợp (association rules)

Trực quan hóa kết quả

Phân tích insight và đề xuất kinh doanh

3. Tiền xử lý Dữ liệu
Các bước làm sạch

Loại bỏ sản phẩm rỗng (Description null)

Loại bỏ giao dịch bị hủy (InvoiceNo bắt đầu bằng “C”)

Loại bỏ các dòng có số lượng âm

Thống kê nhanh (sau lọc)

Số tập mục phổ biến: 400

Thời gian chạy Apriori: 16.05 giây

4. Áp dụng Apriori
Tham số sử dụng (Phiên bản 2)

min_support = 0.02

min_confidence = 0.4

min_lift = 1.2

Kết quả

Tổng số luật ban đầu: 218

Số luật sau khi lọc: 135

→ Số luật giảm mạnh so với tham số gốc, nhưng chất lượng cao và ổn định hơn.

5. Trực quan hóa (Visualization)

Hình 1: Top frequent itemsets theo support
<img width="1271" height="841" alt="image" src="https://github.com/user-attachments/assets/1b52be96-b9a4-4983-a16d-59ff02a8f414" />

→ Cho thấy các sản phẩm phổ biến nhất như JUMBO BAG RED RETROSPOT, REGENCY CAKESTAND 3 TIER.

Hình 2: Phân phối độ dài itemset
<img width="971" height="597" alt="image" src="https://github.com/user-attachments/assets/1506eb70-acf9-41bf-9018-215032d94e6c" />

→ Phần lớn tập mục có độ dài 1 và 2, cho thấy khách hàng thường mua theo cặp sản phẩm như:

GREEN REGENCY TEACUP AND SAUCER & PINK REGENCY TEACUP AND SAUCER

Hình 3: Top 20 luật theo Lift
<img width="1372" height="837" alt="image" src="https://github.com/user-attachments/assets/4f386ec2-9f2e-40bf-b4ff-1eb0a30c4869" />

→ Các cặp sản phẩm có liên kết mạnh như bộ Regency Teacup, Charlotte Bag.

Hình 4: Top 20 luật theo Confidence
→ Các cặp sản phẩm có liên kết mạnh như bộ Regency Teacup, Charlotte Bag.

Hình 5: Scatter Support vs Confidence (màu = Lift)
<img width="920" height="723" alt="image" src="https://github.com/user-attachments/assets/4e858710-3657-442b-a149-e1a194ea8147" />

→ Các luật tập trung trong vùng support ≥ 0.02 và confidence ≥ 0.4.

Hình 6: Mạng lưới luật kết hợp (Network graph)
<img width="1265" height="834" alt="image" src="https://github.com/user-attachments/assets/39051e3e-83a6-4618-a97c-963872c3255c" />

→ Thể hiện rõ các product hub và cụm sản phẩm.

6. Insight từ Kết quả (ghi RÕ TÊN HÀNG HÓA)

Insight #1 – Mua theo bộ sưu tập
Khách hàng thường mua cùng lúc các sản phẩm:

GREEN / PINK / ROSES REGENCY TEACUP AND SAUCER, cho thấy xu hướng mua trọn bộ ấm trà cùng dòng.

Insight #2 – Cặp sản phẩm trang trí Giáng sinh
Hai sản phẩm:

WOODEN HEART CHRISTMAS SCANDINAVIAN

WOODEN STAR CHRISTMAS SCANDINAVIAN
có mối liên kết rất mạnh (lift ≈ 27), phù hợp bán theo combo mùa lễ.

Insight #3 – Product Hub túi xách
RED RETROSPOT CHARLOTTE BAG đóng vai trò trung tâm, thường được mua cùng:

PINK POLKADOT CHARLOTTE BAG

STRAWBERRY CHARLOTTE BAG

WOODLAND CHARLOTTE BAG

Insight #4 – Hành vi mua cặp hộp ăn trưa
Khách mua SPACEBOY LUNCH BOX thường mua thêm DOLLY GIRL LUNCH BOX, cho thấy hai sản phẩm bổ sung cho nhau.

Insight #5 – Ưu điểm của tham số phiên bản 2
Với ngưỡng support và confidence cao, các luật còn lại đều liên quan đến sản phẩm phổ biến, bán ổn định, không bị lệch vào niche hiếm.

7. Kết luận & Đề xuất Kinh doanh (có tên hàng hóa)

Cross-selling:

Gợi ý mua PINK / GREEN / ROSES REGENCY TEACUP AND SAUCER theo bộ.

Gợi ý các mẫu Charlotte Bag cùng dòng khi khách chọn RED RETROSPOT.

Bố trí kệ hàng:

Đặt RED RETROSPOT CHARLOTTE BAG ở trung tâm kệ túi xách.

Nhóm WOODEN HEART và WOODEN STAR CHRISTMAS SCANDINAVIAN trong khu vực quà tặng Giáng sinh.

Khuyến mãi theo mùa:

Tạo combo lễ hội cho WOODEN HEART / STAR CHRISTMAS SCANDINAVIAN.

Combo “Lunch set” cho SPACEBOY LUNCH BOX & DOLLY GIRL LUNCH BOX.
