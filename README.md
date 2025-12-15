# 📦 Case Study: Phân tích giỏ hàng với Apriori

## 👥 Thông tin Nhóm
- **Nhóm:** Nhóm 4
- **Thành viên:**
  - Thành viên 1
  - Thành viên 2
- **Chủ đề:** Phân tích giỏ hàng và luật kết hợp sản phẩm
- **Dataset:** Online Retail (UCI)

## Mục tiêu
Mục tiêu của nhóm là phân tích dữ liệu bán lẻ để khám phá các sản phẩm thường được mua cùng nhau, từ đó đề xuất chiến lược cross-selling và tối ưu hóa bố trí hàng hóa trong cửa hàng.

## 1. Ý tưởng & Feynman Style
Hãy tưởng tượng bạn là chủ siêu thị và muốn biết khách hàng thường mua gì cùng nhau. Ví dụ, nếu ai mua sữa thì hay mua bánh mì, bạn có thể đặt chúng gần nhau để tăng doanh số. Apriori là công cụ giúp tìm ra những "quy tắc mua chung" này từ hàng ngàn hóa đơn.

Apriori phù hợp cho bài toán giỏ hàng vì nó xử lý dữ liệu giao dịch lớn, tìm ra các nhóm sản phẩm liên kết thực sự (không phải ngẫu nhiên). Ý tưởng thuật toán: Bắt đầu từ sản phẩm đơn, dần mở rộng thành nhóm lớn hơn, loại bỏ những nhóm không đủ phổ biến.

## 2. Quy trình Thực hiện

1) Load & làm sạch dữ liệu
2) Tạo ma trận basket
3) Áp dụng Apriori
4) Trích xuất luật
5) Trực quan hóa
6) Phân tích insight

## 3. Tiền xử lý Dữ liệu
- Những bước làm sạch:
  - Loại bỏ sản phẩm "rỗng"
  - Loại bỏ transaction bị cancel (InvoiceNo bắt đầu "C")
  - Loại bỏ số lượng âm

- Thống kê nhanh:
  - Số giao dịch sau lọc: ~400,000
  - Số sản phẩm duy nhất: ~4,000

## 4. Áp dụng Apriori
**Tham số sử dụng:**
- `min_support = 0.01`
- `min_threshold = 1.0`
- `max_len = 3`

```python
from mlxtend.frequent_patterns import apriori, association_rules

frequent_itemsets = apriori(basket_df, min_support=0.01, use_colnames=True)
rules = association_rules(frequent_itemsets, metric="lift", min_threshold=1.0)
rules.sort_values("lift", ascending=False, inplace=True)
rules.head()
```

## 5. Trực quan hóa (Visualization)
- Hình 1: Biểu đồ scatter plot của support vs confidence, cho thấy các luật mạnh.
- Hình 2: Mạng lưới các luật với lift cao, minh họa mối liên kết giữa sản phẩm.

## 6. Insight từ Kết quả
**Insight #1:** Các sản phẩm thảo mộc (herb markers) có liên kết rất mạnh, với lift lên đến 74, cho thấy khách hàng mua combo gia vị thường xuyên.

**Insight #2:** Túi xách Charlotte (nhiều màu) là nhóm sản phẩm trung tâm, xuất hiện nhiều trong luật, phù hợp cho chiến lược phối màu.

**Insight #3:** Bộ ấm trà Regency (xanh, hồng, hoa) có confidence cao, gợi ý khách mua một màu thường mua thêm màu khác.

**Insight #4:** Đồng hồ báo thức Bakelike (xanh, đỏ, hồng) liên kết chặt, phù hợp cho khách mua quà tặng.

**Insight #5:** Túi Jumbo (táo, lê) và túi Woodland Animals có support vừa phải nhưng lift mạnh, cho thấy sở thích mua theo chủ đề.

## 7. Kết luận & Đề xuất Kinh doanh
- Gợi ý cross-sell: Đề xuất sản phẩm liên quan khi khách thêm vào giỏ, như thêm ROSES TEACUP khi mua GREEN TEACUP.
- Gợi ý sắp xếp hàng trên kệ: Nhóm thảo mộc ở khu gia vị, túi Charlotte ở khu phụ kiện, ấm trà ở khu đồ uống.
- Gợi ý khuyến mãi theo mùa: Combo Giáng sinh với WOODEN CHRISTMAS items, hoặc mùa hè với túi màu sáng.
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