# XÂY DỰNG VÀ KIỂM ĐỊNH CHẤT LƯỢNG HỆ THỐNG RELIEFHUB

![Java](https://img.shields.io/badge/Java-21-ED8B00?logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.2.5-6DB33F?logo=springboot&logoColor=white)
![React](https://img.shields.io/badge/React-19.2.7-61DAFB?logo=react&logoColor=black)
![Vite](https://img.shields.io/badge/Vite-8.1.0-646CFF?logo=vite&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-8.0%2B-4479A1?logo=mysql&logoColor=white)
![Coverage](https://img.shields.io/badge/JaCoCo%20Line-78.61%25-blue)
![Tests](https://img.shields.io/badge/Tests-42%2F60%20PASS%20(70%25)-orange)
[![Video Demo](https://img.shields.io/badge/%E2%96%B6%EF%B8%8F%20VIDEO%20THUY%E1%BA%BET%20TR%C3%8CNH%20%26%20DEMO-Google%20Drive-EA4335?style=for-the-badge&logo=googledrive&logoColor=white)](https://drive.google.com/file/d/1jptMBwAwTKpyHUpDQO3YBFIl1DMT4Njv/view?usp=sharing)

> **Học phần:** Đánh giá và Kiểm định Chất lượng Phần mềm  
> **Giảng viên hướng dẫn:** TS. Trịnh Thanh Bình  
> **Nhóm thực hiện:** Nhóm 18 — Nguyễn Thị Thu Giang (`23010871`), Ngô Thị Minh Phương (`23012156`)

> [!IMPORTANT]
> ### 🎥 VIDEO THUYẾT TRÌNH & DEMO KẾT QUẢ KIỂM ĐỊNH (NHÓM 18)
> 👉 **[Xem trực tiếp Video Thuyết trình & Demo Dự án trên Google Drive tại đây](https://drive.google.com/file/d/1jptMBwAwTKpyHUpDQO3YBFIl1DMT4Njv/view?usp=sharing)**  
> *(Link đầy đủ: https://drive.google.com/file/d/1jptMBwAwTKpyHUpDQO3YBFIl1DMT4Njv/view?usp=sharing)*

---

## 1. Giới thiệu dự án

**ReliefHub** là hệ thống quản lý cứu trợ thiên tai hỗ trợ kết nối nhà tài trợ, người dân vùng lũ/thiên tai, ban tài chính, cán bộ địa phương, tình nguyện viên và quản trị viên.

Dự án này tập trung vào **khảo sát kiến trúc, thiết kế bộ kiểm thử và kiểm định chất lượng toàn diện** hệ thống ReliefHub theo chuẩn **IEEE 829** và **ISO/IEC 25010**:
- **Quy mô nghiệp vụ đặc tả:** 6 vai trò xác thực (`ADMIN`, `FINANCE`, `LOCAL`, `DONOR`, `CITIZEN`, `VOLUNTEER`) + `Guest`, **24 Use Case**, **28 yêu cầu hệ thống** (`REQ-001`–`REQ-028`).
- **Luồng cứu trợ 6 trạng thái:** `PENDING → VERIFIED → APPROVED → WAREHOUSE_READY → IN_TRANSIT → DELIVERED` (và `REJECTED`).
- **Kiến trúc 3 tầng:** React WebUI (`:5173`) $\leftrightarrow$ Spring Boot REST API (`:8080/api`) $\leftrightarrow$ MySQL 8.0+ / H2 In-Memory. *(Qua kiểm định phát hiện đứt gãy tích hợp tại `AppContext.jsx` và `api.js` do lỗi `BUG-006`)*.

---

## 2. Công nghệ & Công cụ kiểm định

| Phân lớp | Công nghệ / Công cụ |
|---|---|
| **Frontend (SUT)** | React `19.2.7`, React Router DOM `7.18.0`, Vite `8.1.0`, Tailwind CSS `3.4.19`, Lucide React `1.21.0`, Framer Motion `12.42.0`, Recharts `3.9.0` |
| **Backend (SUT)** | Java `21`, Spring Boot `3.2.5`, Spring Security + JWT (`jjwt 0.12.5`), BCrypt, Spring Data JPA / Hibernate, Springdoc OpenAPI `2.3.0` |
| **Database** | MySQL `8.0+` (45 bảng SQL, 17 `@Entity` JPA đã hiện thực hóa), H2 In-Memory `2.2.224` (`MODE=MySQL`) |
| **Kiểm thử & Phân tích** | JUnit 5 (`5.10.2`), Spring Boot MockMvc, JaCoCo `0.8.12`, Oxlint `1.69.0`, Vite Build, Google Chrome Headless CDP |

---

## 3. Phương pháp & Kết quả kiểm định chất lượng

### 3.1. Phương pháp áp dụng
- **Kiểm thử hộp đen (Black-box):** Phân vùng tương đương (EP), Phân tích giá trị biên (BVA), Bảng quyết định (Decision Table), Chuyển trạng thái (State Transition) và Ma trận truy xuất yêu cầu (RTM).
- **Kiểm thử hộp trắng (White-box):** Lập đồ thị luồng điều khiển (CFG) và tính độ phức tạp Cyclomatic $V(G)$ cho 5 hàm lõi (`AuthService.register` $V(G)=4$, `DonationService.updateDonationStatus` $V(G)=5$, `CampaignService.toResponse` $V(G)=3$, `UserService.changePassword` $V(G)=3$, `FileUploadService.upload` $V(G)=5$).

### 3.2. Kết quả thực thi 60 Test Case

| Hạng mục | Kết quả | Ghi chú |
|---|---:|---|
| **Tổng số Test Case đánh giá** | **60** | 53 Test Case Backend (`TC-001`–`TC-053`) + 7 Test Case WebUI (`TC-054`–`TC-060`) |
| **PASS / FAIL** | **42 PASS (70%) / 18 FAIL (30%)** | Backend: 39 PASS, 14 FAIL \| WebUI: 3 PASS, 4 FAIL \| Blocked/Not Run: 0 |
| **Số lỗi phát hiện** | **12 lỗi** | `BUG-001` đến `BUG-012` (7 lỗi P0, 3 lỗi P1, 2 lỗi P2) |

> **Lưu ý:** Trong mã nguồn gốc hiện có bộ test **Baseline gồm 10 `@Test`** (3 lớp `AuthControllerTest`, `CampaignControllerTest`, `ReliefHubApplicationTests` $\rightarrow$ 10/10 PASS). Bộ 53 kịch bản Backend mở rộng (`ReliefHubFullEvaluationTest`) được nhóm thiết kế trong môi trường kiểm định để không chỉnh sửa mã nguồn gốc của SUT.

### 3.3. Độ bao phủ mã nguồn (JaCoCo 0.8.12)

| Chỉ số | Baseline (10 Test gốc) | Full-Suite (Sau kiểm định) | Tăng trưởng |
|---|---:|---:|---:|
| **Line Coverage toàn Backend** (`jacoco.csv`) | 25.53% *(394/1,543)* | **78.61%** *(1,213/1,543)* | +53.08% |
| **Method Coverage toàn Backend** | 14.33% *(299/2,086)* | **50.53%** *(1,054/2,086)* | +36.20% |
| **Service Line Coverage** (`com.reliefhub.service`) | 14.33% *(93/649)* | **78.43%** *(509/649)* | +64.10% |
| **Service Branch Coverage** (`com.reliefhub.service`) | 6.36% *(15/236)* | **44.07%** *(104/236)* | +37.71% |
| **Service Method Coverage** (`com.reliefhub.service`) | 15.83% *(22/139)* | **58.27%** *(81/139)* | +42.44% |

*(Lưu ý: Giao diện HTML `index.html` của JaCoCo hiển thị Line Coverage toàn dự án đạt **79.51%** do khử trùng lặp dòng nguồn `@Builder` của Lombok).*

### 3.4. Phân tích tĩnh Frontend & Benchmark hiệu năng
- **Oxlint `1.69.0`:** Phát hiện **83 warnings** (`no-unused-vars`) và **36 errors** (`no-undef`), trong đó có **5 lỗi biến nghiệp vụ chưa khai báo** (`API_ENABLED`, `supportRequestService`, `tokenManager`) tại `AppContext.jsx` gây ra `BUG-006`.
- **Vite Build `8.1.0`:** Biên dịch 870 modules; bundle JS chính nặng **`1,214.90 kB`** (gzip: `318.76 kB`), vượt ngưỡng cảnh báo `500 kB`.
- **Hiệu năng ($N = 20$ request độc lập, không phải tải đồng thời):**
  - `GET /campaigns`: `p50 = 4.53 ms`, `p95 = 6.16 ms`
  - `POST /auth/login`: `p50 = 87.83 ms`, `p95 = 177.97 ms`
  - `POST /support-requests`: `p50 = 10.62 ms`, `p95 = 17.57 ms`
  - `PATCH /donations/{id}/status`: `p50 = 12.60 ms`, `p95 = 25.20 ms`

---

## 4. Danh sách 12 lỗi phát hiện (Chưa sửa trên mã nguồn gốc)

| Mã lỗi | Nội dung lỗi phát hiện | Mức độ | Ưu tiên |
|---|---|---:|---:|
| **BUG-001** | Leo thang đặc quyền qua API đăng ký (`POST /auth/register` cho phép tự truyền `roleName: "admin"`) | Critical | **P0** |
| **BUG-002** | Chặn nhầm quyền truy cập hồ sơ cá nhân (`/users/me` trả về HTTP 403 với mọi vai trò ngoài Admin) | High | **P0** |
| **BUG-003** | Thiếu kiểm soát phân quyền RBAC (`@PreAuthorize`) trên các API nghiệp vụ (Donations, Ledger, Inventory...) | Critical | **P0** |
| **BUG-004** | Bỏ qua máy trạng thái trong quy trình cứu trợ 6 bước (nhảy trực tiếp từ `PENDING` sang `DELIVERED`) | High | **P0** |
| **BUG-005** | Cộng dồn trùng lặp số tiền chiến dịch khi xác nhận (`CONFIRMED`) cùng một khoản đóng góp nhiều lần | High | **P0** |
| **BUG-006** | Lỗi tích hợp Frontend – Backend (`AppContext.jsx` thiếu khai báo `API_ENABLED`, chạy mảng tĩnh, mất dữ liệu khi F5) | Critical | **P0** |
| **BUG-007** | Lỗ hổng chiếm đoạt tài khoản qua chức năng Quên mật khẩu trên UI không cần xác thực OTP/Email | Critical | **P0** |
| **BUG-008** | Không kiểm tra đầy đủ định dạng file upload (`POST /uploads` cho phép tải lên file `.html` gây Stored XSS) | High | **P1** |
| **BUG-009** | Lỗ hổng IDOR khi đánh dấu đã đọc thông báo của người dùng khác (`PATCH /notifications/{id}/read`) | Medium | **P1** |
| **BUG-010** | Thiếu kiểm tra dữ liệu biên (số âm/bằng 0) và xử lý dữ liệu sai định dạng gây lỗi HTTP 500 | Medium | **P1** |
| **BUG-011** | Không đồng nhất mật khẩu khởi tạo tài khoản `finance01` giữa SQL/Backend và Frontend | Medium | **P2** |
| **BUG-012** | Nợ kỹ thuật Frontend: 83 cảnh báo biến dư thừa và bundle JS lớn (`1,214.90 kB`) do chưa tách chunk | Low | **P2** |

---

## 5. Cấu trúc thư mục

```text
reliefhub-testing-main/
├── backend/
│   ├── src/main/java/com/reliefhub/   # 15 Controllers, 15 Services, 14 Repositories, 17 Entities, 15 DTOs
│   ├── src/main/resources/            # application.yml, seed_data.sql
│   ├── src/test/java/com/reliefhub/   # 3 lớp test Baseline (10 @Test)
│   ├── reliefhub_full.sql             # Schema 45 bảng, 4 views, 2 procedures + tài khoản gốc
│   └── pom.xml                        # Cấu hình Maven (Java 21, Spring Boot 3.2.5)
├── frontend/
│   ├── src/context/AppContext.jsx     # Quản lý state toàn cục (nơi xảy ra BUG-006)
│   ├── src/services/api.js            # Dịch vụ kết nối REST API & JWT
│   ├── src/pages/                     # 40 trang giao diện theo 6 vai trò + Public
│   └── package.json                   # Cấu hình React 19.2.7, Vite 8.1.0, Oxlint 1.69.0
├── reliefhub_database.sql             # Script SQL tổng hợp (reliefhub_full.sql + seed_data.sql)
└── README.md
```

---

## 6. Hướng dẫn cài đặt và chạy nhanh (Windows PowerShell)

**Yêu cầu:** JDK 21, Maven 3.8+, Node.js 20.19+/22+/24+, MySQL 8.0+.

### Bước 1: Khởi tạo Database MySQL
```powershell
mysql -u root -p -e "CREATE DATABASE IF NOT EXISTS reliefhub CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;"
cmd /c "mysql -u root -p --default-character-set=utf8mb4 reliefhub < reliefhub_database.sql"
```

### Bước 2: Cấu hình & Chạy Backend (`http://localhost:8080/api`)
Cập nhật `username`, `password` MySQL và `app.jwt.secret` trong `backend/src/main/resources/application.yml`, sau đó chạy:
```powershell
cd backend
mvn clean install
mvn spring-boot:run
```
- **Swagger UI:** `http://localhost:8080/api/swagger-ui.html`
- **Chạy bộ test Baseline (H2 In-Memory):** `mvn test`

### Bước 3: Chạy Frontend (`http://localhost:5173`)
```powershell
cd frontend
npm install
npm run dev
```

---

## 7. Đánh giá tổng kết & Thành viên thực hiện

- **Đánh giá mức độ sẵn sàng:** Hệ thống hiện đạt **13/28 yêu cầu hoàn toàn**, còn tồn tại 7 lỗi mức **P0** về phân quyền, bảo mật và đứt gãy tích hợp Frontend – Backend, đồng thời chưa kiểm thử tải đồng thời (100–500 CCU) và chưa quét DAST (OWASP ZAP). Do đó, hệ thống **chưa đủ điều kiện an toàn để vận hành thực tế** khi chưa khắc phục các lỗi nghiêm trọng.

| Thành viên | Mã SV | Nội dung phụ trách |
|---|---|---|
| **Nguyễn Thị Thu Giang** | `23010871` | Backend, CSDL, phân tích CFG & $V(G)$, 53 Test Case Backend, độ bao phủ JaCoCo và hồ sơ lỗi Backend (`BUG-001`–`BUG-005`, `BUG-008`–`BUG-010`) |
| **Ngô Thị Minh Phương** | `23012156` | Kế hoạch kiểm thử IEEE 829, ISO/IEC 25010, RTM 28 yêu cầu, 7 Test Case WebUI (Chrome CDP), Oxlint, Vite Build và hồ sơ lỗi Frontend (`BUG-006`, `BUG-007`, `BUG-011`, `BUG-012`) |
