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

Thử nghiệm thay đổi tham số Apriori và so sánh kết quả
Bộ tham số

Tham số gốc (Version A):

FILTER_MIN_SUPPORT = 0.01

FILTER_MIN_CONF = 0.30

FILTER_MIN_LIFT = 1.2

Tham số mới (Version B – chặt hơn):

FILTER_MIN_SUPPORT = 0.02

FILTER_MIN_CONF = 0.40

FILTER_MIN_LIFT = 1.2

Mục đích thay đổi: giảm “nhiễu” và tập trung vào các luật ổn định hơn (xuất hiện đủ nhiều và có độ tin cậy cao), thay vì thu được quá nhiều luật nhưng khó hành động.

1. Apriori là gì? (Feynman style – dễ hiểu)

Hãy tưởng tượng bạn là chủ cửa hàng và muốn biết khách hay mua những món nào cùng nhau. Nếu thấy “mua A thường mua kèm B”, bạn có thể:

đặt A gần B,

tạo combo,

gợi ý mua thêm khi khách đã chọn A.

Apriori giúp tìm các “quy tắc mua chung” dựa trên 3 thước đo:

Support: mức độ phổ biến (bao nhiêu hóa đơn có món đó).

Confidence: nếu có A thì khả năng có B là bao nhiêu.

Lift: A và B “đi cùng nhau” mạnh hơn ngẫu nhiên không ( > 1 là có liên kết đáng kể).

2. Thiết kế thí nghiệm

Dùng cùng một dataset (Online Retail – UCI).

Giữ nguyên quy trình tiền xử lý, chỉ thay ngưỡng lọc luật.

Mục tiêu so sánh:

Số lượng luật còn lại sau lọc

Độ đa dạng danh mục của luật

Khả năng ứng dụng kinh doanh (hành động được hay không)

3. Kết quả so sánh (tóm tắt)
3.1 Số lượng luật

Version A (gốc):

Luật tạo ra ban đầu: 3,856

Sau lọc theo ngưỡng: 1,794

Version B (chặt hơn):

Sau lọc: 136

Nhận xét: Khi tăng min_support từ 1% → 2% và min_conf từ 30% → 40%, số luật giảm rất mạnh. Đây là hành vi “đúng kỳ vọng” vì ta đang yêu cầu luật phải phổ biến hơn và đáng tin hơn.

3.2 Chất lượng và tính đại diện của luật

Version A: nổi bật nhóm “HERB MARKER …” với lift cực cao (≈ 70–74) và confidence gần 100%.
→ Luật rất “mạnh”, nhưng có xu hướng thuộc về một cụm khách hàng/sản phẩm niche, dễ khiến insight bị “lệch” về một nhóm nhỏ.

Version B: luật trải rộng hơn qua các nhóm như Charlotte bag (nhiều mẫu/màu), bộ ấm trà Regency, đồng hồ báo thức…
lift giảm còn khoảng 10–27, nhưng confidence vẫn cao (≈ 40–90%).
→ Luật “ít hơn” nhưng dễ triển khai hơn, phản ánh hành vi mua chung có tính tổng quát hơn.

Ví dụ ẩn dụ để trình bày:

Version A giống như tìm “cặp bạn thân” trong một nhóm nhỏ rất khăng khít.

Version B giống như tìm “mối liên hệ phổ biến” trong cả lớp – ít nhưng đáng dùng.

4. Ưu – nhược điểm theo mục tiêu kinh doanh
Version A (ngưỡng thấp hơn)

Ưu:

Khai thác sâu các cụm hàng “rất liên quan” (lift cực cao).

Hợp cho chiến lược chuyên sâu theo niche (combo thảo mộc, set phụ kiện đồng bộ…).

Nhược:

Quá nhiều luật → khó chọn luật để hành động.

Dễ bị chi phối bởi nhóm sản phẩm đặc thù/niche.

Version B (ngưỡng chặt hơn)

Ưu:

Ít luật → dễ đọc, dễ chọn, dễ triển khai.

Tập trung luật ổn định: phổ biến hơn và confidence cao hơn.

Nhược:

Có thể bỏ lỡ các luật hiếm nhưng “siêu mạnh” (một số niche).

Kết luận lựa chọn:

Nếu mục tiêu là insight tổng thể / trưng bày / cross-sell đại trà → Version B hợp hơn.

Nếu mục tiêu là khai thác niche / chăm nhóm khách đặc thù → Version A có giá trị.

Chủ đề 2: Tìm “Product Hub” (Sản phẩm trung tâm)
5. Định nghĩa Product Hub (dễ hiểu)

Trong mạng lưới mua sắm, có những sản phẩm đóng vai trò “trung tâm” vì:

xuất hiện trong nhiều luật,

kết nối với nhiều sản phẩm khác,

có thể kéo theo mua thêm.

Cách xác định: đếm số lần mỗi sản phẩm xuất hiện trong:

antecedents (mua trước)

consequents (mua kèm)

Sản phẩm xuất hiện càng nhiều → càng có xu hướng là “hub”.

6. Product Hub theo từng phiên bản
6.1 Version A (1,794 luật sau lọc) – hub theo hướng “niche”

Top hub (tần suất bạn cung cấp):

HERB MARKER THYME: ~1,200 lần

HERB MARKER ROSEMARY: ~1,100 lần

HERB MARKER PARSLEY: ~1,000 lần

HERB MARKER BASIL: ~800 lần

HERB MARKER MINT: ~700 lần

Giải thích: Đây là “hub” vì nhóm thảo mộc tạo thành một cụm mua kèm rất mạnh, lift cực cao → khách mua một món trong nhóm này có xu hướng mua thêm các món còn lại.

6.2 Version B (136 luật sau lọc) – hub theo hướng “đại trà”

Top hub (tần suất bạn cung cấp):

RED RETROSPOT CHARLOTTE BAG: 12 lần

STRAWBERRY CHARLOTTE BAG: 10 lần

CHARLOTTE BAG SUKI DESIGN: 8 lần

ROSES REGENCY TEACUP AND SAUCER: 8 lần

GREEN REGENCY TEACUP AND SAUCER: 8 lần

Giải thích: Các hub này phản ánh hành vi mua theo “dòng sản phẩm/biến thể” (cùng loại túi nhưng khác màu/mẫu; cùng bộ ấm trà nhưng khác phiên bản). Dạng này thường rất phù hợp để bố trí kệ và làm gợi ý mua thêm.

7. So sánh Hub: Version A vs Version B

Độ tập trung:

Version A: ít hub chính nhưng mỗi hub xuất hiện cực nhiều → “sâu và hẹp”

Version B: nhiều hub phân tán hơn, tần suất thấp hơn → “rộng và dễ triển khai”

Đa dạng danh mục:

Version A: thiên về 1 cụm thảo mộc

Version B: túi xách + ấm trà + nhóm sản phẩm gia dụng/trang trí

Giá trị cross-sell:

Version A: gợi ý combo trong niche (tăng giá trị giỏ hàng cho nhóm khách cụ thể)

Version B: gợi ý mua thêm phổ biến (ứng dụng rộng cho nhiều khách hơn)

8. Đề xuất bố trí và khuyến nghị kinh doanh (rút ra từ Hub)
Với Version A (thảo mộc)

Tạo “khu thảo mộc”: đặt THYME làm điểm trung tâm, nhóm ROSEMARY/PARSLEY/BASIL/MINT xung quanh.

Combo: “Bộ gia vị thảo mộc” (bundle) + giảm giá mua 3–5 sản phẩm.

Với Version B (Charlotte bag & Regency teacup)

Gom Charlotte bags theo cụm màu/mẫu; dùng hub “RED RETROSPOT…” làm sản phẩm mồi kéo các mẫu còn lại.

Gom Regency teacup & saucer theo bộ (green/roses/…); tạo gợi ý mua theo set và trưng bày theo “bộ sưu tập”.