# Analyzing Job Market Data in Power BI
***Tình huống***: Công ty ảo DataSearch - là một công ty tuyển dụng - yêu cầu nhân viên tìm ra xu hướng công việc với dữ liệu được cung cấp. Nhiệm vụ của nhân viên là khám phá xu hướng của các vị trí công việc và kỹ năng tronng lĩnh vực Khoa học dữ liệu.

Mô tả dữ liệu - "job_posting": gồm các yếu tố quan trọng trong các bài đăng tuyển dụng từ năm 2017 - 2021 ở một số lĩnh vực gồm 18 cột. Dưới đây là một số cột cần thiết trong dự án:
- Job Posting ID: ID của bài đăng
- Job Posting Date: ngày đăng tin
- Job Title: vị trí công việc (như Data Analyst, Software Engineer, ...)
- Job Position Level: cấp độ công việc (Entry level, Associate, ...)
- Years of Experience: số năm kinh nghiệm
- Job Skills: kỹ năng cần có trong công việc
- Minimum Pay: mức lương tối thiểu
- Maximum Pay: mức lương tối đa
- Company Name: tên công ty
- Company Industry: Lĩnh vực hoạt động
- Company Size: quy mô công ty

Trước tiên, ta cần thực hiện Exploratory data analysis (EDA). Phạm vi phân tích bao gồm:
- Kỹ năng phân tích dữ liệu nào có liên quan đến tin tuyển dụng?
- Các công việc phổ biến nhất trong lĩnh vực khoa học dữ liệu?
- Số năm kinh nghiệm được yêu cầu theo từng cấp độ?

## Bước 1: Kiểm tra dữ liệu
Kiểm tra các giá trị bị khuyết.

![alt](https://user-images.githubusercontent.com/105619352/228890834-65d2a59e-32ea-402f-a33b-09de51fd9eb7.png)

Ta thấy rằng trong 1000 dòng trên cùng, có đến 92% giá trị khuyết ở cột Minimum và Maximum Pay.
Điều này có nghĩa là các công ty không ghi ra số lương cụ thể.
## Bước 2: Khám phá dữ liệu
Tiếp theo, ta thay đổi số dòng thành toàn bộ dòng trên dữ liệu (ở góc dưới bên trái trong Power Query Editor). Sau đó, trên thanh công cụ (ribbon) → Chế độ Xem → Cấu hình dạng cột.

![alt](https://user-images.githubusercontent.com/105619352/228891529-83873316-0f82-4559-92f9-1ea1f7c4c7b0.png)

Ta thấy rằng ngành công nghiệp Internet chiếm nhiều nhất trong các bản tin tuyển dụng, kế đến là Phần mềm máy tính.

Tạo một trang báo cáo phân tích tên là “Job Level Analysis”.

Tạo biểu đồ cột cho thấy Số năm kinh nghiệm trung bình của từng cấp độ.

![alt](https://user-images.githubusercontent.com/105619352/228891590-024225f7-6b2b-451a-9b0e-69d7802aaa26.png)

Một điều khá hiển nhiên là cấp độ càng cao thì số năm kinh nghiệm cũng càng cao.

Sau đó, tạo một biểu đồ vùng thể hiện tổng số tin tuyển dụng theo từng năm ở mỗi cấp độ.

![alt](https://user-images.githubusercontent.com/105619352/228891621-11e616ab-a1e0-4195-b031-7be9e651e4d7.png)

Dựa vào biểu đồ ta thấy rằng vào tháng 3 năm 2020 thì số tin tuyển là ít nhất.

Tạo một trang mới tên “Job Title”. 

Vẽ biểu đồ cây cho thấy tỉ trọng của chức danh nào là nhiều nhất trong số: Data Engineer, Data Analyst, Data Scientist, Data Science Manager, Machine Learning Engineer.

![alt](https://user-images.githubusercontent.com/105619352/228891646-829baa60-6a35-478a-b8ff-934d101b96a4.png)

Tiếp theo tạo một trang báo cáo mới tên là “Salary Analysis”.

Ước tính lương trung bình bằng cách tạo cột tính toán “Average Pay” dựa trên trung bình cộng của Min và Max pay.

Tạo một bảng tên là "\_Measure" để tạo độ đo mới “Average of Average Pay” để tính trung bình lương.

Sau đó tạo biểu đồ đường như sau:
![alt](https://user-images.githubusercontent.com/105619352/228896180-f30e8de0-8bcd-43fc-9b78-5a309e0da350.png)

Quan sát biểu đồ, ta thấy rằng Data Analyst là chức danh được trả lương thấp nhất theo kinh nghiệm.

### Kết luận
- Lương thường không được đề cập rõ ràng
- Số năm kinh nghiệm tăng thì mức lương tăng
- Cấp độ càng cao số năm kinh kiệm cũng càng cao
- Data Engineer là vai trò phổ biến và có nhu cầu cao trong lĩnh vực Khoa học dữ liệu
- Số lượng tuyển dụng có xu hướng tăng trong 5 năm qua (ngoại trừ giai đoạn 2020), đây là một điều tốt cho DataSearch
- Các ngành công nghệ có nhu cầu về Khoa học dữ liệu nhất

## Bước 3: Phân tích và trực quan hóa
Tạo một bảng mới như sau:
![alt](https://user-images.githubusercontent.com/105619352/228897308-7e6dfd68-e306-46e7-9479-5f22764d3696.png)

Job skills được tách từ cột Job Skills của bảng Job Posting.

Vẽ biểu đồ trực quan:

![alt](https://user-images.githubusercontent.com/105619352/228897328-b752dce0-abbe-4580-8833-ab11714bce22.png)

Như vậy, Python và SQL là những kỹ năng cần thiết được doanh nghiệp yêu cầu nhiều nhất.

Tiếp theo, ta sẽ dùng DAX để hiểu rõ hơn về xác suất mà một kỹ năng nhất định sẽ được liệt kê trong tin tuyển dụng.

Tạo các DAX sau:
- Đếm số kỹ năng
- Đếm số tin tuyển dụng
- Xác suất kỹ năng xuất hiện trong 1 tin tuyển dụng

Sau đó tạo ma trận:
![alt](https://user-images.githubusercontent.com/105619352/228897344-b55b29ef-4754-47ff-bf24-b973672605d3.png)

Tiếp theo, ta khám phá xu hướng kỹ năng theo thời gian.

Tạo biểu đồ đường để thể hiện xu hướng xác suất kỹ năng xuất hiện trong một tin tuyển dụng theo thời gian. Vì nếu hiển thị tất cả kỹ năng sẽ rất lộn xộn nên ta chỉ hiển thị 10 kỹ năng có xác suất cao nhất. Và tạo các slicer như Job Title và Job Skills.

![alt](https://user-images.githubusercontent.com/105619352/228897357-7ae708fc-696a-4b67-8bc7-5720d233980d.png)

Ta khám phá các công ty và lĩnh vực hàng đầu đang tuyển dụng các vị trí Data Science. Dưới đây là top 10 lĩnh vực tuyển dụng Data Science theo kinh nghiệm, cấp độ và số lượng tuyển dụng. 

![alt](https://user-images.githubusercontent.com/105619352/228897386-9f5bb12a-3d08-4f82-8719-1de72cecf139.png)

Dễ thấy rằng các công ty hàng đầu đều tuyển số lượng lớn các vị trí data science ở cấp độ Mid-Senior.

Công ty có số lượng bài đăng tuyển dụng nhiều nhất ở vị trí Data Engineer là Toptal.

### Kết luận
- SQL là một kỹ năng hàng đầu ở tất cả các vị trí trong lĩnh vực Data Science
- Các lĩnh vực công nghệ có nhu cầu cao với vai trò Data Science

## Bước 4: Xây dựng Dashboard
Dựa trên những gì đã làm, chia dashboard thành 3 thẻ riêng biệt: Job, Skills và Company.
Thêm vào một số biểu đồ và điều chỉnh các biểu đồ sao cho phù hợp.
Tạo một dashboard có trang chủ và người dùng có thể truy cập vào các trang khác từ trang chủ và ngược lại.

