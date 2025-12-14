# Shopping Cart Analysis

Phân tích dữ liệu bán lẻ để tìm ra mối quan hệ giữa các sản phẩm thường được mua cùng nhau bằng các kỹ thuật **Association Rule Mining** (Apriori). Project triển khai pipeline đầy đủ từ xử lý dữ liệu → phân tích → khai thác luật → sinh báo cáo.

---

## Features

- Làm sạch dữ liệu & xử lý giá trị lỗi
- Xây dựng basket matrix (transaction × product)
- Khai phá tập mục phổ biến (Frequent itemsets)
- Sinh luật kết hợp (Association Rules)
- Các chỉ số:
  - Support
  - Confidence
  - Lift
- Visualization với:
  - bar chart
  - scatter plot
  - network graph
  - interactive Plotly
- Tự động hóa pipeline bằng **Papermill**

---

## Project Structure

```text
shopping_cart_analysis/
├── data/
│   ├── raw/
│   │   └── online_retail.csv
│   └── processed/
│       ├── cleaned_uk_data.csv
│       ├── basket_bool.parquet
│       └── rules_apriori_filtered.csv
│
├── notebooks/
│   ├── preprocessing_and_eda.ipynb
│   ├── basket_preparation.ipynb
│   ├── apriori_modelling.ipynb
│   └── runs/
│       ├── preprocessing_and_eda_run.ipynb
│       ├── basket_preparation_run.ipynb
│       └── apriori_modelling_run.ipynb
│
├── src/
│   └── apriori_library.py
│
├── run_papermill.py
├── requirements.txt
└── README.md
```

---

## Installation

```bash
git clone <your_repo_url>
cd shopping_cart_analysis
pip install -r requirements.txt
Data Preparation
Đặt file gốc vào:
```

```bash
data/raw/online_retail.csv
File output sẽ được sinh tự động vào:
```

```bash
data/processed/
```

Run Pipeline (Recommended)
Chạy toàn bộ phân tích chỉ với 1 lệnh:

```bash
python run_papermill.py
```
Kết quả sinh ra:

```bash
data/processed/cleaned_uk_data.csv
data/processed/basket_bool.parquet
data/processed/rules_apriori_filtered.csv
notebooks/runs/apriori_modelling_run.ipynb
```

### Changing Parameters
Các tham số có thể chỉnh trong run_papermill.py:

```python
MIN_SUPPORT=0.01
MAX_LEN=3
FILTER_MIN_CONF=0.3
FILTER_MIN_LIFT=1.2
```

Hoặc sửa trong cell PARAMETERS của mỗi notebook để chạy với cấu hình khác nhau.

### Visualization & Results
Notebook 03 hiển thị các biểu đồ sau:

Top luật theo Lift

Top luật theo Confidence

Scatter Support–Confidence–Lift

Network Graph giữa các sản phẩm

Biểu đồ Plotly tương tác

Bạn có thể export sang HTML:

```bash
jupyter nbconvert notebooks/runs/priori_modelling_run.ipynb --to html
```

### Ứng dụng thực tế
Product recommendation

Cross-selling strategy

Combo gợi ý sản phẩm

Phân tích hành vi mua hàng

Sắp xếp sản phẩm tại siêu thị

### Tech Stack

| Công nghệ | Mục đích |
|----------|----------|
| Python | Ngôn ngữ chính |
| Pandas | Xử lý dữ liệu transaction |
| MLxtend | Apriori / FP-Growth association rules |
| Papermill | Chạy pipeline notebook tự động |
| Matplotlib & Seaborn | Visualization biểu đồ tĩnh |
| Plotly | Dashboard / biểu đồ tương tác |
| Jupyter Notebook | Môi trường notebook |

### Roadmap
 Thêm FP-Growth notebook (04)

 Streamlit dashboard để lọc luật


### Author
Project được thực hiện bởi:
Trang Le

📄 License
MIT — sử dụng tự do cho nghiên cứu, học thuật và ứng dụng nội bộ.
TrangLé
-------------------------------------------------------
Thử nghiệm đổi tham số thành
FILTER_MIN_SUPPORT=0.02,
FILTER_MIN_CONF=0.4,
FILTER_MIN_LIFT=1.2


* kết quả và so sánh với tham số gốc

1: Thuật Toán Apriori Là Gì?
Hãy tưởng tượng bạn là chủ cửa hàng và muốn biết khách hàng thường mua gì cùng nhau. 
Ví dụ, nếu ai mua sữa thì thường mua bánh mì, bạn có thể đặt chúng gần nhau để tăng doanh số. Apriori là công cụ giúp tìm ra những "quy tắc mua chung" này từ dữ liệu hóa đơn. Nó dựa trên ba ý chính:

Support: Tỷ lệ hóa đơn chứa sản phẩm đó (càng cao càng phổ biến).
Confidence: Xác suất nếu mua A thì mua B (càng cao càng đáng tin).
Lift: Mức độ liên kết thực sự (trên 1 nghĩa là liên kết mạnh hơn ngẫu nhiên).
Chúng ta lọc luật dựa trên ngưỡng để chỉ giữ lại những luật ý nghĩa.

2: Thí Nghiệm Với Hai Bộ Tham Số
Em chạy thuật toán trên cùng một tập dữ liệu bán lẻ (gần 400.000 giao dịch từ Anh). Dữ liệu bao gồm các sản phẩm như túi xách, ấm nước, và đồ trang trí Giáng sinh. Tôi so sánh hai phiên bản:

Phiên bản 1 (Tham số ban đầu): Support tối thiểu 1%, Confidence 30%, Lift 1.2.
Phiên bản 2 (Tham số chặt hơn): Support tối thiểu 2%, Confidence 40%, Lift 1.2.
Tại sao thay đổi? Vì phiên bản 1 có thể tạo ra quá nhiều luật "nhiễu" (ít ý nghĩa), trong khi phiên bản 2 chỉ giữ lại luật thực sự mạnh mẽ.

3: Kết Quả Chính
Số luật tạo ra: Phiên bản 1 có 3.856 luật ban đầu, lọc còn 1.794 luật. Phiên bản 2 có ít luật hơn nhiều – chỉ 136 luật sau lọc.
Chất lượng luật:
Phiên bản 1 tập trung vào sản phẩm thảo mộc (herb markers) với lift cực cao (70-74), confidence gần 100%. Điều này có nghĩa là những sản phẩm này liên kết rất chặt, nhưng có thể chỉ phổ biến trong một nhóm nhỏ khách hàng.
Phiên bản 2 mở rộng ra nhiều danh mục hơn: túi xách Charlotte (nhiều màu), ấm nước, đồng hồ báo thức, và bộ ấm trà Regency. Lift giảm xuống 10-27, nhưng confidence vẫn cao (40-90%). Luật này đa dạng hơn, phản ánh hành vi mua sắm chung của nhiều khách hàng hơn.
Sự khác biệt này như thế nào? Phiên bản 1 giống như tìm "người bạn thân nhất" trong một nhóm nhỏ, còn phiên bản 2 như tìm "người bạn chung" trong cả lớp học lớn hơn.

4: Ưu và Nhược
Phiên bản 1: Tốt cho chiến lược chuyên sâu (ví dụ, combo thảo mộc cho khách thích nấu ăn). Nhưng có thể bỏ lỡ cơ hội lớn hơn.
Phiên bản 2: Ít luật hơn, nhưng chất lượng cao hơn. Phù hợp cho cửa hàng muốn tối ưu hóa tổng thể, như đặt túi xách gần nhau để tăng cross-selling (bán chéo).
Nếu bạn là chủ cửa hàng, phiên bản 2 giúp bạn thấy bức tranh rộng hơn, trong khi phiên bản 1 giúp khai thác sâu một niche.

5: Bài Học Từ Thí Nghiệm
Tham số không phải là "đúng hay sai", mà phụ thuộc vào mục tiêu. Nếu bạn muốn luật siêu mạnh cho một sản phẩm cụ thể, dùng ngưỡng thấp. Nếu muốn luật ổn định cho toàn bộ, dùng ngưỡng cao.
Luôn thử nghiệm! Dữ liệu thực tế có thể bất ngờ – ở đây, tăng support đã thay đổi hoàn toàn danh mục sản phẩm nổi bật.

---- Dự theo chủ đề Chủ Đề 2: Tìm Sản Phẩm Trung Tâm (Product Hub)------
Bây giờ, hãy chuyển sang phần thú vị: tìm "sản phẩm trung tâm" trong phiên bản 2. Hãy tưởng tượng cửa hàng như một mạng lưới, và một số sản phẩm là "trung tâm" – chúng kết nối với nhiều sản phẩm khác, như một người bạn chung trong nhóm.

Bước 1: Cách Xác Định Product Hub
Em đếm số lần mỗi sản phẩm xuất hiện trong tất cả luật (cả antecedent – sản phẩm mua trước, và consequent – sản phẩm mua sau). Sản phẩm xuất hiện nhiều nhất là "hub" – chúng có khả năng kéo theo nhiều mua thêm khác.

Bước 2: Kết Quả Từ Phiên Bản 1
Phiên bản 1: Support 1%, Confidence 30%, Lift 1.2 → 1.794 luật, tập trung herb markers (thảo mộc).

Dựa trên 1.794 luật, tôi đếm tần suất xuất hiện của mỗi sản phẩm. Top "hub" là:

HERB MARKER THYME: Xuất hiện 1.200 lần (rất nhiều!). Đây là "trung tâm" của nhóm thảo mộc, liên kết với rosemary, parsley, basil, mint.
HERB MARKER ROSEMARY: 1.100 lần. Thường đi với thyme và parsley.
HERB MARKER PARSLEY: 1.000 lần. Kết nối với thyme và rosemary.
HERB MARKER BASIL: 800 lần. Liên kết với thyme và rosemary.
HERB MARKER MINT: 700 lần. Đi với thyme và parsley.
Những sản phẩm này là hub vì chúng xuất hiện trong hầu hết luật, với lift cực cao (70+), nghĩa là khách mua thảo mộc rất "trung thành" với nhóm này.

Kết Quả Từ Phiên Bản 2

Dựa trên 136 luật, top sản phẩm trung tâm là:

RED RETROSPOT CHARLOTTE BAG: Xuất hiện 12 lần. Đây là túi xách màu đỏ retro, liên kết với nhiều túi khác (pink polkadot, suki design, strawberry).
STRAWBERRY CHARLOTTE BAG: 10 lần. Túi dâu tây, thường đi với túi woodland và red retrospot.
CHARLOTTE BAG SUKI DESIGN: 8 lần. Túi thiết kế Suki, kết nối với nhiều biến thể khác.
ROSES REGENCY TEACUP AND SAUCER: 8 lần. Bộ ấm trà hồng, liên kết với xanh lá và hồng khác.
GREEN REGENCY TEACUP AND SAUCER: 8 lần. Bộ xanh lá, tương tự.
Những sản phẩm này là "hub" vì chúng xuất hiện trong nhiều luật, nghĩa là khách mua chúng thường mua thêm các sản phẩm liên quan.

So Sánh Với Phiên Bản 1 vs 2
Số lượng hub: Phiên bản 1 có ít hub hơn (chủ yếu 5-6 sản phẩm thảo mộc), nhưng mỗi hub xuất hiện cực nhiều. Phiên bản 2 có nhiều hub hơn (10+ sản phẩm đa dạng), nhưng tần suất thấp hơn (8-12 lần mỗi cái).
Đa dạng: Phiên bản 1 tập trung một niche (thảo mộc), phù hợp cho khách thích nấu ăn. Phiên bản 2 bao quát nhiều danh mục (túi xách, trà, đồng hồ), phản ánh hành vi mua sắm tổng quát hơn.
Vai trò cross-selling:
Phiên bản 1: Hub như THYME giúp gợi ý combo thảo mộc, tăng doanh số trong nhóm nhỏ nhưng loyal.
Phiên bản 2: Hub như RED RETROSPOT CHARLOTTE BAG giúp gợi ý đa dạng, phù hợp cho cửa hàng lớn muốn tối ưu toàn diện.
Đánh giá: Phiên bản 1 mạnh về "sâu" (niche), phiên bản 2 mạnh về "rộng" (đa dạng). Nếu bạn bán chuyên thảo mộc, dùng phiên bản 1; nếu bán tổng hợp, dùng phiên bản 2.

Đề Xuất Bố Trí Hàng Hóa So Sánh
Phiên bản 1: Tạo "khu thảo mộc" với THYME ở trung tâm, xung quanh ROSEMARY và PARSLEY. Trưng bày combo "Bộ gia vị thảo mộc".
Phiên bản 2: Nhóm túi Charlotte ở một kệ, ấm trà Regency ở kệ khác. Sử dụng hub như RED RETROSPOT để gợi ý "Túi xách phối màu".
Tóm lại, product hub giúp bạn thấy "điểm nóng" trong cửa hàng. Phiên bản 1 cho thấy thảo mộc là "ngôi sao", phiên bản 2 cho thấy túi xách và trà cũng mạnh.