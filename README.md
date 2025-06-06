# 🔐  Bài toán 1: Dự đoán mở khóa thuê bao bị khóa 1 chiều

## 📌 Mô tả dự án
Dự đoán việc mở khóa thuê bao cho nhóm khách hàng tiềm năng bị khóa SIM.
Vì một vài lí do nào đó (chủ quan & khách quan), mà khách hàng bị khóa thuê bao 1 chiều, nhưng trong số đó lại có khách hàng tiềm năng sinh lợi. Do đó, ta quyết định nên hay không mở (phân loại 2 nhãn). Những khách hàng tiềm năng được mở khóa sẽ có giá trị kinh doanh hơn để đảm bảo không rời bỏ dịch vụ (như sự hỗ trợ mở khóa, giải đáp, cung cấp dịch vụ phù hợp).

## 📌 Phương án đánh giá
-	Lượng người được mở khóa thuê bao phán đoán được phải gần/đủ với lượng thuê bao được mở khóa thực tế (true positive).  Dùng AUC ROC để cực đại hóa true positive.
-	Việc xác định ra nhiều trường hợp false positive  (tức không mở nhưng lại dự đoán là mở) cũng cần được quan tâm vì thực tế các thuê bao đó không sinh lời nhưng lại làm ảnh hưởng đến các thuê bao khác, làm ảnh hưởng uy tín dịch vụ. Dùng precision để cực tiểu false positive.
-	Phán đoán đúng việc không mở khóa cho các thuê bao đang khóa không quan trọng vì không tác động được gì thêm.
-	Phán đoán ra các trường hợp false negative (thực tế cần mở nhưng lại dự đoán không mở ) cần quan tâm, vì có thể làm khách hàng bất mãn, giảm uy tin dịch vụ. Dùng recall để cực tiểu false negative.
-	Tóm lại, sử dụng độ đo AUC ROC và f1 score (để cân bằng recall và precision).

## 📊 Dữ liệu đầu vào
- Dữ liệu từ hơn 330,000 thuê bao giai đoạn 1/9/2021 – 14/10/2021.
- Bao gồm: ARPU, lượng data, số cuộc gọi, ngày gọi tổng đài, ngày nạp tiền, lịch sử đăng ký gói,...

## ⚙️ Xử lý dữ liệu
- Xử lý missing và duplicate values.
- Scaling ARPU, loại bỏ ngoại lai bằng capping.
- Chọn đặc trưng dựa trên tương quan và loại trừ data leakage.

## 📈 Huấn luyện mô hình
- Áp dụng các mô hình: Random Forest và XGBoost.
- Chỉ số đánh giá: F1-Score, AUC-ROC, Precision, Recall.
- Tối ưu lựa chọn mô hình bằng GridSearchCV.

## 📉 Kết quả
- Random Forest cho kết quả tốt nhất với F1 ~ 0.84 và AUC ~ 0.77.
- Phân tích lỗi giúp xác định đặc trưng gây sai lệch như ngày đăng ký gói và ARPU.

# 📈 Bài toán 2: Dự đoán ARPU của khách hàng có doanh thu cao

## 🧠 Mô tả dự án
Xây dựng mô hình hồi quy nhằm **dự đoán doanh thu (ARPU)** của các thuê bao có mức chi tiêu cao. Từ đó hỗ trợ:
- Cảnh báo sớm khách hàng có xu hướng **giảm chi tiêu**.
- Đề xuất gói **ưu đãi phù hợp** dựa trên phân khúc sử dụng.

## 📊 Dữ liệu sử dụng
- ARPU, lưu lượng data, số cuộc gọi của thuê bao trong 5 tháng gần nhất.
- Các cột dạng thời gian: ngày nạp tiền, ngày gọi tổng đài, ngày đăng ký gói gần nhất.
- Dữ liệu gồm hơn 120,000 thuê bao sau khi tiền xử lý.

## ⚙️ Tiền xử lý & Feature Engineering
- Lọc khách hàng có ARPU tháng 7 > 125,000 đồng.
- Xử lý outlier bằng **capping (Q3)**.
- Điền missing bằng **0**,...
- Tạo đặc trưng giảm chi tiêu theo 3 tháng gần nhất (decrease_arpu_123,...).

## 🤖 Mô hình và đánh giá kết quả
Áp dụng 2 mô hình:
- **Random Forest**
- **XGBoost**

| Model         | MAE   | RMSE   | R²     |
|---------------|--------|--------|--------|
| Random Forest | 55.78  | 121.57 | 0.488  |
| XGBoost       | 48.41  | 118.13 | 0.530  |

✅ **XGBoost** cho kết quả tốt hơn với sai lệch thấp hơn và dự đoán sát thực tế.



