# MD_AdsCrawl

## Tổng quan về Facebook API Marketing 

Quảng cáo Facebook là một trong những lựa chọn hàng đầu của các doanh nghiệp, tổ chức và cá nhân khi muốn thực hiện các chiến dịch marketing trên nền tảng mạng xã hội. Facebook Ads cũng cung cấp các tính năng để theo dõi Ads Performance trên nền trang của Facebook Ads, tuy nhiên việc lấy dữ liệu từ đó về máy tính cá nhân/ hệ thống lưu trữ của doanh nghiệp vẫn chưa được Facebook hỗ trợ. Có 2 lựa chọn cho công ty nếu muốn lưu lại dữ liệu từ hoạt quảng cáo vào hệ thống lưu trữ của họ.

- Một là, dùng các phần mềm do bên thứ 3 cung cấp: **Two Minutes Report, SuperMetrics** và trả phí hằng tháng.

- Hai là, tự động hóa quá trình này bằng cách xây dựng một tools riêng để kéo dữ liệu từ Facebook API (API miễn phí do Facebook cung cấp). **Resporitory này là một dự án lấy dữ liệu từ Facebook Ads bằng cách kết nối API thông qua ngôn ngữ lập trình Python**.

**Facebook API Marketing** là API dựa trên HTTP mà bạn có thể dùng để truy vấn dữ liệu, tạo cũng như quản lý quảng cáo và thực hiện nhiều tác vụ khác theo lập trình. Vì dựa trên HTTP nên API này hoạt động với bất kỳ ngôn ngữ hoặc phần mềm nào hỗ trợ HTTP, bao gồm cả cURL và hầu hết mọi trình duyệt web hiện đại. API Marketing được xây dựng dựa trên **API Đồ thị** của Facebook nên được gọi là Graph API và hầu hết mọi yêu cầu đều được chuyển vào URL lưu trữ **graph.facebook.com**.

***IMPORTANT NOTE: TẤT CẢ QUERY DÙNG ĐỂ TRUY VẤN DỮ LIỆU CÓ THẺ DÙNG TRỰC TIẾP TRÊN WEB (KHÔNG CẦN THÔNG QUA Ô TRUY VẤN Ở https://developers.facebook.com/tools/explorer/) BẰNG CÁCH THÊM ĐƯỜNG DẪN GRAPH API (https://graph.facebook.com/v15.0/) Ở ĐẦU.***

![image](https://user-images.githubusercontent.com/117967392/212632515-7934b03f-dde6-4121-8284-15afe8eb97ed.png)

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

### Mã truy cập trang

Cũng tương tự mã người dùng, mỗi Page cũng yêu cầu **Mã truy cập** để có thể lấy dữ liệu về. Có 2 cách để lấy **Mã truy cập trang**:

   1. Truy cập https://developers.facebook.com/tools/explorer/ vào thanh bên phải để lấy mã giống như cách lấy mã người dùng.
      ![image](https://user-images.githubusercontent.com/117967392/212622073-de70c228-5ff4-44ff-92d1-b3d37dea396c.png)

   2. Truy cập thông qua **Tài khoản Facebook** bằng cách sử dụng **Mã người dùng** đã tìm được trước đó. Cách này được sử dụng trong source code vì tối ưu hơn và không thủ công như cách 1.

***Cụ thể hơn về cách 2***

Truy cập đến dữ liệu mong muốn bằng cách sử dụng Graph API. Cụ thể hơn, để truy cập vào nhánh các Page do tài khoản Facebook quản lý, sẽ truy cập vào **me (tài khoản này) -> accounts (các Page do tài khoản quản lý) -> {access_token, id, name} (các trường mong muốn lấy ra)**
![image](https://user-images.githubusercontent.com/117967392/212624462-c8eec2c0-9046-4b1a-b19b-df54962f98cd.png)

Cú pháp để nhập vào ô truy vấn là **me?fields=accounts.limit(1000){access_token,id,name}&access_token={access_token}**

### Dữ liệu trên trang

Có 4 loại dữ liệu trên trang mà dự án này quan tâm là: 

**1. date (ngày)**

**2. page_name (tên trang)**

**3. page_likes_unique (lượt thích trang - chỉ tính user mới)**

**4. new_conversation_unique (lượt tin nhắn gửi đến - chỉ tính user mới)**

page_name đã lấy được khi lấy **Mã truy cập trang** trước đó.

date, page_likes_unique, new_conversation_unique sẽ được lấy bằng cách truy vấn vào **insights** từ mã truy cập trang.

- Cú pháp dùng để truy vấn **page_likes_unique**: **me/insights/page_fan_adds_unique/?since={start}&until={end}&access_token={access_token}**

![image](https://user-images.githubusercontent.com/117967392/212629355-d141848b-6a43-4671-9a2e-5189a501e483.png)

- Cú pháp dùng để truy vấn **new_conversation_unique**: **me/insights/page_messages_new_conversations_unique/?since={start}&until={end}&access_token={access_token}**

![image](https://user-images.githubusercontent.com/117967392/212629498-735e70c2-6245-4af1-a42f-1fa4c89e6eda.png)

- date có thể được lấy về từ 1 trong 2 cú pháp trên, vì tất cả giá trị trả về đều kèm theo ngày.

Lưu ý: since = {start} và until = {end} là 2 parameters truyền vào để thay đổi khoảng thời gian dữ liệu lấy về. access_token={access_token} là mã truy cập **trang** đã lấy trước đó, mã này dùng để phân biệt các trang nên mỗi trang sẽ có một access_token khác nhau.

***IMPORTANT NOTE: Facebook API chỉ hỗ trợ lấy dữ liệu nhiều nhất là 93 ngày, nên để lấy được dữ liệu nhiều hơn số ngày đó thì cần tạo ra nhiều query bằng cách thay đổi parameters since và until sau đó gộp dữ liệu thu được từ tất cả các query thành một bộ dữ liệu lớn.***

## Facebook Ads

### Adaccounts (Tài khoản quảng cáo)
Tất cả source code để lấy tài Adaccounts đều được lưu ở file **Update_account_id.ipynb**. Khác với source code của BOT (phải chạy liên tục), code này chỉ chạy khi có sự update Adaccounts.

Từ tài khoản Facebook, để lấy được các thông tin quảng cáo đã chạy, cần truy cập vào tài khoản quảng cáo mà tài khoản Facebook quản lý. Cần có **Mã người dùng** (đã lấy được ở trên).

**Mã tài khoản quảng cáo** có dạng: **act_{number} **

Cú pháp dùng để truy vấn Adaccounts: **https://graph.facebook.com/v15.0/me/?fields=adaccounts{name,id}&access_token={access_token}**

![image](https://user-images.githubusercontent.com/117967392/212634010-db6a6c22-77e4-4e25-a8de-8f4dd605c806.png)

Lưu ý: access_token={access_token} là mã truy cập **tài khoản Facebook** đã lấy trước đó, mã này dùng để phân biệt các **Tài khoản người dùng** nên mỗi **Tài khoản người dùng** sẽ có một access_token khác nhau, các **tài khoản quảng cáo** do cùng một **Tài khoản người dùng** quản lý sẽ có cùng access_token.

### Dữ liệu chạy Ads

Toàn bộ source code để lấy dữ liệu chạy Ads được lưu trong file **FB_Ads_API.ipynb**.

Dữ liệu sẽ được tách thành 2 phần để lấy về:

1. Lấy về thông tin cơ bản, chỉ số bài Ad (Ad Insights):
 
- **account_name (tên tài khoản chạy Ad)**

- **campaign_name (tên campaign)**

- **adset_name (tên adset)**

- **ad_id (mã bài đăng)**

- **date (ngày đăng)**

- **reach (số người dùng tiếp cận được)**

- **impressions (tần suất quảng cáo/nội dung của bạn được hiển thị)**

- **spend (chi phí)**

- **actions (đo lường hành động của mọi người đối với bài Ad)**

Lưu ý: Tùy vào mục tiêu đặt ra của từng campaign, actions quan tâm sẽ khác nhau. Chủ yếu sẽ quan tâm đến các trường: **Likes (lượt thích trang), Post engagement (tương tác), New message within 7 days (tin nhắn mới trong vòng 7 ngày - user nhắn tin lại trong vòng 7 ngày sẽ không tính)**.

Dữ liệu được lấy về cũng bằng cách requests từ Graph API tuy nhiên ở phần này vì lượng dữ liệu khá lớn nên sẽ được lấy **daily** và sử dụng thư viện **facebook-business** của Python để lấy về. Chi tiết tham khảo thêm ở source code.

Cú pháp dùng để truy vấn: **{Adaccount_id}/insights/?level=ad&fields=ad_id,account_name,campaign_name,adset_name,actions,impressions,reach,spend&time_range[since]={date_start}&time_range[until]= {date_stop}&limit=10000&access_token={access_token}**.

Có tất cả 4 level để lấy về: **Adaccount -> Campaign -> Adset -> Ad**, ở dự án này sẽ lấy ở level Ad. Level nhỏ nhất sẽ kế thừa tất cả các thông tin ở các level trên (account_name, campaign_name, adset_name)

![image](https://user-images.githubusercontent.com/117967392/212660596-42eda1dd-fc66-4ed3-89e3-22aedd14997d.png)

Vì dữ liệu được lấy **daily** nên parameters date_start và date_stop sẽ giống nhau. Sử dụng thư viện **facebook-business** các trường sẽ được tách ra thành fields (các trường cần trả về) và params (parameters truyền vào).

2. Lấy về thông tin chi tiết bài Ad (Ad Creative) thông qua ad_id đã tìm được và nối lại với dữ liệu ở phần 1:

- **Ad body (nội dung bài Ad)**

- **Ad url (liên kết dẫn đến bài đăng) = "www.facebook.com/ + object_story_id"**

- **Ad image (hình ảnh đính kèm)**

Ở phần này sẽ requests riêng lẻ theo từng **ad_id** (lượng dữ liệu cho mỗi lần requests ít) nên sẽ quay về cách requests bằng cách truy vấn như ban đầu.

Cú pháp truy vấn: **{ad_id}?fields=adcreatives{body,effective_object_story_id,thumbnail_url}&access_token={access_token}**

![image](https://user-images.githubusercontent.com/117967392/212641504-3a5394aa-2726-40cc-8d58-248071442fec.png)

##Source code summary:
- FB_Ads_API_daily: Chạy liên tục để lấy dữ liệu FB Ads trong ngày. Thời gian chạy tuỳ vào lượng Ads chạy trong ngày đó.
- FB_Ads_API_backup: Chức năng để lấy dữ liệu FB Ads như FB_Ads_API_daily, tuy nhiên sẽ chạy cả lifetime.
- GetAds: Dùng để kiểm tra điều kiện data hiện tại để quyết định chạy **FB_Ads_API_daily (nếu chạy trước 5AM và ngày lớn nhất trong data hiện tại bằng với ngày hôm nay)** hay **FB_Ads_API_backup(nếu chạy sau 5AM và ngày lớn nhất trong data hiện tại bé hơn ngày hôm nay)**.
- LookupValue: Chứa các **defined functions** dùng để nối giá trị giữa nhiều sheet excel lại thành một và tách các giá trị cần thiết.
- FB_Page_API: Chạy liên tục để láy dữ liệu FB Page, vì dữ liệu ít nên lấy cả lifetime (tính từ 2022-10-01). Thời gian chạy 2 phút.
- Update_account_id: Chạy khi được báo là đã có update trong tài khoản chạy ad. Thời gian chạy 2 phút.

## Refresh rate
- Dữ liệu từ FB Page sẽ được chạy liên tục và refresh **1 tiếng 1 lần**.
- Dữ liệu từ FB Ads (FB_Ads_API_daily) sẽ được chạy liên tục và refresh **1 tiếng 1 lần**. 
- Update_account_id sẽ được chạy **thủ công** hoặc **1 ngày 1 lần**.
- File LookupValue sẽ chạy mỗi khi chạy file FB_Ads_API_daily hoặc FB_Ads_API_backup.
