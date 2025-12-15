👥 Thông tin Nhóm

Nhóm:04
…
Thành viên:
Đinh Trọng Quỳnh
Nguyễn Việt Chung
Nguyễn Mạnh Đông

…

Chủ đề: Phân tích giỏ hàng và khai thác luật kết hợp mua sắm

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
→ Cho thấy các sản phẩm phổ biến nhất như JUMBO BAG RED RETROSPOT, REGENCY CAKESTAND 3 TIER.

Hình 2: Phân phối độ dài itemset
→ Chủ yếu là itemset độ dài 1 và 2, phù hợp với hành vi mua kèm thực tế.

Hình 3: Top 20 luật theo Lift
→ Các cặp sản phẩm có liên kết mạnh như bộ Regency Teacup, Charlotte Bag.

Hình 4: Top 20 luật theo Confidence
→ Nhiều luật có confidence từ 60–90%, rất phù hợp cho gợi ý mua thêm.

Hình 5: Scatter Support vs Confidence (màu = Lift)
→ Các luật tập trung trong vùng support ≥ 0.02 và confidence ≥ 0.4.

Hình 6: Mạng lưới luật kết hợp (Network graph)
→ Thể hiện rõ các product hub và cụm sản phẩm.

6. Insight từ Kết quả

Insight #1:
Các sản phẩm cùng bộ hoặc cùng biến thể màu (Regency Teacup, Charlotte Bag) có xu hướng được mua cùng nhau với lift và confidence cao.

Insight #2:
Những luật có lift rất cao nhưng support vừa phải cho thấy hành vi mua theo bộ sưu tập, không phải ngẫu nhiên.

Insight #3:
Hành vi mua kèm chủ yếu diễn ra theo cặp sản phẩm, không theo nhóm lớn.

Insight #4:
Một số sản phẩm đóng vai trò Product Hub, xuất hiện trong nhiều luật và kéo theo nhiều sản phẩm khác.

Insight #5:
Tham số chặt giúp loại bỏ luật nhiễu và làm nổi bật các mẫu mua sắm ổn định, dễ triển khai.

7. Kết luận & Đề xuất Kinh doanh

Cross-selling:
Gợi ý mua kèm các sản phẩm trong cùng nhóm (Charlotte Bag, Regency Teacup).

Bố trí kệ hàng:
Đặt các product hub ở vị trí trung tâm, các biến thể xung quanh.

Khuyến mãi theo mùa:
Các cặp sản phẩm liên quan đến Giáng sinh hoặc quà tặng có thể được gộp thành combo.

8. Link Code & Notebook

Notebook: …

GitHub Repo: …