# ĐẾN TUẦN HỌC 4, LỚP 59KMT ĐÃ HỌC:
1. cách cài đặt sql server trên windows
2. cách cấu hình tcp/port để mở cổng động (dinamic port)
3. cách kiểm tra xem sql server có thực sự đang running và listening port đã cấu hình hay ko.
4. cách tạo 1 db mới, cách phân quyền cho 1 user mới, với pw, sau khi login chỉ thao tác đc full quyền với db mới này.
5. cách backup, restore, sinh ra code sql tái hiện lại toàn bộ csdl.
6. dạy về các kiểu dữ liệu trong sql server: tên gọi, miền giá trị, khi nào dùng kiểu nào
7. dạy về quy tắc đặt tên bảng, đặt tên trường dữ liệu, cách dùng [ ] để bọc tên bảng, tên trường trong sql.
8. giới thiệu về một số function có sẵn, store procedure có sẵn
9. dạy về cách viết function trả về 1 giá trị, trả về bảng dữ liệu - dạng inline, và hàm trả về bảng dữ liệu kiểu multi statements.
10. dạy về cách viết store procedure: loại sp_ ko trả về dữ liệu, loại sp_ trả về thông qua tham số loại output, và loại sp_ trả về dữ liệu của lệnh select bên trong sp đó.
11. dạy về cách dùng con trỏ (CURSOR) trong sql server thông qua ví dụ kinh điển về bài toán bán hàng: tạo trigger để tự động cập nhật số lượng tồn của hàng hoá liên quan và tổng tiền của hoá đơn liên quan.

## Bài kiểm tra số 1: (đã kiểm tra các kiến thức 1..5) 

# Bài kiểm tra số 2: (Sẽ kiểm tra các kiến thức 6..11)

## YÊU CẦU CHUNG CỦA BÀI TẬP 02

1. **Nội dung:**

  - Sinh viên thực hiện các yêu cầu dưới đây trên hệ quản trị SQL Server. Mỗi bước thực hiện cần chụp ảnh màn hình (screenshot) minh họa
  - Ảnh chụp phải bao gồm cả mã SQL và kết quả thực thi.
  - Mỗi ảnh phải có chú thích rõ ràng: Ảnh này làm gì? Lệnh SQL đó giải quyết vấn đề gì? Kết quả nói lên điều gì?

2. **Sản phẩm nộp:**
  - Đẩy lên repository GitHub cá nhân (để chế độ Public). Repository gồm 2 file:
  - README.md: Chứa nội dung bài làm, các đoạn code và các ảnh chụp minh họa (dùng cú pháp Markdown để chèn ảnh).
  - baikiemtra2.sql: File script chứa toàn bộ mã nguồn SQL đã viết.

3. **Hình thức chấm:**
  - Giảng viên sẽ kiểm tra tính đúng đắn của logic, cách đặt tên theo quy chuẩn và đối soát thời gian lưu vết trên GitHub (Commit history).

4. **Deadline:** 23:59:59 ngày 03 tháng 5 năm 2026. (hạn rất dài, muộn là quá lười)

## NỘI DUNG YÊU CẦU (GỒM 5 PHẦN):

### Phần 1: Thiết kế và Khởi tạo Cấu trúc Dữ liệu (Kiến thức 6, 7) 
SV: Vi Hồng Mai. K235480106049

Trong bài tập lớn này, em chọn đề tài QUẢN LÝ PHÒNG TRỌ. Để giải quyết bài toán một cách hệ thống, em thực hiện theo các bước:

  + Xây dựng nền tảng: Thiết kế Database chuẩn hóa với các ràng buộc cứng (CHECK, PRIMARY KEY, FOREIGN KEY) để đảm bảo dữ liệu "sạch" ngay từ đầu.

  + Tối ưu hóa bằng Function: Tận dụng hàm hệ thống và xây dựng hàm tự định nghĩa để tái sử dụng các công thức tính toán phức tạp (như tính tiền phòng theo số ngày).

  + Điều khiển nghiệp vụ bằng SP & Trigger: Sử dụng Stored Procedure để đóng gói các thao tác thêm/sửa/xóa và Trigger để tự động hóa việc ghi nhật ký hoặc cập nhật dữ liệu chéo.

  + Phân tích hiệu năng: So sánh giữa xử lý tập hợp (Set-based) và xử lý tuần tự (Cursor) để hiểu rõ khi nào cần ưu tiên tốc độ, khi nào cần sự linh hoạt.

1. Sinh viên tự chọn một chủ đề quản lý (Ví dụ: Quản lý thư viện, Quản lý khách sạn, hoặc Quản lý dự án, HOẶC BẤT KỲ BÀI TOÁN QUẢN LÝ NÀO KHÁC).

    <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2878f067-2ef9-48d2-8856-656cff23981b" />

     Database mới với tên [QuanLyTro.K235480106049].

    
    <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d61b7850-18da-486b-b6e2-eaf7d5db5b49" />

     Gồm 4 bảng có quan hệ với nhau.
   

  + Sử dụng đa dạng các kiểu dữ liệu (Số nguyên, số thực, chuỗi ký tự Unicode, ngày tháng, tiền tệ, ...)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bc343fc0-f1e7-410f-8a15-99750dfb65dd" />



Kiểu dữ liệu: Đa dạng gồm INT (Mã), NVARCHAR (Tên tiếng Việt), MONEY (Giá phòng), DATE (Hợp đồng), FLOAT (Diện tích).

 


  + Áp dụng đúng quy tắc đặt tên (BướuLạcĐà).
   <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1e53d77e-7e31-4d19-9fee-ea49c5237b29" />

   VD:Như trên ảnh MaHopDong đã được đặt tên theo đúng quy tắc này.
 

  + Sử dụng cặp ngoặc [ ] để bọc tên bảng và tên trường trong script khởi tạo.
    <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1cf449f9-4a60-4ac9-994f-25bc0985ff70" />


  + Có giải thích chỗ nào là PK, chỗ nào là FK, trường nào có ràng buộc cứng CK (ví dụ điểm từ 0..10),...
    
   QuanLyPhongTro_K235480106049], các quy tắc logic được thiết lập như sau:
   
1. Khóa chính (PK - Primary Key)
• Vị trí: [MaLoaiPhong], [MaKhachThue], [MaPhong], [MaHopDong].
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e1ac4ea5-a141-4db1-a8b3-c82dcdebf045" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6c7fcf51-b92d-4859-9886-6e7b699e9eaa" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/03611f4b-030e-4717-9731-7d07b1c167c3" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e185498b-c6be-4639-8a4e-29c7e0836b73" />




3. Khóa ngoại (FK - Foreign Key)
• Vị trí:
• [MaLoaiPhong] (trong bảng PhongTro): Nối sang bảng [LoaiPhong].
• [MaPhong] (trong bảng HopDong): Nối sang bảng [PhongTro].
• [MaKhachThue] (trong bảng HopDong): Nối sang bảng [KhachThue]
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0ee83cc6-76f2-447e-8c0d-df06bf312d38" />
 Sơ đồ thực thể quan hệ thể hiện các Khóa ngoại (FK) nối giữa bảng LoaiPhong - PhongTro và PhongTro - HopDong."



### Phần 2: Xây dựng Function (Kiến thức 8, 9) 


1. Hãy cho biết trong SQL Server có những loại function build_in (hàm có sẵn) nào, nêu 1 vài system function build_in mà em tìm hiểu được (ko cần nhiều, cần đặc sắc theo góc nhìn của em), cho SQL khai thác các hàm đó.
    SQL Server cung cấp hàng trăm hàm chia thành các nhóm chính sau:
    
+ String Functions (Hàm chuỗi): Xử lý văn bản (Vd: LEN, UPPER, REPLACE, SUBSTRING).
+ Math Functions (Hàm toán học): Tính toán số học (Vd: ROUND, ABS, RAND).
+ Date and Time Functions (Hàm ngày tháng): Xử lý thời gian (Vd: GETDATE, DATEDIFF, DATEADD).
+ Aggregate Functions (Hàm tập hợp): Tính toán trên một tập dữ liệu (Vd: SUM, AVG, COUNT, MAX, MIN).
+ Conversion Functions (Hàm chuyển đổi): Chuyển đổi kiểu dữ liệu (Vd: CAST, CONVERT).

  Các hàm System mà e đã tìm hiểu đc:
 
+ ISNULL(check_expression, replacement_value): Kiểm tra nếu dữ liệu bị trống (NULL) thì thay bằng một giá trị khác. Rất quan trọng khi tính tiền để tránh bị lỗi phép tính.

+ FORMAT(): Hàm này cực kỳ mạnh mẽ để định dạng dữ liệu theo chuẩn địa phương. Ví dụ: biến một con số thành định dạng tiền tệ Việt Nam (VNĐ).

+ DATEDIFF(interval, start, end): Tính toán khoảng cách giữa hai mốc thời gian. Cực kỳ hữu ích để tính số tháng khách đã ở hoặc số ngày quá hạn tiền nhà

   <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/76b487fb-93d9-4687-977a-0213ca6fdcd1" />



 2. Hàm do người dùng tự viết trong SQL thường mang mục đích gì? Nó có những loại nào? Mỗi loại thường được dùng khi nào? Tại sao có nhiều system function rồi mà vẫn cần tự viết fn riêng?

  Mục đích:
    
    + Tính tái sử dụng: Thay vì phải viết đi viết lại một công thức tính tiền cọc phức tạp ở 10 trang báo cáo khác nhau, bạn chỉ cần viết một cái hàm và gọi tên nó ra.
    
    + Đơn giản hóa câu lệnh SQL: Biến một đoạn code dài dằng dặc thành một cái tên hàm ngắn gọn, giúp code sạch sẽ và dễ đọc hơn.
    + Tính nhất quán: Khi công thức tính tiền thay đổi, bạn chỉ cần sửa ở 1 nơi (trong hàm), tất cả các báo cáo khác sẽ tự động cập nhật theo

    "Trong hệ thống quản lý trọ, việc tính toán tiền phòng tháng đầu rất dễ nhầm lẫn. Vì vậy, chúng tôi xây dựng hàm fn_TongTienNopDauVao để chuẩn hóa công thức này."

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9a183e9f-f642-4049-ac3d-0b9002a01cbb" />


  có 3 loại:

    Scalar Function (Hàm vô hướng)
    • Đặc điểm: Trả về duy nhất 01 giá trị (có thể là số, chuỗi ký tự, hoặc ngày tháng).
    • Khi nào dùng: * Thực hiện các phép tính toán đơn giản dựa trên đầu vào (ví dụ: tính tổng tiền, tính thuế, tính tuổi).
    • Chuẩn hóa dữ liệu (ví dụ: chuyển tên thành viết hoa, cắt tỉa khoảng trắng).

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4c748bba-7155-425e-bd5c-a87f8bd1b59c" />


Inline Table-Valued Function (Hàm trả về bảng đơn giản)
  • Đặc điểm: Trả về kết quả là một tập dữ liệu (bảng). Cấu trúc của hàm này cực kỳ gọn nhẹ, bên trong chỉ chứa duy nhất một câu lệnh SELECT.
  • Khi nào dùng: * Dùng để lọc dữ liệu nhanh chóng theo tham số truyền vào.
  • Nó hoạt động giống như một cái "View động" có thể nhận tham số.
  • Loại này có hiệu suất (performance) tốt nhất vì SQL Server tối ưu nó rất tốt

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1e44d96c-920b-4c9c-829e-062b02952bd5" />



Multi-statement Table-Valued Function (Hàm trả về bảng phức tạp)
  - Đặc điểm: Cũng trả về một bảng, nhưng khác ở chỗ nó có cấu trúc phức tạp hơn. Bạn phải khai báo một biến bảng tạm thời, sau đó dùng các lệnh INSERT để đổ dữ liệu vào bảng đó trước khi trả về kết quả cuối cùng.
  - Khi nào dùng: * Khi logic để lấy dữ liệu quá phức tạp, không thể viết gọn trong 1 câu SELECT.
  - Cần sử dụng các cấu trúc điều khiển như IF...ELSE, WHILE hoặc thực hiện nhiều bước tính toán trung gian trước khi ra kết qủa

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/76c7c977-2369-43cd-96a8-1e218ef740f2" />


Dù SQL cung cấp sẵn rất nhiều hàm (như SUM, GETDATE, UPPER), UDF vẫn không thể thiếu vì các lý do sau:

  - Logic nghiệp vụ riêng biệt (Domain Logic): System functions chỉ xử lý các tác vụ chung. Hệ thống của bạn có cách tính điểm ưu tiên cho khách hàng thân thiết rất "quái chiêu" mà không ngôn ngữ SQL nào có sẵn hàm TINH_DIEM_UU_TIEN() cho bạn cả. Đơn giản hóa câu lệnh 

  - Query: Một câu SELECT chứa hàng chục phép toán lồng nhau sẽ cực kỳ khó đọc. Đưa logic đó vào hàm giúp câu query chính trông sạch sẽ và dễ bảo trì hơn.

  - Tính nhất quán: Nếu công thức tính lương thay đổi, bạn chỉ cần sửa trong 1 hàm duy nhất thay vì đi lùng sục hàng trăm file code hay store procedure để sửa thủ công.

  - Bù đắp giới hạn của View: Như đã nói, UDF có thể nhận tham số, giúp nó linh hoạt hơn View rất nhiều trong việc lọc dữ liệu động.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f39875bd-4f62-44a2-80ac-c8c90075460a" />

Ảnh trên đã minh chứng sự khác biệt giữa System Functions (xử lý toán học/chuỗi cơ bản) và User-Defined Functions (xử lý logic nghiệp vụ quản lý trọ phức tạp).

 
 3. Viết 01 Scalar Function (Hàm trả về một giá trị): Đưa ra 1 logic cho cơ sở dữ liệu của em, mà cần dùng đến function này. (SV TỰ NGHĨ RA YÊU CẦU CỦA HÀM VÀ VIẾT HÀM GIẢI QUYẾT NÓ)

Sau khi đã có hàm, viết câu lệnh sql khai thác hàm đó.
    
  Yêu cầu của hàm (Logic nghiệp vụ tự nghĩ ra)

    Bài toán: Mỗi khi có khách mới thuê phòng, chủ trọ cần biết tổng số tiền khách phải nộp ngay lúc đó. Số tiền này bao gồm: Giá thuê phòng (lấy từ bảng Loại phòng thông qua bảng Phòng trọ) cộng với Số tiền cọc thực tế mà khách đưa.
    Tại sao cần Function? Vì giá phòng nằm ở bảng LoaiPhong, mã phòng nằm ở bảng PhongTro, còn tiền cọc là giá trị biến động theo từng hợp đồng. Việc viết hàm giúp chuẩn hóa công thức này, tránh tính toán sai sót thủ công.

  Tạo hàm: Hàm này nhận vào 2 tham số:@MaPhong và @TienCoc, sau đó trả về gia giá trị duy nhất.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/41abe883-e024-4deb-ab7d-b8c564f51378" />

Viết hàm giải quyết:
  TH1: Tính cho một phòng cụ thể với số tiền cọc cụ thể.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ed222dbf-815b-4358-ba97-7f3c30331696" />

  TH2: TRuy vấn danh sách tất cả các phòng và dự toán tiền thu( khách cọc 1 tháng)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/444be9de-f1e4-4c99-afaa-03e41fe8a4f1" />


4. Viết 01 Inline Table-Valued Function: Trả về danh sách các bản ghi theo một điều kiện lọc cụ thể (SV TỰ NGHĨ RA YÊU CẦU CỦA HÀM VÀ VIẾT HÀM GIẢI QUYẾT NÓ)

Sau khi đã có hàm, viết câu lệnh sql khai thác hàm đó.

Yêu cầu của hàm (Logic nghiệp vụ tự nghĩ ra)
  Bài toán: Chủ trọ thường xuyên cần kiểm tra danh sách các phòng thuộc một Loại phòng nhất định (ví dụ: tìm tất cả "Phòng VIP" hoặc "Phòng giá rẻ") để tư vấn cho khách.
  Tại sao cần Function? Thay vì mỗi lần tìm lại phải viết câu lệnh JOIN phức tạp giữa bảng PhongTro và LoaiPhong, ta tạo một hàm nhận vào Tên loại phòng và trả về toàn bộ danh sách phòng tương ứng kèm theo giá cả và trạng thái

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7ae3a0b8-5f60-45ca-b8bf-aff2765e73de" />


  <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a3b94cd4-c00a-4b78-89b9-9a8debf57648" />




5. Viết 01 Multi-statement Table-Valued Function: Thực hiện xử lý logic phức tạp bên trong (có sử dụng biến bảng) trước khi trả về kết quả. (SV TỰ NGHĨ RA YÊU CẦU CỦA HÀM VÀ VIẾT HÀM GIẢI QUYẾT NÓ)

    Sau khi đã có hàm, viết câu lệnh sql khai thác hàm đó.

Yêu cầu của hàm (Logic nghiệp vụ tự nghĩ ra)
  Bài toán: Chủ trọ muốn có một báo cáo "Phân loại diện tích phòng".
  Nếu phòng dưới 20m², phân loại là "Phòng Nhỏ".
  Nếu phòng từ 20m² đến 30m², phân loại là "Phòng Vừa".
  Nếu phòng trên 30m², phân loại là "Phòng Rộng".
  Tại sao cần Multi-statement? Vì ta cần duyệt qua từng dòng dữ liệu, dùng cấu trúc điều khiển (logic phân loại) để gán nhãn cho từng phòng trước khi xuất ra danh sách cuối cùng.
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c322c4ae-0dc6-47bb-bf1a-5e880e836620" />


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3f253beb-71c6-4cf4-a123-1f36de4c3c75" />



### Phần 3: Xây dựng Store Procedure (Kiến thức 10) 

1. System Stored Procedures là các thủ tục được cài đặt sẵn cùng với SQL Server, thường được lưu trữ trong Database hệ thống master hoặc msdb, nhưng có thể gọi được từ bất kỳ Database nào của người dùng. Đặc điểm nhận dạng của các thủ tục này là thường bắt đầu bằng tiền tố sp_. Chúng giúp thực hiện các nhiệm vụ từ quản trị bảo mật, kiểm tra cấu trúc đối tượng, đến theo dõi hiệu suất hệ thống.

  Dưới đây là một vài thủ tục quan trọng mà bạn có thể tìm hiểu và ứng dụng ngay trong đồ án

    • sp_help: Đây là thủ tục "vạn năng" để xem thông tin chi tiết. Khi bạn truyền tên của một bảng, View hoặc Function vào, SQL Server sẽ trả về toàn bộ thông tin về các cột, kiểu dữ liệu, các khóa chính (Primary Key), khóa ngoại (Foreign Key) và các ràng buộc đi kèm.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d733a86c-0937-4644-9baf-441c3544a154" />

  KQ: Trả về nhiều bảng dữ liệu nhỏ, liệt kê: Tên cột, kiểu dữ kiệu( int, nvarchar...,) độ dài, các khóa chính/ khóa ngoại.

  • sp_helptext: Thủ tục này cực kỳ hữu ích khi bạn muốn xem lại nội dung mã nguồn của một đối tượng (như Function, View, hoặc Procedure khác) đã được lưu trong cơ sở dữ liệu. Nó sẽ hiển thị toàn bộ đoạn code CREATE mà bạn đã thực thi trước đó.
  
  • Cách dùng: EXEC sp_helptext 'Ten_Doi_Tuong';

  <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d2ee0aad-b0e9-4f18-a013-1d71869ac4ad" />



    • sp_rename: Đúng như tên gọi, thủ tục này dùng để đổi tên các đối tượng (bảng, cột, chỉ mục) trong Database hiện hành. Việc sử dụng thủ tục này được khuyến khích thay vì cố gắng sửa tên bằng giao diện đồ họa để đảm bảo tính đồng bộ.

    • Cách dùng: EXEC sp_rename 'Ten_Cu', 'Ten_Moi';

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4ebbfd31-9523-477d-b32e-89b245e8926d" />

    • sp_who: Cung cấp thông tin về các người dùng và các tiến trình (sessions) hiện đang kết nối vào SQL Server. Nó giúp bạn biết được ai đang truy cập vào Database của mình và trạng thái hoạt động của họ ra sao.
  
    • Cách dùng: EXEC sp_who;
  
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fa914149-f6eb-481c-b16c-bc0096399417" />

    • sp_databases: Liệt kê danh sách tất cả các Database hiện có trên máy chủ SQL Server đó, kèm theo thông tin về kích thước của từng Database.

    • Cách dùng: EXEC sp_databases

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/45e3ab60-31d8-40f8-bd60-79246c9e5225" />


2. Viết 01 Store Procedure đơn giản để thực hiện lệnh INSERT hoặc UPDATE dữ liệu, có kiểm tra điều kiện logic (SV TỰ NGHĨ RA YÊU CẦU CỦA SP VÀ VIẾT SP GIẢI QUYẾT NÓ)

  "Thêm mới một phòng trọ (PhongTro). Nếu mã phòng đã tồn tại thì cập nhật thông tin, nếu chưa có thì thêm mới. Tuy nhiên, đơn giá (DonGia) không được phép nhỏ hơn hoặc bằng 0."

   <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1fa01c22-4c0f-4ae9-9c06-2f7fccb187b4" />
   
   <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6df6322f-adaa-41d5-ab76-119d0f794ae5" />

3. Viết 01 Store Procedure có sử dụng tham số OUTPUT để trả về một giá trị tính toán (SV TỰ NGHĨ RA YÊU CẦU CỦA SP VÀ VIẾT SP GIẢI QUYẾT NÓ, SP NÀY CÓ DÙNG THAM SỐ LOẠI OUTPUT

"Tính tổng doanh thu dự kiến của một loại phòng cụ thể (ví dụ: loại phòng 'VIP'). Kết quả tổng tiền sẽ được trả về thông qua một tham số OUTPUT để có thể tái sử dụng ở các script khác."

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bfdd6266-8155-4e22-b97d-0ef3cfb8ce55" />

4. Viết 01 Store Procedure trả về một tập kết quả (Result set) từ lệnh SELECT sau khi đã join nhiều bảng. (SV TỰ NGHĨ RA YÊU CẦU CỦA SP VÀ VIẾT SP GIẢI QUYẾT NÓ)

   Thống kê danh sách hợp đồng thuê phòng đang còn hiệu lực. Người quản lý cần một danh sách tổng hợp để biết khách nào đang ở phòng nào, thuộc loại phòng gì và giá thuê bao nhiêu. Dữ liệu cần được kết hợp từ 4 bảng: KhachThue, HopDong, CanHo, và LoaiPhong.

   <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d1c81095-56e3-4212-97da-cd5696aa7c5b" />



### Phần 4: Trigger và Xử lý logic nghiệp vụ (Kiến thức 11)

  1. Viết 01 Trigger để tự động làm gì đó tại 1 bảng B khi mà dữ liệu thay đổi dữ liệu ở bảng A. Logic giải quyết do sv tự nghĩ ra, sao cho thực tế và thuyết phục.

    Bài toán đặt ra: Tự động cập nhật số điện thoại của khách hàng trên tất cả các hợp đồng khi thông tin khách hàng thay đổi

    Yêu cầu: Khi cập nhật số điện thoại (SoDienThoai) của một khách hàng trong bảng KhachThue (Bảng A), một bảng nhật ký chỉnh sửa LichSuThayDoi (Bảng B) sẽ tự động ghi lại thông tin này để quản lý.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4a392cb1-8067-4bf3-a9d6-dcf0b3e296c6" />


  2.  Thử viết Trigger cho Bảng A : Khi insert thì cập nhật dữ liệu vào bảng B; sau đó viết trigger cho bảng B để khi B được cập nhật thì cập nhật sang bảng A : Quan sát các thông báo (nếu có) của hệ thống, giải thích các thông báo đó (nếu có). Đưa ra nhật xét cuối cùng về tình trạng này.

    
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/220ebeb4-d137-49bc-9d19-3531214ef2de" />

  - Hiện tượng lặp: Các dòng "Trigger A/B đang chạy" xuất hiện liên tục cho thấy Trigger của bảng này đang gọi Update bảng kia và ngược lại.

  - Lỗi Msg 2628 (Truncated value): Trong code của bạn, Trigger B có lệnh: SET HoTen = HoTen + ' (Updated)'.

  - Mỗi lần vòng lặp chạy, tên khách hàng lại dài thêm 10 ký tự. Chỉ sau vài vòng, độ dài chuỗi đã vượt quá giới hạn khai báo của cột HoTen (thường là NVARCHAR(100)), dẫn đến việc SQL Server chặn lại vì dữ liệu bị tràn.

  - Lỗi Msg 217 (Nesting level - Nếu không bị tràn chuỗi): Nếu cột tên của cực kỳ dài, hệ thống sẽ chạy đến khi đạt mức 32 cấp lặp chồng nhau rồi mới tự ngắt.

### Phần 5: Cursor và Duyệt dữ liệu (Kiến thức 11)

  + Viết một đoạn script sử dụng CURSOR để duyệt qua danh sách của 1 câu lệnh SQL dạng SELECT, duyệt qua từng bản ghi, xử lý riêng từng bản ghi (THEO LOGIC SV TỰ ĐẶT RA: SAO CHO HỢP LÝ VÀ THUYẾT PHỤC)

    Bài toán: Duyệt qua danh sách các phòng, nếu giá phòng > 4,000,000 thì in ra thông báo "Phòng cao cấp", ngược lại in ra "Phòng bình dân".

    <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/49f2b556-404c-46ca-a12c-d7638c4afe03" />


  + Tìm cách không sử dụng CURSOR để giải quyết bài toán mà em đã dùng CURSOR mới giải quyết được ở trên. thử so sánh tốc độ giữa có dùng cursor và không dùng cursor (nếu cùng kết quả) thì thời gian xử lý cái nào nhanh hơn, cần ảnh chụp màn hình minh chứng.

    <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/458f72de-59ad-4217-b532-259d475c0f2a" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bbd2769d-141a-4efc-a8b4-656a8c13198f" />

"như trên hình, Trong thực tế, các câu lệnh không dùng Cursor (Set-based) thường nhanh hơnso với dùng Cursor khi làm việc với lượng dữ liệu lớn."


3. Nếu vẫn tìm được cách dùng SQL để giải quyết vấn đề mà ko cần CURSOR: thử nghĩ bài toán khác, mà chỉ CURSOR mới giải quyết được, còn SQL rất khó giải quyết đc (theo logic suy nghĩ của em)
   
  Tự động tạo Tài khoản đăng nhập (Username) cho Khách thuê
  bài tập: Bạn mới nhập một danh sách 100 khách thuê vào bảng KhachThue. Bây giờ bạn cần tạo cho mỗi người một Username riêng biệt trong bảng TaiKhoan dựa trên tên của họ (ví dụ: Nguyễn Văn A -> anv_2026).
  
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e8ae0735-c54b-4409-bfe3-5b29dd4ae378" />

  * Tại sao chỉ CURSOR mới giải quyết tốt: * Nó cho phép em kiểm soát từng bước: Với mỗi khách hàng, em có thể in ra thông báo trạng thái (PRINT) hoặc thực hiện các lệnh kiểm tra lồng nhau (IF...ELSE) cực kỳ chi tiết mà lệnh INSERT tập hợp không thể hiển thị báo cáo từng dòng như vậy được.

Nó tách biệt quá trình xử lý: Nếu dòng số 5 bị lỗi, em có thể dùng TRY...CATCH bên trong Cursor để bỏ qua dòng đó và tiếp tục xử lý dòng số 6. Lệnh SQL tập hợp nếu lỗi 1 dòng sẽ văng lỗi cả tập hợp (Rollback




## HƯỚNG DẪN TRÌNH BÀY TRÊN GITHUB
Sinh viên trình bày file README.md theo cấu trúc:

Phần mở đầu: Thông tin cá nhân, yêu cầu đầu bài, giới thiệu sơ qua về cách làm.

Phần 1: Khởi tạo bảng

[Mô tả logic]

[Code SQL]

![Ảnh chụp màn hình kết quả]

Chú thích: Ảnh này cho thấy tôi đã tạo thành công 3 bảng với các kiểu dữ liệu đúng yêu cầu...

Phần 2: ... (Tương tự cho các câu tiếp theo, mỗi phần có thể có nhiều ảnh, mỗi ảnh hãy gõ thêm chú thích)

...

Lưu ý: Mọi hành vi sao chép code hoặc ảnh chụp của người khác sẽ bị phát hiện tự động và nhận điểm 0 (cấm thi) theo quy định của môn học.

code sql lưu vết các demo trên lớp được gửi kèm trong repo này, readme.md chứa bài tập 1 được đổi tên thành bai_tap_01.md để lưu trữ, readme được tạo mới với nội dung bài tập 2. 

sv làm bài xong cập nhật link repo (public access able) vào file sau: https://docs.google.com/spreadsheets/d/1iwHJ6qSFKkS3iUjtlbCxw_0jUWC56PnCso1kcgxOEas/edit?usp=sharing  (nhớ dùng tài khoản @tnut)
