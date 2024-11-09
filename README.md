# Kho Dữ Liệu (Data Warehouse) - Fashion Ecommerce
Dự án này triển khai và quản lý kho dữ liệu sử dụng các công nghệ SSAS, SSIS và SSRS.

## Mô Tả Dự Án
Dự án này nhằm xây dựng và quản lý một kho dữ liệu sử dụng ba công nghệ chính của SQL Server:
- **SSAS**: Cung cấp các phân tích OLAP (Online Analytical Processing).
- **SSIS**: Quá trình tích hợp dữ liệu từ các nguồn khác nhau vào kho dữ liệu.
- **SSRS**: Xây dựng các báo cáo tùy chỉnh và trực quan hóa dữ liệu.

Kho dữ liệu sẽ lưu trữ và xử lý các thông tin về các đơn hàng, khách hàng, và các giao dịch bán hàng của công ty, giúp hỗ trợ các quyết định kinh doanh dựa trên dữ liệu.
## Thành Phần Chính
1. **SSAS (SQL Server Analysis Services)**:
   - Cấu hình cube và các measures (chỉ số) phục vụ cho phân tích dữ liệu.
   - Xây dựng các dimension (chiều dữ liệu) như khách hàng, sản phẩm, thời gian, v.v.
   - Tạo các measure như tổng doanh thu, số lượng bán, v.v.

2. **SSIS (SQL Server Integration Services)**:
   - Thực hiện các gói ETL (Extract, Transform, Load) để trích xuất dữ liệu từ các hệ thống nguồn và chuyển vào kho dữ liệu.
   - Cấu hình các tác vụ để xử lý, làm sạch và chuyển đổi dữ liệu.

3. **SSRS (SQL Server Reporting Services)**:
   - Xây dựng báo cáo cho người dùng cuối dựa trên các dữ liệu trong kho dữ liệu.
   - Báo cáo có thể bao gồm các bảng, biểu đồ, và báo cáo tùy chỉnh cho các cấp quản lý.

## Cài Đặt và Cấu Hình
1. **Yêu Cầu Hệ Thống**:
   - SQL Server 2022 hoặc phiên bản mới hơn
   - SQL Server Management Studio (SSMS)
   - SQL Server Data Tools (SSDT) cho Visual Studio
   - Truy cập mạng để tải xuống các tài liệu cần thiết

2. **Cài Đặt SSAS**:
   - Cài đặt và cấu hình SQL Server Analysis Services để bắt đầu tạo các mô hình OLAP.
   - Tạo một Cube trong SSAS và xác định các Dimensions và Measures.

3. **Cài Đặt SSIS**:
   - Sử dụng SQL Server Data Tools để phát triển và triển khai các gói ETL.
   - Kết nối tới các nguồn dữ liệu và tải dữ liệu vào các bảng trong kho dữ liệu.

4. **Cài Đặt SSRS**:
   - Cài đặt và cấu hình SQL Server Reporting Services.
   - Phát triển các báo cáo và triển khai trên SSRS.

## Hướng Dẫn Sử Dụng
1. **Sử Dụng SSIS để Tích Hợp Dữ Liệu**:
   - Mở SQL Server Data Tools và chạy các gói SSIS để trích xuất và tải dữ liệu vào kho dữ liệu.
   - Kiểm tra các lỗi trong quá trình ETL và thực hiện các bước sửa lỗi.

2. **Truy Vấn Dữ Liệu từ SSAS**:
   - Mở SQL Server Management Studio và kết nối đến Cube của bạn trong SSAS.
   - Viết các câu truy vấn MDX để truy xuất dữ liệu từ Cube.

3. **Tạo và Xem Báo Cáo trong SSRS**:
   - Mở SQL Server Reporting Services, tạo báo cáo và triển khai chúng lên server.
   - Cấu hình lịch trình và các tham số báo cáo để xuất ra dữ liệu mong muốn.

# Các thành phần SSIS
    - Bảng DIM: DIM_Product, DIM_Customer,DIM_Payment,DIM_Shipement,DIM_Time,DIM_Order,DIM_Event, DIM_Platform
    - Bảng FACT: FACT_Sale, FACT_Behavior
# Các thành phần SSAS
    - Data Sources: SSIS Fashion DW.ds 
    - Data Sources View : SSIS Fashion DW.dsv
    - Cubes : Cube_Sale, Cube_Behavior
    - Dimensions : DIM_Product, DIM_Customer,DIM_Payment,DIM_Shipement,DIM_Time,DIM_Order,DIM_Event, DIM_Platform
# MDX Query 
* 1.Tổng số lượng đơn hàng theo năm
SELECT NON EMPTY
    [DIM Time].[Year].Members ON ROWS,
    [Measures].[FACT Sale Count] ON COLUMNS
FROM [SaleCube];

*  2. Tổng số lượng đơn theo tháng
SELECT NON EMPTY
    [DIM Time].[Month].Members ON ROWS,
    [Measures].[FACT Sale Count] ON COLUMNS
FROM [SaleCube];

* 3.Tổng số lượng đơn hàng theo tháng và năm
SELECT NON EMPTY
    [DIM Time].[Year].Members * [DIM Time].[Month].Members ON ROWS,
    [Measures].[FACT Sale Count] ON COLUMNS
FROM [SaleCube]; 

* 4.Tổng doanh thu theo năm
SELECT NON EMPTY
    [DIM Time].[Year].Members ON ROWS,
    [DIM Order].[Total Amount] ON COLUMNS
FROM [SaleCube]; 

* 5. Tổng phí vận chuyển theo năm
SELECT NON EMPTY
    [DIM Time].[Year].Members ON ROWS,
    [Measures].[Shipment Count] ON COLUMNS
FROM [SaleCube];  

* 6. Tổng số lượng sản phẩm bán ra theo sản phẩm và năm
SELECT NON EMPTY
    [DIM Time].[Year].Members ON COLUMNS,
    [DIM Product].[Product Display Name].Members ON ROWS
FROM [SaleCube]
WHERE [DIM Order].[Total Amount];  

* 7. Tổng số lượng đơn hàng theo từng loại sản phẩm
SELECT NON EMPTY
    [DIM Product].[Article Type].Members ON ROWS, 
    [Measures].[FACT Sale Count] ON COLUMNS 
FROM [SaleCube]; 

* 8. Số lượng khách hàng duy nhất theo giới tính
SELECT 
    [DIM Customer].[Gender].MEMBERS ON ROWS,
    {[Measures].[Distinct Customers]} ON COLUMNS
FROM [SaleCube];

* 9. Đếm số phiên theo sự kiện
SELECT NON EMPTY
    [DIM Event].[Event Name].MEMBERS ON ROWS,
    [Measures].[FACT Behavior Count] ON COLUMNS
FROM [BehaviorCube];

* 10. Đếm số phiên theo nguồn truy cập
SELECT NON EMPTY
    [DIM Platform].[Traffic Source].MEMBERS ON ROWS,
    [Measures].[FACT Behavior Count] ON COLUMNS
FROM [BehaviorCube];

* 11. Số phiên theo sự kiện và nguồn truy cập
SELECT NON EMPTY
    [DIM Event].[Event Name].MEMBERS ON ROWS,
    [DIM Platform].[Traffic Source].MEMBERS ON COLUMNS
FROM [BehaviorCube];

* 12.Số phiên theo sự kiện và nền tảng
SELECT NON EMPTY
    CROSSJOIN([DIM Event].[Event Name].MEMBERS, [DIM Platform].[Traffic Source].MEMBERS) ON ROWS,
    [Measures].[FACT Behavior Count] ON COLUMNS
FROM [BehaviorCube];

* 13.Top 5 sự kiện có số phiên cao nhất
SELECT
    TOPCOUNT([DIM Event].[Event Name].MEMBERS, 5, [Measures].[FACT Behavior Count]) ON ROWS,
    [Measures].[FACT Behavior Count] ON COLUMNS
FROM [BehaviorCube];
* 14.Tỷ lệ đặt chỗ theo từng nguồn truy cập
WITH MEMBER [Measures].[Booking Rate by Source] AS
    IIF(
        [Measures].[FACT Behavior Count] = 0,
        NULL,
        [Measures].[Booking Count] / [Measures].[FACT Behavior Count]
    ),
    FORMAT_STRING = "Percent"
SELECT
    [DIM Platform].[Traffic Source].MEMBERS ON ROWS,
    [Measures].[Booking Rate by Source] ON COLUMNS
FROM [BehaviorCube];

* 15. Tổng số đặt chỗ theo từng nguồn truy cập và sự kiện
SELECT NON EMPTY
    [DIM Platform].[Traffic Source].MEMBERS ON ROWS,
    [DIM Event].[Event Name].MEMBERS ON COLUMNS
FROM [BehaviorCube]
WHERE ([Measures].[Booking Count]);

## Các Tính Năng
- Cung cấp báo cáo tương tác cho người sử dụng cuối.
- Phân tích dữ liệu OLAP với SSAS, cho phép người dùng khám phá và phân tích theo nhiều chiều.

## Các Mở Rộng
- Kết nối kho dữ liệu với các hệ thống bên ngoài như CRM, ERP để lấy dữ liệu.
- Tích hợp với Power BI để tạo các báo cáo và biểu đồ tương tác.

## Tài Liệu Tham Khảo
- Dataset : https://www.kaggle.com/datasets/aldoattallah/fashion-ecommerce/data?select=transactions_test.csv
- [Hướng dẫn SSAS](https://learn.microsoft.com/en-us/sql/analysis-services/)
- [Hướng dẫn SSIS](https://learn.microsoft.com/en-us/sql/integration-services/)
- [Hướng dẫn SSRS](https://learn.microsoft.com/en-us/sql/reporting-services/)
