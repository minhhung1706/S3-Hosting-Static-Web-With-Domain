# S3 Hosting Static Web With Domain
---
### Xin vui lòng tham khảo bài lab này trước để biết cách host 1 static website trên Amazon S3. 
Link : [Basic Hosting A Static Website on S3](https://github.com/minhhung1706/s3-hosting-static-web)
---
### I Đăng ký tên miền (Domain) bằng dịch vụ Route 53
1 Vào AWS Management Console và tìm dịch vụ Route53(R53)

2 Vào R53 Management Console => Register Domain
![](images/route53-registration-a-domain.jpg)

3 Chọn tên cho Domain, tuỳ ý thích, miễn sao domain available (tick xanh) là được => **Continue**
![](images/route53-registration-a-domain-Choosing-a-Domain.jpg)

4 Điền contact detail của bạn. Đồng thời nhìn qua bên phải sẽ thấy thời gian (minimum 1 năm) và giá cả của Domain. => **Continue**
![](images/route53-reg-a-domain-step2.jpg) 

5 Verify email của bạn. Nhớ check cả spam/junk mail. Có thể bấm resend verification email nếu không thấy email. **Giữ trang web này**

Chú ý: Dòng **Do you want to automatic cally renew your domain**
* Nếu chọn **Enable** có nghĩa là cứ sau Z năm (do bạn chọn thời gian cho domain.) thì tài khoảng của bạn sẽ bị rút tiền để duy trì domain
* Nếu chọn **Disable** thì sau Z năm domain của bạn sẽ hết hạn và ko xài được nữa. 
![](images/route53-reg-a-domain-step3-checking-your-email-verify-email.jpg)

6 Email Verification có dạng như vầy
![](images/route53-reg-a-domain-step4-Email-Confirmation.jpg)

7 Sau khi verified email
![](images/route53-reg-a-domain-step4-Email-Confirmation-COMPLETED.jpg)

8 Trở lại trang web bước 5, phía dưới Verify Email ... bấm nút **Refresh** => Tick Term & Condition => Complete Order
![](images/route53-reg-a-domain-Final-Step-BackToStep3-Refresh-Status.jpg)

9 Tối đa 3 ngày domain sẽ sử dụng được. Nhanh nhất là sau 1 ngày sẽ dùng được.
---
### II Hosting Static Website trên Amazon S3 có sử dụng Domain
1 Vào S3 tạo 2 buckets: **domain.com** và **www.domain.com** (domain: tên domain của bạn). 1 cái sẽ là **domain chính**, 1 cái là **sub-domain**. Tuỳ ý bạn quyết định. 
* **Chú Ý:** cả 2 buckets này khi tạo thì **un-check** Block All Public Access
![](images/s3-create-2-bucket-domain-subdomain.jpg)

2 Chọn 1 bucket (ex: domain.com) tuỳ ý và Enable Static Web Hosting 
![](images/s3-choose-A-bucket-to-enable-static-web-hosting.jpg)

3 Tạo thêm 1 bucket thứ 3 để ghi log. Trong Bucket này, tạo 1 folder *log*
![](images/s3-Create-bucket-LOG-DOMAIN.jpg)
![](images/s3-Create-bucket-LOG-DOMAIN-Create-LOG-Folder.jpg)

4 Chọn **Sub-domain** để cấu hình Redirect to Domain. cấu hình xong =>  Save Changes
![](images/s3-choose-SUB-Domain-config-Redirect-TO-Domain.jpg)

5 Vào bucket domain => Server Access Logging => Edit => Enable => Browse S3 => Chọn bucket log => folder log đã tạo ở bước 3 => Save Changes
![](images/s3-Domain-Config-Server-Access-Logging.jpg)

6 Vào bucket domain => Bucket Policy => Edit => Save Changes
![](images/s3-Domain-Edit-Bucket-Policy.jpg)

7 Vào bucket domain => Bucket Access Control List => Edit => Tick *"i understand ......"* => Save Changes
![](images/s3-Domain-Edit-Bucket-ACL.jpg)

8 Tạo 1 file index.html đơn giản (hoặc vào folder file đính kèm down về) => upload vào **bucket domain** =>  Edit Access Control List cho file index.html (chỗ upload kéo xuống Additional Uploads Options) => **Upload**
![](images/s3-Domain-Upload-index-html.jpg)
![](images/s3-Domain-index-html-edit-ACL.jpg)

9 Bucket domain => Properties = Static Web Hosting => Bucket Website End Point => Copy & Paste vào browser để test
![](images/s3-ENDPOINT-Test-Succeeded.jpg)

10 vào Route 53 => Hosted Zones => Create Record
![](images/route53-Domain--hosted-zones-create-Record.jpg)

11 Nếu GUI hiện không phải là wizard mode thì bấm Switch To Wizard => Simple Routing => Next 
![](images/route53-Domain--SwitchToWizard-SimpleRouting.jpg)

12 **Define Simple Record:** Record Name bỏ trống để lấy giá trị mặc định. Configure xong bấm Define simple record => Create Records
![](images/route53-Domain-Define-Simple-Record.jpg)

13 Tạo thêm 1 record. Nhưng bây giờ phần Record Name: **www** => Để tạo alias. 
![](images/route53-Sub-Domain-Define-Simple-Record-.jpg)

14 Ra ngoài R53 hosted zones để check lại đã tạo thành công 2 record. 
![](images/route53-Domain-SubDomain-HostedZones-DoubleCheck-Record.jpg)

15 Check website với tên domain
![](images/Final-Check-website-with-domain.jpg)

16 Check Log Bucket, nếu configured đúng sẽ ghi lại log
![](images/Finally-checking-LOG-Bucket-Config-Correctly-Created-Logs.jpg)

---
# Hãy nhớ xoá đi tất cả những gì đã làm khi xong lab để tránh chi trả phí AWS không mong muốn

