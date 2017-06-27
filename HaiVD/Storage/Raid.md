# Tìm hiểu về RAID

# Mục Lục
## [I. Giới thiệu](#gt)
## [II. Phân Loại](#pl)
### [2.1 RAID 0](#21)
### [2.2 RAID 1](#22)
### [2.3 RAID 10](#23)
### [2.4 RAID 2](#24)
### [2.5 RAID 3](#25)
### [2.6 RAID 4](#26)
### [2.7 RAID 5](#27)
### [2.8 RAID 6](#28)
## [III. Những điều cần thiết khi xây dựng RAID](#vd)


<a name=vd></a>
## I. Giới thiệu
- RAID (Redundant Arrays of Inexpensive Disks) là hình thức ghép nhiều ổ đĩa cứng vật lý thành một hệ thống ổ đĩa cứng có chức năng gia tăng tốc độ đọc/ghi dữ liệu hoặc nhằm tăng thêm sự an toàn của dữ liệu chứa trên hệ thống đĩa hoặc kết hợp cả hai yếu tố trên.

- Lần đầu tiên RAID được phát triển năm 1987 tại trường Đại học California tại Berkeley (Hoa Kỳ) với những đặc điểm chỉ ghép các phần đĩa cứng nhỏ hơn thông qua phần mềm để tạo ra một hệ thống đĩa dung lượng lớn hơn thay thế cho các ổ cứng dung lượng lớn giá đắt thời bấy giờ.

<a name=pl></a>
## II. Phân loại

- Ban đầu, RAID được sử dụng như một giải pháp phòng hộ vì nó cho phép ghi dữ liệu lên nhiều đĩa cứng cùng lúc. Về sau, RAID đã có nhiều biến thể cho phép không chỉ đảm bảo an toàn dữ liệu mà còn giúp gia tăng đáng kể tốc độ truy xuất dữ liệu từ đĩa cứng.

- Theo RAB thì RAID được chia thành 7 cấp độ (level), mỗi cấp độ có các tính năng riêng, hầu hết chúng được xây dựng từ hai cấp độ cơ bản là RAID 0 và RAID 1.

<a name=21></a>
### 2.1 RAID 0

![r](/HaiVD/Storage/images/Raid0.png)

- Đây là dạng RAID đang được người dùng ưa thích do khả năng nâng cao hiệu suất trao đổi dữ liệu của đĩa cứng. Đòi hỏi tối thiểu hai đĩa cứng, RAID 0 cho phép máy tính ghi dữ liệu lên chúng theo một phương thức đặc biệt được gọi là Striping. Ví dụ bạn có 8 đoạn dữ liệu được đánh số từ 1 đến 8, các đoạn đánh số lẻ (1,3,5,7) sẽ được ghi lên đĩa cứng đầu tiên và các đoạn đánh số chẵn (2,4,6,8) sẽ được ghi lên đĩa thứ hai. Để đơn giản hơn, bạn có thể hình dung mình có 100MB dữ liệu và thay vì dồn 100MB vào một đĩa cứng duy nhất, RAID 0 sẽ giúp dồn 50MB vào mỗi đĩa cứng riêng giúp giảm một nửa thời gian làm việc theo lý thuyết. Từ đó bạn có thể dễ dàng suy ra nếu có 4, 8 hay nhiều đĩa cứng hơn nữa thì tốc độ sẽ càng cao hơn. Tuy nghe có vẻ hấp dẫn nhưng trên thực tế, RAID 0 vẫn ẩn chứa nguy cơ mất dữ liệu. Nguyên nhân chính lại nằm ở cách ghi thông tin xé lẻ vì như vậy dữ liệu không nằm hoàn toàn ở một đĩa cứng nào và mỗi khi cần truy xuất thông tin (ví dụ một file nào đó), máy tính sẽ phải tổng hợp từ các đĩa cứng. Nếu một đĩa cứng gặp trục trặc thì thông tin (file) đó coi như không thể đọc được và mất luôn. Thật may mắn là với công nghệ hiện đại, sản phẩm phần cứng khá bền nên những trường hợp mất dữ liệu như vậy xảy ra không nhiều.
- RAID 0 thực sự thích hợp cho những người dùng cần truy cập nhanh khối lượng dữ liệu lớn, ví dụ các game thủ hoặc những người chuyên làm đồ hoạ, video số.
- RAID 0 cần ít nhất 2 ổ đĩa. Tổng quát ta có n đĩa (n >= 2) và các đĩa là cùng loại.
- Dữ liệu sẽ được chia ra nhiều phần bằng nhau để lưu trên từng đĩa. Như vậy mỗi đĩa sẽ chứa 1/n dữ liệu. Tổng dung lượng = dung lượng đĩa nhỏ nhất nhân với tổng số đĩa. Array Capacity = Size of Smallest Drive * Number of Drives Dung lượng tổng cộng của ổ cứng trong hệ thống RAID0 bằng tổng dung lượng của hai ổ đĩa. Nếu chúng ta dùng 02 ổ cứng 80GB thì hệ thống đĩa của chúng ta là 160GB. Ưu điểm: - Tăng tốc độ đọc/ghi đĩa: mỗi đĩa chỉ cần phải đọc/ghi 1/n lượng dữ liệu được yêu cầu. Lý thuyết thì tốc độ sẽ tăng n lần.
- Nhược điểm: - Tính an toàn thấp. Nếu một đĩa bị hư thì dữ liệu trên tất cả các đĩa còn lại sẽ không còn sử dụng được. Xác suất để mất dữ liệu sẽ tăng n lần so với dùng ổ đĩa đơn.

<a name=22></a>
### 2.2 RAID 1

![r](/HaiVD/Storage/images/Raid0.png)

- Đây là dạng RAID cơ bản nhất có khả năng đảm bảo an toàn dữ liệu. Cũng giống như RAID 0, RAID 1 đòi hỏi ít nhất hai đĩa cứng để làm việc. Dữ liệu được ghi vào 2 ổ giống hệt nhau (Mirroring). Trong trường hợp một ổ bị trục trặc, ổ còn lại sẽ tiếp tục hoạt động bình thường. Bạn có thể thay thế ổ đĩa bị hỏng mà không phải lo lắng đến vấn đề thông tin thất lạc. Đối với RAID 1, hiệu năng không phải là yếu tố hàng đầu nên chẳng có gì ngạc nhiên nếu nó không phải là lựa chọn số một cho những người say mê tốc độ.
- Tuy nhiên đối với những nhà quản trị mạng hoặc những ai phải quản lý nhiều thông tin quan trọng thì hệ thống RAID 1 là thứ không thể thiếu. Nếu có sự hư hỏng ổ cứng xảy ra, người quản trị hệ thống có thể dễ dàng thay thế ổ đĩa hư hỏng đó mà không làm dừng hệ thống. RAID 1 thường được kết hợp với việc gắn nóng các ổ cứng (cũng giống như việc gắn và thay thế nóng các thiết bị tại các máy chủ nói chung). Dung lượng cuối cùng của hệ thống RAID 1 bằng dung lượng của ổ đơn (hai ổ 80GB chạy RAID 1 sẽ cho hệ thống nhìn thấy duy nhất một ổ RAID 80GB).


<a name=23></a>
### 2.3 RAID 10

![r](/HaiVD/Storage/images/Raid10.jpg)

![r](/HaiVD/Storage/images/Raid10.gif)

- Hệ thống lưu trữ này nhanh nhẹn như RAID 0, an toàn như RAID 1.  Hệ thống RAID kết hợp 0+1 đã ra đời, tổng hợp ưu điểm của cả hai "đàn anh". Tuy nhiên chi phí cho một hệ thống kiểu này khá đắt, bạn sẽ cần tối thiểu 4 đĩa cứng để chạy RAID 0+1. Dữ liệu sẽ được ghi đồng thời lên 4 đĩa cứng với 2 ổ dạng Striping tăng tốc và 2 ổ dạng Mirroring sao lưu. 4 ổ đĩa này phải giống hệt nhau và khi đưa vào hệ thống RAID 0+1, dung lượng cuối cùng sẽ bằng ½ tổng dung lượng 4 ổ, ví dụ bạn chạy 4 ổ 80GB thì lượng dữ liệu "thấy được" là (4*80)/2 = 160GB


<a name=24></a>
### 2.4 RAID 2

 ![r](/HaiVD/Storage/images/Raid2.jpg)

 ![r](/HaiVD/Storage/images/Raid2.gif)

 RAID 2 gồm hai cụm ổ đĩa, cụm thứ nhất chứa các dữ liệu được phân tách giống như là RAID 0, cụm thứ hai chứa các mã ECC dành cho sửa chữa lỗi ở cụm thứ nhất. Sự hoạt động của các ổ đĩa ở RAID 2 là đồng thời để đảm bảo rằng các dữ liệu được đọc đúng, chính do vậy chúng không hiệu quả bằng một số loại RAID khác nên ít được sử dụng.


<a name=25></a>
### 2.5 RAID 3

 ![r](/HaiVD/Storage/images/Raid3.jpg)

 ![r](/HaiVD/Storage/images/Raid3.gif)

- RAID 3 là sự cải tiến của RAID 0 nhưng có thêm (ít nhất) một ổ cứng chứa thông tin có thể khôi phục lại dữ liệu đã hư hỏng của các ổ cứng RAID 0. Giả sử dữ liệu A được phân tách thành 3 phần A1, A2, A3 (Xem hình minh hoạ RAID 3), khi đó dữ liệu được chia thành 3 phần chứa trên các ổ cứng 0, 1, 2 (giống như RAID 0). Phần ổ cứng thứ 3 chứa dữ liệu của tất cả để khôi phục dữ liệu có thể sẽ mất ở ổ cứng 0, 1, 2. Giả sử ổ cứng 1 hư hỏng, hệ thống vẫn hoạt động bình thường cho đến khi thay thế ổ cứng này. Sau khi gắn nóng ổ cứng mới, dữ liệu lại được khôi phục trở về ổ đĩa 1 như trước khi nó bị hư hỏng.

- Yêu cầu tối thiểu của RAID 3 là có ít nhất 3 ổ cứng.


<a name=26></a>
### 2.6 RAID 4

 ![r](/HaiVD/Storage/images/Raid4.png)

 ![r](/HaiVD/Storage/images/Raid4.gif)

 - RAID 4 tương tự như RAID 3 nhưng ở một mức độ các khối dữ liệu lớn hơn chứ không phải đến từng byte. Chúng cũng yêu cầu tối thiểu 3 đĩa cứng (ít nhất hai đĩa dành cho chứa dữ liệu và ít nhất 1 đĩa dùng cho lưu trữ dữ liệu tổng thể)


<a name=27></a>
### 2.7 RAID 5

![r](/HaiVD/Storage/images/Raid5.jpg)

![r](/HaiVD/Storage/images/Raid5.gif)

- RAID 5 thực hiện chia đều dữ liệu trên các ổ đĩa giống như RAID 0 nhưng với một cơ chế phức tạp hơn.
- Đây có lẽ là dạng RAID mạnh mẽ nhất cho người dùng văn phòng và gia đình với 3 hoặc 5 đĩa cứng riêng biệt. Dữ liệu và bản sao lưu được chia lên tất cả các ổ cứng. Nguyên tắc này khá rối rắm. Chúng ta quay trở lại ví dụ về 8 đoạn dữ liệu (1-8) và giờ đây là 3 ổ đĩa cứng. Đoạn dữ liệu số 1 và số 2 sẽ được ghi vào ổ đĩa 1 và 2 riêng rẽ, đoạn sao lưu của chúng được ghi vào ổ cứng 3. Đoạn số 3 và 4 được ghi vào ổ 1 và 3 với đoạn sao lưu tương ứng ghi vào ổ đĩa 2. Đoạn số 5, 6 ghi vào ổ đĩa 2 và 3, còn đoạn sao lưu được ghi vào ổ đĩa 1 và sau đó trình tự này lặp lại, đoạn số 7,8 được ghi vào ổ 1, 2 và đoạn sao lưu ghi vào ổ 3 như ban đầu. Như vậy RAID 5 vừa đảm bảo tốc độ có cải thiện, vừa giữ được tính an toàn cao. Dung lượng đĩa cứng cuối cùng bằng tổng dung lượng đĩa sử dụng trừ đi một ổ. Tức là nếu bạn dùng 3 ổ 80GB thì dung lượng cuối cùng sẽ là 160GB.
- RAID 5 cũng yêu cầu tối thiểu có 3 ổ cứng.


<a name=28></a>
### 2.8 RAID 6

  ![r](/HaiVD/Storage/images/Raid6.jpg)

  ![r](/HaiVD/Storage/images/Raid61.jpg)

- RAID 6 phần nào giống như RAID 5 nhưng lại được sử dụng lặp lại nhiều hơn số lần sự phân tách dữ liệu để ghi vào các đĩa cứng khác nhau. Ví dụ như ở RAID 5 thì mỗi một dữ liệu được tách thành hai vị trí lưu trữ trên hai đĩa cứng khác nhau, nhưng ở RAID 6 thì mỗi dữ liệu lại được lưu trữ ở ít nhất ba vị trí (trở lên), điều này giúp cho sự an toàn của dữ liệu tăng lên so với RAID 5.

- RAID 6 yêu cầu tối thiểu 4 ổ cứng.
- Trong RAID 6, ta thấy rằng khả năng chịu đựng rủi ro hư hỏng cứng được tăng lên rất nhiều. Nếu với 4 ổ cứng thì chúng cho phép hư hỏng đồng thời đến 2 ổ cứng mà hệ thống vẫn làm việc bình thường, điều này tạo ra một xác xuất an toàn rất lớn. Chính do đó mà RAID 6 thường chỉ được sử dụng trong các máy chủ chứa dữ liệu cực kỳ quan trọng.


<a name=vd></a>

## III. Những điều cần thiết khi xây dựng RAID
- Để chạy được RAID, bạn cần tối thiểu một card điều khiển và hai ổ đĩa cứng giống nhau. Đĩa cứng có thể ở bất cứ chuẩn nào, từ ATA, Serial ATA hay SCSI, tốt nhất chúng nên hoàn toàn giống nhau vì một nguyên tắc đơn giản là khi hoạt động ở chế độ đồng bộ như RAID, hiệu năng chung của cả hệ thống sẽ bị kéo xuống theo ổ thấp nhất nếu có. Ví dụ khi bạn bắt ổ 160GB chạy RAID với ổ 40GB (bất kể 0 hay 1) thì coi như bạn đã lãng phí 120GB vô ích vì hệ thống điều khiển chỉ coi chúng là một cặp hai ổ cứng 40GB mà thôi (ngoại trừ trường hợp JBOD như đã đề cập). Yếu tố quyết định tới số lượng ổ đĩa chính là kiểu RAID mà bạn định chạy. Chuẩn giao tiếp không quan trọng lắm, đặc biệt là giữa SATA và ATA. Một số BMC đời mới cho phép chạy RAID theo kiểu trộn lẫn cả hai giao tiếp này với nhau. Điển hình như MSI K8N Neo2 Platinum hay dòng DFI Lanparty NForce4.
- Bộ điều khiển RAID (RAID Controller) là nơi tập trung các cáp dữ liệu nối các đĩa cứng trong hệ thống RAID và nó xử lý toàn bộ dữ liệu đi qua đó. Bộ điều khiển này có nhiều dạng khác nhau, từ card tách rời cho dến chip tích hợp trên BMC.
- Đối với các hệ thống PC, tuy chưa phổ biến nhưng việc chọn BMC có RAID tích hợp là điều nên làm vì nói chung đây là một trong những giải pháp cải thiện hiệu năng hệ thống rõ rệt và rẻ tiền nhất, chưa tính tới giá trị an toàn dữ liệu của chúng. Trong trường hợp BMC không có RAID, bạn vẫn có thể được card điều khiển PCI trên thị trường với giá không cao lắm.
- Một thành phần khác của hệ thống RAID không bắt buộc phải có nhưng đôi khi là hữu dụng, đó là các khay hoán đổi nóng ổ đĩa. Nó cho phép bạn thay các đĩa cứng gặp trục trặc trong khi hệ thống đang hoạt động mà không phải tắt máy (chỉ đơn giản là mở khóa, rút ổ ra và cắm ổ mới vào). Thiết bị này thường sử dụng với ổ cứng SCSI và khá quan trọng đối với các hệ thống máy chủ vốn yêu cầu hoạt động liên tục.
- Về phần mềm thì khá đơn giản vì hầu hết các hệ điều hành hiện đại đều hỗ trợ RAID rất tốt, đặc biệt là Microsoft Windows. Nếu bạn sử dụng Windows XP thì bổ sung RAID khá dễ dàng. Quan trọng nhất là trình điều khiển nhưng thật tuyệt khi chúng đã được kèm sẵn với thiết bị. Việc cài đặt RAID có thể gây một vài rắc rối nếu bạn thiếu kinh nghiệm nhưng vẫn có hướng giải quyết trong phần sau của bài viết.
- Có hai trường hợp sẽ xảy ra khi người dung nâng cấp RAID cho hệ thống. Nếu hệ thống RAID bổ sung chỉ được dùng với mục đích lưu trữ hoặc làm nơi trao đổi thông tin tốc độ cao thì việc cài đặt rất đơn giản. Tuy nhiên nếu bạn dự định dùng nó làm nơi cài hệ điều hành, phần mềm thì sẽ rất rắc rối và phải cài đặt lại toàn bộ từ con số 0.
