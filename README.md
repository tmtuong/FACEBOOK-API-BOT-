# FACEBOOK-API-BOT-
## Lấy các thông tin cần thiết để requests
Để lấy được dữ liệu từ Facebook API trước tiên cần truy cập vào https://developers.facebook.com/apps/?show_reminder=true để kiểm tra có ứng dụng chưa. Nếu chưa cần tạo một ứng dụng mới, thiết lập API Marketing và chia sẻ các quyền liên quan đến đọc và lấy dữ liệu.
![image](https://user-images.githubusercontent.com/117967392/212615807-582c176b-ce4a-4c50-a68d-dce2cd37e96c.png)

### Lấy ID ứng dụng
Ở mục **Bảng điều khiển** ở góc trái mở thẻ **Cài đặt -> Thông tin cơ bản**, lấy ra ID ứng dụng.
![image](https://user-images.githubusercontent.com/117967392/212616494-42a7039c-9e57-4203-9c7b-df25d626ca9d.png)

### Lấy Khóa bí mật ứng dụng
Giống như cách lấy **ID ứng dụng**, nhập mật khẩu tài khoản Facebook để xem Khóa bí mật ứng dụng.
![image](https://user-images.githubusercontent.com/117967392/212616834-c9580399-1bcb-4bbd-ab95-8adf8bbfb40c.png)

### Lấy mã truy cập người dùng
Sau khi đã khởi tạo ứng dụng, truy cập https://developers.facebook.com/tools/explorer/ để lấy mã người dùng ở góc phải màn hình.
**Tuy nhiên tất cả mã truy cập lấy bằng cách này đều sẽ hết hạn trong vòng 1 tiếng**
![image](https://user-images.githubusercontent.com/117967392/212621616-4fdf0b3f-d625-4a79-adf9-d6191d615880.png)

***Cách lấy mã truy cập người dùng có hạn lâu hơn***

Có thể tham khảo chi tiết tại https://developers.facebook.com/docs/facebook-login/guides/access-tokens/get-long-lived/.

Tóm gọn lại bài viết trên, mã truy cập dài hạn sẽ có thời hạn sử dụng là 60 ngày (tính bằng giây) kể từ khi requests. Tất cả những gì cần có là: **Id ứng dụng, Khóa bí mật, Mã người dùng hiện tại**.

Sau khi có được tất cả những điều trên, truy cập https://developers.facebook.com/tools/explorer/, tại ô truy vấn nhập câu lệnh: **oauth/access_token?grant_type=fb_exchange_token&client_id={app-id}&client_secret={app-secret}&fb_exchange_token={your-access-token}** và thay **{app-id} = ID ứng dụng, {app-secret} = Khóa bí mật ứng dụng, {your-access-token} = Mã truy cập người dùng** rồi nhấn gửi.
![image](https://user-images.githubusercontent.com/117967392/212618508-c2d32eab-68dc-4ca4-b81a-9c0ca8c7cdf6.png)

Kết quả thu được là một mã truy cập có thời hạn 5175806s ~ 60 ngày.
![image](https://user-images.githubusercontent.com/117967392/212618624-12840845-177b-49b5-88f5-38d89c961c70.png)

## Facebook Page
Toàn bộ source code của bot kéo dữ liệu của Facebook Page được lưu trong file **FB_Page_API.ipynb**.

Cũng tương tự mã người dùng, mỗi Page cũng yêu cầu **Mã truy cập** để có thể lấy dữ liệu về. Có 2 cách để lấy **Mã truy cập trang**:

   1. Truy cập https://developers.facebook.com/tools/explorer/ vào thanh bên phải để lấy mã giống như cách lấy mã người dùng.
      ![image](https://user-images.githubusercontent.com/117967392/212622073-de70c228-5ff4-44ff-92d1-b3d37dea396c.png)

   2. Truy cập thông qua **Tài khoản Facebook** bằng cách sử dụng **Id ứng dụng, Khóa bí mật, Mã người dùng** đã tìm được trước đó. Cách này được sử dụng trong source code vì tối ưu hơn và không thủ công như cách 1.

***Cụ thể hơn về cách 2***

Truy cập đến dữ liệu mong muốn bằng cách sử dụng Graph API. Cụ thể hơn, để truy cập vào nhánh các Page do tài khoản Facebook quản lý, sẽ truy cập vào **me (tài khoản này) -> accounts (các Page do tài khoản quản lý) -> {access_token, id, name} (các trường mong muốn lấy ra)**
![image](https://user-images.githubusercontent.com/117967392/212623494-199f0596-3507-41af-8ad4-b8808d39672b.png)

Cú pháp để nhập vào ô truy vấn là 

