# aws-saa
Note for AWS SAA-C02

==========================NOTE EXAM======================================

1. S3 không thể kết nối bằng https mà chỉ có thể kết nối bằng http. Nên để có thể host website mà có https thì phải sử dụng qua Cloudfront.Trỏ OAI về S3
2. Scale RDS chỉ để high availability thì có thể lưu ý tới scale read replica multiple AZ
3. DynamoDB ko có khái niệm Read-Replica, mà chỉ có thể dùng DAX để tăng tốc độ đọc nhờ vào cơ chế caching
4. Api Gateway cũng có thể dùng để tách các application ra với nhau bằng cách cung cấp cho mỗi application một đầu api endpoint
5. EC2 hoặc VM muốn access data sử dụng native file  thì có thể nghĩ đến FSx for Lustre, nó làm việc như bất kì file system nào với linux. FSx for lustre này thì kết hợp tốt được với S3
6. Với ECS thì để cấp quyền cho một application container thì cần phải tạo IAM role cho Task Definition. Và chỉ có thể gắn 1 IAM role cho 1 Task Definition.
7. ALB có thể route traffic với một service nào đó dựa trên Path-base routing rule. Rule điều hướng sẽ dựa vào hostname hoặc là URL.
8. Redshift có thể dùng để chạy phân thích các truy vấn phức tạp. Để cải thiện hiệu suất cho các truy vấn lặp lại thì nó sẽ lưu vào cache. Các tool dashboard, visualization và BI thường thực hiện các truy vấn lặp lại nên sẽ tận dụng được hiệu năng khi caching
9. Khi yêu cầu hỏi về truy xuất dữ liệu trả về kết quả update mới nhất thì dùng RDS, vì nó có cơ chế read-after-write.
10. Với mô hình ELB + CloudFront thì để không cho user bỏ qua cloudfront mà truy cập trực tiếp vào ELB thì ta dùng security Group để chỉ cho phép range ip của cloudfront có thể truy cập tới ELB. Vì được update liên tực nên có thể sử dụng lamdard để update ip mỗi khi thay đổi dựa vào trigger từ SNS gửi đến mỗi khi cloudfront thay đổi địa chỉ.
11. Trên EC2 để lưu trữ tạm thời mà cần IOPS cao thì có thể lưu vào Instance Storage bằng cách gắn thêm.  Đây là loại có tốc độ nhanh nên có thể giải quyết được tình trạng cần IOPS cao
12. Những câu hỏi mà đòi hỏi storage low latency , IOPS lên đến cả triệu, và xử lý được hàng triệu transactions cùng lúc thì có thể dùng đến Instance store. Tuy nhiên dữ liệu sẽ ko bền.
13. Với câu hỏi mà yêu cầu low latency và hoạt động global, ứng dụng thuộc dạng dynamic data và trong đề có gợi ý tới 2 IP whitelist thì có thể để ý tới Global Accelerator.
14. Để truy cập từ 1 application tới DynamoDB hoặc S3 mà ko ra internet thì sử dụng Gateway Enpoint.  Sau đó tạo route table cụ thể cho truy cập vả gắn vào Gateway Enpoint
15. Muốn hạn chế quyền gì đó trên OU AWS Organization thì ta cần sử dụng SCP và deny các rule ko mong muốn. Còn muốn allow gì đó thì dùng allow rule.
16. Dùng container trên AWS mà opensource ---> EKS
17. Với những task mà chạy vài giờ trong 1 ngày, vài ngày trong 1 quý hoặc 1 năm. Thì nên cân nhắc dùng Spot Block instance. Vì nó hỗ trợ giữ ko bị gián đoán trong tối đa 6h, và giá thì oke hơn.
18. Trường hợp mà yêu cầu database có khả năng chạy trên nhiều region và active-active thì có thể cân nhắc dùng đến DynamoDB with global tables
19. Với cloudfront với s3 là origin thì có thể dùng pre-sign URL để authorized bằng cách owner object share object với người khác bằng cách tạo presign url và sử dụng chính security credential của họ
20. Với những câu hỏi database mà yêu cầu highly responsive and reieval time optimized thì nên chú ý tới elasticcache redis
21. Với câu hỏi disaster recovery mà yêu cầu scaled down version thì sẽ là warm standby
22. Với redshirt để thực hiện phân tích, truy vấn thì cần phải tải data từ S3 vào redshift để thực hiện. Tuy nhiên với redshift spectrum thì có thể thực hiện trực tiếp trên S3. Redshift sử dụng cho OLAP.
23. Để mở rộng thông lượng đáp ứng traffic thực tế thì có thể sử dụng DynamoDB Auto Scaling, và cùng là giải pháp mở rộng tối ưu chi phí nhất. Ngoài ra để có thể tăng tốc trong việc đọc và performance thì hãy cân nhắc tới DAX vì nó cache lại thông tin cho tốc độ phản hồi cực nhanh.
24. Có thể dùng S3 select cho phân tích và sử lý dữ liệu trong một đối tượng trong nhóm S3, nhanh hơn và rẻ hơn.
25. Câu hỏi giải pháp giúp giảm latency cực thấp từ 5-10ms, và đòi hỏi performace cực cao thì nên dùng tới caching như là elasticcache redis, vì nó là memory cache nên có performance tốt, latency thấp và xử lý được hàng triệu request/perseconds
26. Với restore data point-in-time của RDS thì có thể restore trong tối đa 5p đã qua, và khi restore lại DB thì security group được apply cho RDS mới đó là default DB security group. RDS là OLTP
27. Để phân biệt các môi trường và tránh ảnh hưởng với nhau ta có thể dùng tag, và khi apply một cấu hình gì đó theo tags thì nó sẽ chỉ apply đúng với tag đc matching và ko ảnh hưởng tới những resource có tags khác
28. Để chạy được autoscale for multiple subnet thì điều cần làm là kiểm tra xem các subnet cần autoscale đã được cấu hình và khai báo trong ASGs chưa. Nếu k cấu hình thì sẽ ko autoscale các instance trên subnet đó được.
29. Để kết nối tới một application mà ko phải là DynamoDB và S3 trong private network thì ta có thể sử dụng interface gateway endpoint sử dụng private link và một ENI
30. Để dễ dàng deploy một ứng dụng và vẫn kiểm soát được phần resource đằng sau đó thì nên sử dụng Beanstalk
31. Để orchestrate serverles cụ thể là lamdar thì ta nên dùng Step Functions. Để nhận biết thì khi nào có compute layer of severles thì khả năng là nó. Hoặc là coordinate multiple AWS services into serverless workflows.  
32. Cloudwatch theo dõi performance của lamda dựa trên số lượng request và latency của request
33. Để query meta-data của instance trong một VPC ta có thể dùng Metadata Querytool hoặc là curl "curl http://169.254.169.254/latest/meta-data/"
34. với security Group mới tạo, thì tất cả traffic out đều cho phép, và traffic in thì đều bị denie
35. Khi tạo 1 instance trong VPC default thì sẽ được assigned DNS hostname public và private. Còn khi tạo 1 instance trên non VPC default thì chỉ có dns hostname private. Nếu muốn public thì instance đó cần phải có ipv4 public và cần chỉ định thì mới có.
36. Để copy môt EBS snapshot đã encrypt từ môi trường này sang môi trường khác(mỗi môi trg là mọt account), ví dụ như dev ---> prod, thì cần modifier permission của snapshot đó cho phép share qua một tài khoản khác, và sau đó share custom key dùng để encrypt volume đó 
37. Câu hỏi về storage mà có yêu cầu file system interface ---> EFS  Elastic File System (EFS) 
38. Để có thể dụng S3 lưu trữ dữ liệu cho từng user và user này ko được tháy dữ liệu của user khác. Thì việc đầu tiên là tạo IAM Policy áp dụng cho folder permission và sau đó tạo IAM group và đính kèm IAM policy, sau đó thêm IAM user vào group đó.
39. File  gateway để lưu trữ các đối tượng(NFS, SMB), trong đó các tệp được lưu trữ trực tiếp trên S3. Còn Volume gateway thì dùng để lưu trữ block(iSCSI). Stored volume là lưu trữ dữ liệu trên on-priemese và sao lưu trên S3. Còn Cache volume thì lưu toàn bộ dữ liệu trên S3 và chỉ cache những dữ liệu thường xuyên được sử dụng trên on-priemese
40. SAM (Serverless Application Model) là phần mở rộng của cloudformation sử dụng để đóng gói, kiểm tra và triển khai các serverless application
41. Với DR strategy có core service will be replicated to the DR site thì khả năng cao là Pilot Light. Với chế độ này thì những thành phần quan trọng như database hoặc documents quan trọng sẽ được đồng bộ hóa liên tục. Lúc sự cố xảy ra thì chỉ cần tạo các service khác (EC2, Lamdar ....)một cách nhanh chóng tạo ra một mô hình đầy đủ .
42. Để RDS có thể truy cập public thì cần phải chọn option publicly accessible, và sau đó tạo SGs và gắn cho RDS để có thể truy cập từ ip public. Đặt RDS ở subnet public để có thể truy cập từ bên ngoài)
43. Với DynamoDB thì nên lưu trữ những data truy cập thường xuyên và không thường xuyên ra các bảng khác nhau. Lưu trữ những đối tượng lớn 400KB trong S3 và sử dung con trỏ (ID đối tượng S3) trong DynamoDB. Ngoài ra có thể nén các giá trị có thuộc tính lớn hơn.
44. Với câu hỏi mà có từ khóa columnar database, và có liên quan tới query hay analytics thì khả năng cao là Redshift.
45. Multi-AZ RDS tạo bản sao và sao chép đồng bộ.
46. Với MySQL có thể sử dụng AWSAuthentication Plugin để xác thực tài khoản. Có thể dùng cho các trường hợp  short-lived credentials.
47. Nên sử dụng Launch Template hơn là Lauch Config. Để sử dụng những tính năng mới hơn. Với LC mỗi khi muốn sửa đổi gì thì bắt buộc phải tạo LC mới rồi sau đó mới apply được vào Auto scale group, còn với LT thì có thể sửa đổi và apply vào Auto Scale Group mà ko cần tạo mới.
48. Với những task HPC architecture mà yêu cầu chạy trên nhiều node cùng lúc. Thì giải pháp là dùng AWS Batch multiple-node parallel job.
49. Với healthcheck, trạng thái unhealthy status ==> phiên bản sẽ bị terminate. với impaired ==> ASG sẽ đợi vài phút nếu instance ko phục hồi được thì sẽ thực hiện hành động tiếp theo. Nếu impaired vẫn còn thì sẽ thực hiện terminate.
50. Một số lý do khiến EC2 bị terminate ngay lập tức là : Hết EBS, EBS snapshot bị hỏng, Root EBS bị mã hóa và bạn không có quyền truy cập KMS để giải mã, Instance store-backend AMI sử dụng để khỏi chạy bị thiếu một phần bắt buộc (image.part.xx file)
51. API gateway + Lamdar là 1 cặp GWL
52. Trên Gateway để giới hạn 1 client request quá nhiều lần ta sử dụng per-client throttling limits. Người ta thường áp dụng với các api liên kết với chính sách sử dụng của bạn làm mã định danh khách hàng (usage policy as client indentifier).
53. Để limit request thì ta sử dụng API Gateway và cấu hình throttling rule. Tại đây có thể điều chỉnh các quy tắc giới hạn số lượng request.
54. Với các yêu cầu mà để ứng dụng trên AWS có thế kết nối và truy cập đc với bên On-primeses mà ko phải đi ra ngoài internet thì giải pháp tốt nhất là sử dụng private virtual interface qua Direct connect để kết nối với VPC bằng ip private. Một VPC enpoint cho một dịch vụ nào đó cần được kết nối trên CLoud có thể được tạo, và điều này cung cấp quyền truy cập vào dịch vụ đó bằng ip private mà ko cần đi qua internet.
55. Trên API gateway ta có thể bật tính năng cache stage để caching giảm lượng request xuống backend và giảm latency. Tại đây có thể set được Method (Default là GET)nào cần cache và TTL để expire cache
56. AThena có thể truy vấn dữ liệu đã được mã hóa lưu trữ trong S3 và ghi ra kết quả đc mã hóa trở lại S3. Hỗ trợ cả mã hóa server-side và client-side
57. Trong S3 có thể cấp quyền programmatic để có thể access vào S3 bằng lệnh hoặc managerment console access để access vào bằng console
58. Muốn backup EBS snapshot lên S3 thì chỉ cần snapshot nó. Vì mặc định nó sẽ tự động lưu trữ snapshot lên S3 rồi
59. Với những website mà có S3 là storage cho lưu trữ, mà ko muốn user có thể gọi vào trực tiếp S3, thì có thể dùng bucket policy và allow chỉ referrals từ chính domain của website đó mới có thể get nội dung từ S3. Và nếu các nguồn khác truy cập sẽ ko được vì ref ko bắt nguồn từ domain đã allow
60. Formation cung cấp hai phương pháp cập nhật ngăn xếp: là direct update hoặc creating và executing change set.  
61. Với trường hợp scale-in trong AGS để có thể giữ lại được data của instance sắp bị terminate. Thì ta nên cài đặt lifecycle hook trong AGS, trong khoảng thời gian chờ lifcecycle này ta có thể thực hiện các hoạt động tùy chỉnh như backup data, move data hoặc làm gì đó khác. (thời gian mặc định là 1H)
62. Snapshot EBS được lưu trữ trong S3 và kiếm soát bởi aws, ta không thể xem được snapshot trong s3.
63. ĐỐi với dữ liệu cần query hoặc phân tích từ 100TB trở lên thì redshift là solution lý tưởng nhất.
64. Các trường  hợp mà disaster mà ko biết chắc câu trả lời thì nên chọn những câu nói về regions.
65. Với S3 mà cần max performance nữa thì có thể dùng prefix cho mỗi object name with hex hash key along wirh the current date (hiện tại ko còn hữu dụng nữa)
66. Default invisible time của SQS là 30s
67. Để quản lý các API và monitor  các api thì dùng cloud trails, vì cloud trails giám sát các lệnh gọi api cho tất cả tài nguyên trong aws và lưu nó vào s3
68. Với cloudfront để tránh trùng lập logs trừ cloud trails thì có thể tắt logs từ tất cả regions trừ regions ta muốn audit logs. Đặc biệt để tắt logs cloud trails cho cloudfront thì chỉ có thể sử dụng cli.
69. Chuyển tiếp chuỗi truy vấn ở cloudfront chỉ hỗ trợ web distribution. Với cloudfront để caching url gồm có param thì chuỗi truy vấn tham số đó sẽ xuất hiện sau ? và chuỗi truy vấn có thể có nhiều tham số và phân tách với nhau luôn là kí tự &,  tên và giá trị của tham số được sử dụng trong chuổi truy vấn có phân biệt chữ hoa và chữ thường.
70. Câu hỏi có chef hoặc salt ==> opswork
71. Câu hỏi Blue-Green ==> route53 weighted
72. Với EFS để mã hóa dữ liệu in transit trong quá trình chuyển dữ liệu từ  AWS EFS tới client ta có thể sử dụng EFS mount, và bật tính năng mã hóa dữ liệu chuyển tiếp.
73. Với NLB, để sử dụng TLS kết nối với máy khách, NLB sử dụng chính sách bảo mật bao gồm giao thức và mật mã (ciphers). Nên sử dụng AWS Certicicate Manager vì các chứng chỉ sẽ tự động gia hạn khi hết hạn.
74. Trong RDS, Read Replica laf những database riêng được sao chép một cách không đồng bộ. Nên đôi khi sẽ có trường hợp trễ dẫn tới bản ghi cuối chưa kịp đồng bộ.
75. Trong S3 tuân theo mô hình eventual consistency model. Do đó nên việc cập nhật đối tượng có thể có một chút chậm trễ khi một đối tượng vừa cập nhật được yêu cầu truy cập từ người dùng trong yêu cầu đọc tiếp theo - tức là người dùng có thể ko đọc được cập nhật mới nhất của đối tượng.
76. Với AWS Glue ta có thể bật Job Bookmark để theo dõi dữ liệu, bật bookmark sẽ giúp chỉ quét các thay đổi kể từ dấu trang cuối cùng và ngăn chặn viẹc xử lý  lại toàn bộ dữ liệu
77. Với các resource aws ở các môi trường khác nhau mà ko muốn bị ai đó xóa nhầm, thif ta có thể thêm tags resource để dễ dàng quản lý hơn các quyền truy xuất tới resource, đồng thời ngăn chặn nhầm lẫn trong quá trình phân quyền. 
78.  Mặc định logs của cloudtrails đã được mã hóa ở phía máy chủ S3(SSE). Tuy nhiên bạn có thể chỉnh sửa kiểu mã hóa bằng KMS và cài đặt các thông số tùy ý
79. Với athena để giảm chi phí xuống thì cần lưu ý những vấn đề sau: Aws athena tính tiền dựa trên mỗi truy vấn và lượng dữ liệu được quét trong mỗi truy vấn. Nêu để giảm thiểu chi phí thì có thể giảm lượt truy vấn xuống hoặc là giảm khối lượng dữ liệu quét trên mỗi truy vấn bằng cách phân tách các dữ liệu theo workgroup (VD: theo nhóm dữ liệu, theo tháng, theo năm,  theo ứng dụng ..... ) để khi quét ta chỉ tập trung vào đúng dữ liệu cần quét mà ko cần quét lan man sang cả những dữ liệu k cần thiết để giảm thiểu chi phí.
80. Với EBS thì mỗi EBS được sao chép tự đọng trong vùng khả dụng của nó để bảo vệ dữ liêu khỏi lỗi. mang lại tính khả dungj và độ bền cao.
81. Trong EMR bản spot cung cấp tùy chọn để mua dung luong phiên bản EC2 với chi phí giảm so với mua theo nhu cầu
82. Với việc sử lý logs realtime và report realtime thì nên cân nhắc việc sử dụng với kinesis data stream.
83. Khoảng thời gian cooldown trong instance là một cài đặt để định cấu hình cho ASG nó sẽ định khoảng thời gian cooldown hoàn tất trước khi mở rộng quy mô ứng dụng tiếp tục.  Việc này giúp cho ứng dụng không bị scale một cách quá dẫn tới tình trạng dư thừa resource trước khi được route traffic vào. (stabilize)
84. Để mà backup một lương lớn EC2 để sau này có thể khôi phục sau sự cố nhanh nhất mà ít thao tác nhất thì có thể dùng AWS backup để có thể lên plan backup cho toàn bộ group ec2 instance. Rồi khi recorvery thì dùng AWS api hoặc AWS CLI để restore lại nhanh nhất.
85. Để có thể xem response code của Route53 thì ta có thể  dùng tính năng Route 53 to configure query togging
86. Để kiểm tra tất cả thông tin trạng thaí của các service AWS thì có thể dùng AWS Personal Health
87. Trong CLoudfront để bảo vệ dữ liệu của user up lên ta config field-level encryption profile
88. Với câu hỏi migrate data (fileshare) của windown từ on-premises lên aws thì khả năng cao là dùng FSx for Windowns File Server. KHông nên dùng Storage gateway vì thường storage gateway dùng để tích hợp với storage của AWS, nên có thể hiểu là nó dùng để chia sẽ tệp giữ on-primese với aws hoặc mở rộng khả năng lưu trữ mà vẫn giữ hệ thống tại chỗ không hoàn toàn di chuyển nó
89. Có thể sử dụng lamdar@edge để custom request và response trả về cho cloudfront. Ngoài ra tại đây ta có thể thực hiện các hành động xử lý trực tiếp mà không cần phải gửi request đó tới origin để xử lý. Thích hợp cho các trường hợp yêu cầu xử lý nhanh tránh để user đợi gây ra timeout.
90. Trong trường hợp mà over-provisioning thì nên sử dụng target tracking-scale.
91. Sự khác nhau cơ bản giữa multi AZ vs read replica là : multi AZ là synchronous còn read replica là asynchronous. Ngoài ra nó còn tự failover khi sự cố xảy ra và promoted từ seconds lên primary.
92. Với Standard Reverved Instance khi mà không dùng có thể bán trên Reserved Instance Maketplace (but not Convertible Reserved Instances).
93. Cloudwatch chỉ có sẵn những metrics sau: CPU utilization, Network utilization, Disk performance, Disk read/write. Nếu muốn lấy thêm các metrics khác thì phải cài thêm cloudwatch agent lên server thì mới lấy đc.
94. Không có khái niệm IAM Group nên chú ý 
95. AWS lake formation là một service dễ dàng tạo và setup một datalake.
96. ECS chạy theo task nên có thể dùng để involk một task lên khi được trigger từ EventBridge.(tương tự như cách thực hiện với lamdard)
97. Trên S3 để nhận được notification khi có một hành động nào đó trên bucket của mình thì enable notification. Sau đó setup destination muốn push noti (SQS, SNS, Lamdard)
98. Trong RDS có một loại authen thông qua token để truy cập RDS(postgress + mysql). Authentication token là một chuỗi không trùng, đc gen ra bởi RDS. Mỗi token thì chỉ có thể dùng trong vòng 15p. Nên ko cần phải store user credential hoặc user/password để có thể access.- TÍnh năng đó gọi là IAM DB authentication
99. Những trường hợp sử dụng signed URL cloudfront: RTMP (signed cookies ko support RTMP), khi muốn hạn chế quyền truy cập vào các tệp riêng lẻ, sử dụng cho các ứng dụng client
100. Những trường hợp sử dụng signed cookie cloudfront: khi muốn hạn chế quyền truy cập vào nhiều tệp, khi bạn không muốn thay đổi URL hiện tại.
101. TRong redis cluster muốn thêm một lớp bảo mật nữa bằng password thì ta thêm param auth-token, còn muốn encrypt data transit thì thêm --transit-encryption-enable
102. Trong Aurora Serverless cluster có thể set minimum and maximum capacity cho cluster
103. Để monitor các thông số chi tiết của RDS Instance như Os metric, ram, cpu, RDS chill process .... ta có thể bật chế độ Enhanced monitoring in RDS
104. Database ko bao giờ nên dùng HDD mà nên dùng SSD hiệu suất cao như IOPS
105. Để có thể phân quyền và sửa dụng được S3 cho công ty với lượng user lớn mà cần phải authen qua SSO như AD, LDAP ... thì đây là trường hợp nên sử dụng Temporary credential , thực hiện các bước sau: Setup a Federation proxy or an Identity provider --> Setup an AWS Security Token Service to generate temporary tokens --> Configure an IAM role and an IAM Policy to access the bucket.
106. Những câu hỏi để bảo vệ backend system khi có lượng truy cập lớn từ bên ngoài thì có thể lưu ý tới throtting limit ở trên API Gateway nếu có sử dụng API Gateway.
107. Với những câu hỏi move data từ on-premiese lên aws thì chú ý đến giải pháp snowball thay  vì direct-connect, bởi vì direct-connect thường dùng cho việc hybric trao đổi dữ liệu qua lại, còn snowball thì move data lên đúng 1 lần.
108. DataSync có thể chuyển trực tiếp dữ liệu lưu trữ dài hạn như S3 glacier và S3 glacier deep archive, mà ko cần phải dùng licycle để move class.
109. Mặc định không thể tắt IPV4 trên VPC. Còn IPV6 có thể tắt hoặc mở.
110. Secret Manager thường dùng để chứa các credential database, và có thể rotate. 
111.  Redshift xử lý analytics rất nhanh (near real time)
112. Để bảo mật hơn trong việc migrate database bằng DMS ta có thể cấu hình SSL assign certificate tới endpoint.
113. Trong aws để bắt buộc các cấu hình cũng như tài nguyên phải được thực thi tuân thủ theo một tiêu chuẩn nào đó (enforce strict compliance). Thì ta có thể sử dụng Aws Config để cấu hình tiêu chuẩn, cũng như là những hành động sẽ thực thi nếu một config hoặc một resouce nào đó vi phạm tiêu chuẩn này.
114. Trong RDS multiple AZ, failover sẽ xử lý tự động khi node primary down mà không cần phải can thiệp bất cứ hành động nào. Khi đó RDS sẽ check lại record name (CNAME) của DB instance, và thay thế nó bằng hostname của DB standby. Thì khi đó ứng dụng gọi DB sẽ đc trỏ về Instance DB primary mới (Standby trước đó được promote lên primary, khi primary cũ chết).
115. Với các website tĩnh bao gồm HTML, CSS, javascript và hình ảnh thì có thể dùng S3 để làm static website. (do javascript là ngôn ngữ xử lý phía máy khách nên nó được xem như là một file tĩnh).
116. Trong Dynamo DB, read và write độc lập nhau, nên khi muốn tăng gì đó thì sẽ ko ảnh hưởng cái còn lại. Ta có thể tăng write capacity assigned to the shard tables (mà ko cần phải tăng hết cả capacity cho read và write).
117. Với các trường hợp cần phân tách messege theo một filter nào đó, thì ta có thể sử dụng SNS để filter sau đó có thể gửi về cho SQS hoặc Lamdard để có thể xử lý. 
118. Với câu hỏi cho storage datawarehouse hoặc cho những lưu trữ mà cần có khối lượng lớn nhưng ko cần IOPS cao, ít truy cập, tiết kiệm chi phí hoặc các yêu cầu sau (depending on whether the question asks for a storage type which has small, random I/O operations or large, sequential I/O operations.) thì có thể lưu ý tới cool HDD(sc1)
119. Với Cloudwatch alarm actions ta có thể tạo alarms để thực hiện các hành động tự động như stop, terminate, reboot hoạc recover EC2 instance mà ko cần thực hiện bằng tay.
120. Thực tế thì SQS có giá thành rẻ hơn Kinesis data stream. Nếu yêu cầu ko bắt buộc phải real-time thì nên thay thế Kinesis data stream bằng SQS.
121. Trong trường hợp có quá nhiều connect (too many connection )gọi từ client đến DB instance. Thì có thể sử dụng RDS Proxy để tận dụng lại các kết nối trước đó cho query đồng thời cũng hạn chế được tình trặng một lượng lớn connect ập vào làm chết DB. 
122. Với các câu hỏi route traffic từ DNS Route53 mà có yêu cầu route traffic từ chỗ này nhiều hơn chỗ khác dựa vào vị trí của user để route traffic thì cân nhắc tới Geoproximity.
123. Để có thể sử dụng VPC endpoint cần attach endpoint policy tới service mà muốn connect tới.  Ví dụ muốn sử dụng VPC enpoint để kết nối tới S3 thì phải gán policy tới S3 để trusted S3 bucket.
124. Với các trường hợp mà xây dựng hệ thống nhưng ko biết hệ thống sẽ phát triển ra sao, hoặc trong quá trình thử nghiệm hệ thống mà có thể thay đổi các thành phần cáu trúc bên trong của hệ thống thì có thể sử dụng Proton để xây dựng hệ thống theo template, và khi muốn thay đổi hay thêm sửa gì thì có thể chỉnh sửa file template này.
125. Trong trường hợp sử dụng SQS và đặt thời gian retention, tuy nhiên khi vượt quá thời gian retention thì các messege trong queue sẽ bị xóa. 
126. Với trường hợp một instance chạy dịch vụ quan trọng và được add DNS private. Để hạn chế down time khi instance quan trongj đó chết, ta tạo ,cho nó một interface network và một ip private. Dùng IP đó add lên DNS của dịch vụ đó. Khi mà instance chính down thì ta dùng interface netowrk đó gắn cho một instance khác. Khi này thì không cần đổi DNS gì cả.
127. Service  Simple Workflow (SWF) cũng có thể dùng để decouple một hệ thống. Dùng để điều phối task và theo dõi state.
128. Có thể sử dụng Control Tower để tự động hóa các hoạt đọng như cấu hình đa tài khoản, quản lý quyền truy cập và quy trinh cấp tải khoản. Quản trị viên có thể chọn và áp dụng các chính sách có sẵn hoặc cho nhóm doanh nghiệp cụ thể. 
129. Để có thể cấu hình website static trên S3 với một domain thì trước hết phải đăng kí domain và để đăng kí được một domain như mong muốn thì tên của bucket phải giống với tên của domain muốn đăng kí. Ví dụ: muốn đăng kí domain thanhlam.me cho static website trên S3 thì tên của bucket phải là thanhlam.me
130. Storage gateway bao gồm : file gateway, volume gateway và tapegateway
131. Trường hợp gửi mail trong lúc monitoring thì nên sử dụng SNS hơn thay vì SES, vì SES sinh ra là dịch vụ đẻ gửi mail thông báo, mail cho khác hàng hơn là mail cho monitor hệ thống.
132. Trong trường hợp một vài ứng dụng mobile muốn cập nhật dữ liệu liên tục từ client thì có thể sử dụng AppSync để làm việc đó
133. Elasticcache cải thiện performance bằng cách caching kết quả query.
134. DAX có thể cung cấp performance lên tới miliseconds to microsecond, và đáp ứng được hàng triệu request/s
135. Kinessis Data Firehose có thể dễ dàng load stream data into data store and analytics tools. nó có thể caputure, transform và load streaming data vào S3
136. Với chế độ hibernation thì không thể enable hoăc disable cho một instance sau khi mà nó đã lauch, để làm được điều này ta phải migrate ưng sdungj sang một instance khác với hibernation đã được enable.
137. Trong cloudformation để ngăn chặn không cho trạng thái tạo hoàn tất cho đến khi nhận được tín hiệu thành công được chỉ định hoặc khoảng thời gian chờ bị vượt quá, ta có thể sử dụng CreationPolicy. Để báo hiệu một tài nguyên có thể sử dụng được thì ta sử dụng tập lệnh cfn-signal.
138. Aurora thường liên quan đến một cụm cá thể thay vì một cá thể. Ví dụ khi có nhiều read request vô Aurora thì reader enpoint (build-in)của nó sẽ loadbanlance các request này vào nhiều read replica instance bên dưới.
139. AWS Backup là một centralized backup service, có thể dễ dàng tạo backup với chi phí rẻ. Nó có thể backup storage volume, database, filesystem ... có thể tự backup, auto backup theo schedule, retention policies và monitor all recent backup và restore activity. AWS backup hữu dụng với các trường hợp backup vượt ngoài tầm backup của service ví dụ như retention period của service bị giới hạn.
140. Autoscale không phải là tùy chọn mặc định trong DynamoDB
141. Cluster placement group  thường dùng cho tightly-coupled node-to-node, Partition Placement group thường dùng cho hadoop, cassandra, kafka.
142. Trong AWS sẽ có giới hạn vCPU-base On-demand cho mỗi khu vực, vì vậy khi dùng vượt quá giới hạn này thì sẽ ko tạo được nữa. Khi này ta điền vào biểu mẫu yêu cầu tăng giới hạn tới AWS.
143. Mỗi subnet trong VPC chỉ map với một AZ. và mỗi subnet sau khi được tạo nó sẽ đc liên kết tới main route table của VPC đó.
144. Nếu ta tắt một EC2 Instance thì khi bật lại EC2 Instance này có thể sẽ đc chạy trên một host khác mà ko phải host ban đầu. Tuy nhiên Elastic IP thì sẽ ko bị thay đỏi với các EC2 (trừ EC2 Classic)
145. Trong trường hợp chạy RDS nhưng mà hết sotarge để lưu trữ mà cần phải tăng dung lượng lưu trữ nhưng ko muốn ảnh hưởng đến database performance của nó và ko bị downtime thì ta modifey DB instance à bật tính năng storage autoscaling lên.
146. Mặc đinh KInesis data chỉ lưu record trong vòng 24h, ta có thể tăng thời gian lưu lên đến 1 năm
147. So với EFS, EBS có lowest-latency access to data from a single EC2 instance tốt hơn. and up to 16TB.
148. Muốn tăng performance của DynamoDB có thể dùng DynamoDB Auto Scaling.
149. Auto scaling group thưường là để scale EC2, và ko dùng cho scale DB.
150. Với những trường hợp mà dữ liệu không quan trọng hoặc có thể dễ dàng tạo lại (reproducible) thì lựa chọn One zone AI là lựa chọn cost-effective nhất.
151. Cloudwatch Logs Insight cho phép tích hơp tìm kiếm và phân thích logs trên Cloudwatch logs.
152. Cloudtrails cung cấp các thông tin về hành động của user, role, hoặc aws service trong S3, Trong khi đó S3 event access logs cung cấp các thông tin chi tiết hơn cho các request được gọi đến S3 (vd: bucket name, request time, request action, referrer, turnaround time, và error code information).
153. Với S3 select có thể dễ dàng query và filter content của s3 object và retrieve just the subset of data you need. Bằng cách này ta có thể dễ dàng giảm lượng dữ liêu mà S3 truyền và giúp giảm chi phí và độ trễ đễ truy xuất dữ liệu. Select work với CSV, JSON ... Apache Parquet format, GZIP, BZIP2.
154. IOPS SSD cũng cấp tối đa volume size và IOPS theo tỉ lệ 50:1 (tức là 100GB sẽ có 5000 iops)
155. Transite-gateway có thể kết nối nhiều VPC, VPN, DX, hoặc là có thể peering giữa nhiều Transite gateway với nhau.
156. Để tích hợp LDAP vào AWS VPC sử dụng IAM thì ta có thể dụng một STS  hoặc SAML đứng giữa để cung cấp token truy cập vào resource của AWS khi mà đã  được verify từ LDAP dưới on-premises.
157. API Gateway  hỗ trợ X-Ray  dùng để tracing và analyze user request ... khi mà họ duy chuyển qua lại giữa Api-Gateway to the underlying services. Thông tin này giống như logs proxy hay lấy trên nginx hoặc traffic-server.'
158. Nếu muốn lưu trữ Cert CA của bên thứ ba thì có thể lưu trữ vào Certificate manager hoặc IAM Certificate store
159. Khi mà dùng DX kết nối giữa on-primese với aws thì mọi traffic qua lại giữa 2 cái là private, ko phải public.
160. Tranfer Acceleration là một tính năng giúp upload nhanh hơn dựa trên sức mạnh của các node cloudfront edge. Giúp tăng tốc từ 50-500%
161. Để Disaster Recovery cho Redshift ta Enable Cross-Region Snapshot copy snapshot sang một regions khác.
162. Trong instance-store để có thể dùng với I/O cao nhất thì ta nên chọn RAID0
163. Aws Wavelenghth kết hợp hight bandwith với ultralow latenct 5Geo
164. Với EKS để có thể truy cập vào cụm EKS thì có thể IAM cho kubernetes. Để xác thực nó sẽ lấy thông tin từ aws-auth Configmap. Ngoài ra có thể sử dụng aws-auth Configmap này để thêm quyền truy cập kiểm soát truy cập dựa trên RBAC.
165. Đối với trường hợp chạy HPC mà cần tăng thêm network thì sử  dụng ENA thay vì EFA, vì Elastic Fabic không hỗ trợ cho hệ điều hành windows.
166. Khi up object lên S3 mà muốn nó chuyển ngay sang một class khác thì ta set lifecycle policy transition data after 0 day.
167. Câu hỏi  về service ETL thì nghĩ ngay tới Glue nếu có.
168. Với trường họp đăng nhập vào cloud HSSM quá giới hạn, khi HSM is zeroized thì tất cả các key và service và tất cả data trên HSM đều bị xóa, và không có cách nào để có thể khôi phục hoặc lấy lại.
169. ELB có thể health check bằng http và https.
170. Có thể dùng Aws Workspace để tạo máy tinh ảo (virtula desktop)trên cloud.
171. Trusted Advisord là noojht tool real-time giups quản lý resource một cách tốt nhất. Nó đứa ra các recommend giúp tiết kiệm chi phi, cải thiện performance, security hệ thống và theo dõi các limit của hệ thống.
172. Trong SQS có thông số ApproximateAgeOfOldestMassage thường sử dụng cho các trường hợp cần đảm bảo các thông báo được xử lý trong một khoảng thời gian cụ thể. Có thể sử dụng chỉ số này để đặt cảnh báo cho cloudwatch hoặc trigger gọi một hành đông nào đó, ví dụ như tăng EC2 khi mà số này quá cao để tăng khả năng xử lý.
173. Với các  instance mà được khởi tạo do ASG, khi mà muốn truy cập vào các instance ta có thể sử dụng run command trên console của aws. (System Manager run command)
174. Application Load-balance có thể load banlance theo % weigh cho cả on-primese và aws
175. Để có thể giảm chi phí trong SQS có thể dùng longpoll để giảm lại số lần poll liên tiếp từ SQS và set ReceiveMesssageWaittimeSecond lớn hơn 0.
176. warm-up trong ASG sẽ cho phép kiểm soát thời gian cho đến khi phiên bản khỏi động hoàn thành, và khi đó Instance mới được coi là một thành viên trong ASG và sẽ đc route traffic vào nó.
177. Khi tạo ASG nhưng nó không hoạt động thì có thể xem phần healcheck đã có config chưa, nếu chưa thì nó sẽ ko thể hd được.
178. KHông có limit số lượng Instance trên placement group. Lâu lâu khi add thêm instance và placement group sẽ bị lỗi thì stop và start lại thì sẽ được :))
179. Để encrypt data giữa RDS và EC2 thì ta có thể dùng ssl để encrypt kết nối giữa 2 thành phần này với param rds.force_ssl
180. Với trường hợp muons ingest và analyze data in real-time và milisecond response time thì hãy chú ý tới Kinesis data stream + DynamoDB. Vì Redshift chỉ procces data near-realtime.
181. Với trường hợp để tối ưu chi phí mà ko yêu cầu HA thì nên deploy các service trong cùng một AZ để giảm chi phí network tranfer, vì trong cùng AZ thì network tranfer free.
182. TRong Elastic IP sẽ không bị tính tiền nếu gắn nó tới một instance, instance đang được gắn ElasticIP đó đang chạy, và instance đó chỉ gắn một elasticIP. Và sẽ bị tính tiền nếu gắn rồi mà tắt hoặc xóa đi.
183. Trường hợp mà dùng cloudfrond nhưng request vẫn về origin quá nhiều thì hãy xem xét thử Cache-control max age có set 0 hay ko, nếu có thì tăng nó lên.
184. Trong EFS ta có thể set Max I/O cho các trường hợp cần sử dụng I/O nhiều.
185. Để tối ưu tốc độ xử lý cho Athena ta có thể partition data, compress data, convert it to columar format such as Apache Parquet.
186. Có thể sử dụng AWS Network fire để kiểm soát các luồng dữ liệu qua lại giữa các VPC, hạn chế tình trạng bị tấn công mạng.
187. Để tối ưu khả năng download từ S3 ta sử dụng S3 Byte-range function.
===================================REFERENCE=============================================
https://towardsaws.com/aws-solution-architect-associate-certification-study-notes-c32f50503f8a
