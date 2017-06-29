# Hệ thống FileSystem

# Mục Lục
## [1. File System Windows](#fwd)
## [2. File System Linux](#fln)
### [2.1 Journaling](#21)
### [2.2 Các tùy chọn File System](#22)
## [3. Khái niệm cơ bản trong Storage](#kncb)
### [3.1 MBR](#31)
### [3.2 GPT](#32)
## [4. Tài liệu tham khảo](#tl)

<a name=fwd></a>
## 1. File System Windows

Các hệ thống file trong windows gồm 2 hệ thống chính là FAt và NTFS, trong đó NTFS là hệ thống file có nhiều đặc tính hiện đại mà hệ thống FAT không có.

- FAT16: Với HĐH MS-DOS, hệ thống tập tin FAT (FAT16 – để phân biệt với FAT32) được công bố vào năm 1981 đưa ra một cách thức mới về việc tổ chức và quản lý tập tin trên đĩa cứng, đĩa mềm. Tuy nhiên, khi dung lượng đĩa cứng ngày càng tăng nhanh, FAT16 đã bộc lộ nhiều hạn chế. Với không gian địa chỉ 16 bit, FAT16 chỉ hỗ trợ đến 65.536 liên cung (clusters) trên một partition, gây ra sự lãng phí dung lượng đáng kể (đến 50% dung lượng đối với những ổ đĩa cứng trên 2 GB).
- FAT32: được giới thiệu trong phiên bản Windows 95 Service Pack 2 (OSR 2), được xem là phiên bản mở rộng của FAT16. Do sử dụng không gian địa chỉ 32 bit nên FAT32 hỗ trợ nhiều cluster trên một partition hơn, do vậy không gian đĩa cứng được tận dụng nhiều hơn. Ngoài ra với khả năng hỗ trợ kích thước của phân vùng từ 2GB lên 2TB và chiều dài tối đa của tên tập tin được mở rộng đến 255 ký tự đã làm cho FAT16 nhanh chóng bị lãng quên. Tuy nhiên, nhược điểm của FAT32 là tính bảo mật và khả năng chịu lỗi (Fault Tolerance) không cao.

![fs](/HaiVD/Storage/images/fat.png)

- NTFS (New Technology File System): được giới thiệu cùng với phiên bản Windows NT đầu tiên (phiên bản này cũng hỗ trợ FAT32). Với không gian địa chỉ 64 bit, khả năng thay đổi kích thước của cluster độc lập với dung lượng đĩa cứng, NTFS hầu như đã loại trừ được những hạn chế về số cluster, kích thước tối đa của tập tin trên một phân vùng đĩa cứng.
- NTFS sử dụng bảng quản lý tập tin MFT (Master File Table) thay cho bảng FAT quen thuộc nhằm tăng cường khả năng lưu trữ, tính bảo mật cho tập tin và thư mục, khả năng mã hóa dữ liệu đến từng tập tin. Ngoài ra, NTFS có khả năng chịu lỗi cao, cho phép người dùng đóng một ứng dụng “chết” (not responding) mà không làm ảnh hưởng đến những ứng dụng khác. Tuy nhiên, NTFS lại không thích hợp với những ổ đĩa có dung lượng thấp (dưới 400 MB) và không sử dụng được trên đĩa mềm.
- NTFS: Có những cải tiến kĩ thuật trên FAT và HPFS, cải thiện khả năng hỗ trợ cho các metadata và sử dụng các cấu trúc dữ liệu tiên tiến để cải thiện hiệu suất. Thêm vào đó là phần mở rộng bổ sung chẳng hạn như kiểm soát truy cập bảo mật danh sách (ACL) và filesystem journaling.

**So sánh FAT32 và NTFS**

NTFS là hệ thống file tiên tiến hơn rất nhiều so với FAT32. Nó có đầy đủ các đặc tính của hệ thống file hiện đại và FAT32 không hề có.Một số đặc tính mà FAT32 không bằng NTFS :
- FAT32 không hỗ trợ các tính năng bảo mật như phần quyền quản lý, mã hoá.. như NTFS. Vấn đề này đặc biệt hiệu quả đối với Windows. Với NTFS, bạn có thể không cần sử dụng các tiện ích mã hoá hay đặt mật khẩu giấu thư mục v.v, vì đây là đặc tính đã có sẵn của NTFS, chỉ cần bạn biết khai thác. Việc xài các tiện ích không nằm sẵn trong hệ điều hành để thao tác trực tiếp với đĩa vẫn có ít nhiều rủi ro.
- FAT32 có khả năng phục hồi và chịu lỗi rất kém so với NTFS. NTFS là hệ thống file có khả năng ghi lại được các hoạt động mà hệ điều hành đã và đang thao tác trên dữ liệu, nó có khả năng xác định được ngay những file bị sự cố mà không cần phải quét lại toàn bộ hệ thống file, giúp quá trình phục hồi dữ liệu trở nên tin cậy và nhanh chóng hơn. Đây là ưu điểm mà FAT 32 hoàn toàn không có.
- Khi mà mất điện đột ngột thì Windows 98, 2000, XP… đều phải quét lại đĩa khi khởi động lại nếu đĩa đó được format bằng chuẩn FAT32. Trong khi format đĩa cứng bằng NTFS thì lại hoàn toàn không cần quét đĩa lại, bởi vì hệ thống dùng NTFS có được những thông tin về tính toàn vẹn dữ liệu ghi trên đĩa và nó mất rất ít thời gian để biết được về mặt logic đĩa của mình có lỗi hay không và nếu có thì hệ thống cũng tự phục hồi một cách cực kỳ đơn giản và nhanh chóng. Với FAT32 thì nó phải rà quét toàn bộ lâu hơn nhiều. Một hệ thống Windows 2000, XP sẽ ổn định hơn nhiều nếu cài trên phân vùng được format bằng NTFS. Ngoài ra NTFS còn được trang bị công cụ kiểm tra và sửa đĩa rất tốt của Microsoft.
- NTFS có khả năng truy cập và xử lý file nén như truy cập vào các file chưa nén, điều này không chỉ tiết kiệm được đĩa cứng mà còn gia tăng được tuổi thọ của đĩa cứng.

<a name=fln></a>
## 2. File System Linux

Filesystem là 1 phương thức để lưu trữ dữ liệu bằng cách cung cấp các thủ tục để lưu trữ, truy xuất và cập nhật giới thiệu, cũng như quản lý các không gian có sẵn trên thiết bị có chứa nó. Một filesystem tổ chức dữ liệu 1 cách hiệu quả và được điều chỉnh để phù hợp với các đặc điểm cụ thể của thiết bị. các phân vùng ổ đĩa khác nhau có thể được thiết lập bằng cách sử dụng một trong nhiều loại filesystem có sẵn khác nhau. Mỗi loại có những lợi thế riêg cũng như như yếu điểm riêng.

<a name=21></a>
**Journaling**

![fs](/HaiVD/Storage/images/journal.jpg)

- Journaling chỉ được sử dụng khi dữ liệu ghi lên ổ cứng và đóng vai trò như những đục lỗ để ghi nhận thông tin vào phân vùng. Đồng thời , nó cũng khắc phục vấn đề xảy ra khi ổ cứng gặp lỗi trong quá trình này, nếu không có journal thì hệ điều hành sẽ không thể biết được file dữ liệu có được ghi đầy đủ tới ổ cứng hay không.
- Journal hoạt động nôm na như sau : trước tiên file sẽ được ghi vào journal , đẩy vào bên trong lớp quản lý dữ liệu, sau đó journal ssex ghi file đó vào phân vùng ổ cứng khi đã sẵn sang.Và khi thành công, file sẽ được xóa bỏ khỏi journal , đẩy ngược ra bên ngoài và quá trình hoàn tất. Nếu xảy ra lỗi khi thực hiện thì file hệ thống có thể kiểm tra lại journal và tất cả các thao tác chưa được hoàn tất, đông thời ghi nhớ lại đúng vị trí xảy ra lỗi đó .
- Tuy nhiên việc sử dụng journaling gây ảnh hưởng đến hiệu suốt trong việc ghi dữ liệu với tính ổn định.

Journal có 3 chế độ : journal, writeback và odered .
- Mode `data= writeback` : quá trình khởi động nhanh, dữ liệu được ghi vào đĩa ngay sau khi đã ghi xong thông tin trong journal log (write back), với mode này đôi khi cũng xảy ra tình trạng dữ liệu bị hư nếu sự cố xảy ra ngay sau khi ghi journal log mà chưa kịp ghi vào đĩa, nhưng bù lại tốc độ thao tác file nhanh hơn trong một vài trường hợp.
- Mode `data= odered` :  dữ liệu được ghi lên đĩa trước rồi mới đến journal log, cho phép luôn luôn bảo đảm tính toàn vẹn của dữ liệu trong mọi tình huống và đây cũng chính là chế độ mặc định của ext3.
- Mode `data= journal` : việc bảo vệ được thực hiện trên cả hai: dữ liệu và journal log; thông tin được ghi chi tiết và nhiều hơn giúp cải thiện tốc độ truy cập dữ liệu nhờ tối ưu việc di chuyển của đầu từ, hoạt động rất tốt đối với kiểu dữ liệu là database hoặc dữ liệu dùng chung trên mạng (NFS). Tuy nhiên do phải đọc lại nhiều loại thông tin trên journal log nên thời gian khởi động lại máy hơi chậm hơn so với hai chế độ trên một chút.

<a name=22></a>
**Các tùy chọn file system**
- Ext – Extended file system: là định dạng file hệ thống đầu tiên được thiết kế dành riêng cho Linux. Có tổng cộng 4 phiên bản và mỗi phiên bản lại có 1 tính năng nổi bật. Phiên bản đầu tiên của Ext là phần nâng cấp từ file hệ thống Minix được sử dụng tại thời điểm đó, nhưng lại không đáp ứng được nhiều tính năng phổ biến ngày nay. Và tại thời điểm này, chúng ta không nên sử dụng Ext vì có nhiều hạn chế, không còn được hỗ trợ trên nhiều distribution.

**Ext2**
- Ext2 : thực chất không phải là file hệ thống journaling, được phát triển để kế thừa các thuộc tính của file hệ thống cũ, đồng thời hỗ trợ dung lượng ổ cứng lên tới 2 TB. Ext2 không sử dụng journal cho nên sẽ có ít dữ liệu được ghi vào ổ đĩa hơn. Ext2 dùng cấu trúc dữ liệu được gọi là nút định dạng (inode) để tham chiếu và định vị tập tin cũng như các dữ liệu tương ứng. Bảng inode chứa các thông tin gồm loại tập tin, kích thước, quyền truy cập, con trỏ đến những khối dữ liệu liên quan và các thuộc tính khác.Do lượng yêu cầu viết và xóa dữ liệu khá thấp, cho nên rất phù hợp với những thiết bị lưu trữ bên ngoài như thẻ nhớ, ổ USB...
- Cấu trúc dữ liệu của Ext2 là các block, kích cỡ các block thay đổi phụ thuộc vào hệ thống tập tin . Mỗi block lại được chia nhỏ hơn thành các fragment .
- Block đầu tiên trong mỗi partion Ext3 không bao giờ được quản lý bởi hệ thống tệp tin Ext2, bỏi vì nó được dùng cho partition boot sector . Phần còn lại của partition Ext2 được chia thành các block group , được biểu diễn như sau :

![fs](/images/ext2.png)

- Tất cả các block group đều có cùng kích thược và lưu trữ liên tiếp nhau. Vị trí đầu của mỗi block group là các cấu trúc dữ liệu khác nhau liệt kê đường dẫn của phần còn lại, cung cấp nhiều thông tin hơn, cấu trúc dữ liệu xác định trạng thái hệ thống tệp tin hiện hành.
- Các block group làm tăng sự phân mảnh tệp tin.
- Supper block chứa thông tin về kích thước các block , số  block , i node trên 1 block group . Nếu không có thông tin về supper block thì đĩa sẽ vô ích, do đó hãy sao lưu supper block để dự phòng.
- Block bitmap và Inode bitmap được dùng để xác định block nào và các entry inode nào trống để sử dụng.các Data block là nơi lưu trữ nhiều loại tập tin.
- Inode Table dùng để giữ rãnh ghi của mỗi tập tin : vị trí của nó, kích thước, kiểu và thứ tự truy xuất đêu được lưu trữ trong các inode. Tên tập tin không được cất giữ ở đây, với vùng Inode Table tất cả các tập tin được tham chiếu đến số inode của chúng.

- Ext3 về căn bản chỉ là Ext2 đi kèm với journaling. Mục đích chính của Ext3 là tương thích ngược với Ext2, và do vậy những ổ đĩa, phân vùng có thể dễ dàng được chuyển đổi giữa 2 chế độ mà không cần phải format như trước kia.Directory chứa tối đa 32000 subdirectory. Tuy nhiên, vấn đề vẫn còn tồn tại ở đây là những giới hạn của Ext2 vẫn còn nguyên trong Ext3, và ưu điểm của Ext3 là hoạt động nhanh, ổn định hơn rất nhiều. Không thực sự phù hợp để làm file hệ thống dành cho máy chủ bởi vì không hỗ trợ tính năng tạo disk snapshot và file được khôi phục sẽ rất khó để xóa bỏ sau này.
- Ext4: cũng giống như Ext3, lưu giữ được những ưu điểm và tính tương thích ngược với phiên bản trước đó. Như vậy, chúng ta có thể dễ dàng kết hợp các phân vùng định dạng Ext2, Ext3 và Ext4 trong cùng 1 ổ đĩa trong Ubuntu để tăng hiệu suất hoạt động. Ext4 Hỗ trợ nhiều tính năng mới tăng performance và độ tin cậy (reliability) như multiblock allocation, delayed allocation, journal checksum. fast fsck, etc.Trên thực tế, Ext4 có thể giảm bớt hiện tượng phân mảnh dữ liệu trong ổ cứng, hỗ trợ các file và phân vùng có dung lượng lớn... Thích hợp với ổ SSD so với Ext3, tốc độ hoạt động nhanh hơn so với 2 phiên bản Ext trước đó, cũng khá phù hợp để hoạt động trên server, nhưng lại không bằng Ext3.
- XFS được phát triển bởi Silicon Graphics từ năm 1994 để hoạt động với hệ điều hành riêng biệt của họ, và sau đó chuyển sang Linux trong năm 2001. Khá tương đồng với Ext4 về một số mặt nào đó, chẳng hạn như hạn chế được tình trạng phân mảnh dữ liệu, không cho phép các snapshot tự động kết hợp với nhau, hỗ trợ nhiều file dung lượng lớn, có thể thay đổi kích thước file dữ liệu... nhưng không thể shrink – chia nhỏ phân vùng XFS. XFS là hệ thống file 64 bit, nó có thể quản lý được file có kích thước là 264-1 byte = 9 Exabyte  (do sử dụng số nguyên có dấu nên 1 bit dùng để biểu thị dấu), có kèm theo công cụ  Volume Manager cho phép quản lý lên tới 128 Volume, mỗi Volume có thể được ghép lên tới 100 partition đĩa cứng vật lý, hỗ trợ chức năng journaling đối với dữ liệu.Một đặc tính quan trọng của XFS đó là khả năng bảo đảm tốc độ truy cập dữ liệu cho các ứng dụng (Guaranteed Rate I/O),  cho phép các ứng dụng duy trì được tốc độ truy xuất dữ liệu trên đĩa, rất quan trọng đối với các hệ thống phân phối dịch vụ video có độ phân giải cao hoặc các ứng dụng xử lý thông tin vệ tinh đòi hỏi duy trì ổn định tốc độ thao tác dữ liệu. Kernel Linux 2.4.17 trở lên hỗ trợ rất tốt đối với hệ thống file này, để sử dụng cần phải patch lại kernel với các module của XFS. Với những đặc điểm như vậy thì XFS khá phù hợp với việc áp dụng vào mô hình server media vì khả năng truyền tải file video rất tốt. Tuy nhiên, nhiều phiên bản distributor yêu cầu phân vùng /boot riêng biệt, hiệu suất hoạt động với các file dung lượng nhỏ không bằng được khi so với các định dạng file hệ thống khác, do vậy sẽ không thể áp dụng với mô hình database, email và một vài loại server có nhiều file log. Nếu dùng với máy tính cá nhân, thì đây cũng không phải là sự lựa chọn tốt nên so sánh với Ext, vì hiệu suất hoạt động không khả thi, ngoài ra cũng không có gì nổi trội về hiệu năng, quản lý so với Ext3/4.

Đây là bảng so sánh các File system

![fs](/HaiVD/Storage/images/ss.png)


<a name=kn></a>
**Các khái niệm cơ bản trong Storage**
- Block storage: Block storage hay còn trong Linux còn gọi là Block device. Block device là 1 phần của phần cứng dùng để lưu trữ dữ liệu, giống như 1 ổ đĩa cứng truyền thống (HDD), (SSD). Được gọi là block bởi vì kernel tương tác với phần cứng bằng cách tham khảo các block cố định hoặc các khối của không gian Về cơ bản, block storage được xem như là lưu trữ trên ổ đĩa của 1 máy tính. Một khi đã được set up, về cơ bản nó sẽ hoạt động như 1 phần mở rộng của filesystem tree và có thể viết và đọc thông tin từ ổ 1 cách liền mạch.
- Disk Partition: Disk partition là 1 cách để phân vùng ổ đĩa thành các đơn vị nhỏ hơn có giá trị sử dụng. Một partition là 1 phần của thiết bị lưu trữ có thể được điều chỉnh cho giống như ổ đĩa đó Partition cho phép người dùng có thể chia các không gian có sẵn và sử dụng chúng vào các mục đích khách nhau cho mỗi partition Điều này cho phép người sử dụng có rất nhiều tính linh hoạt cho người sử dụng, cho phép linh hoạt, họ có thể dễ dàng phân khúc cài đặt và nâng cấp.

Có 2 loại định dạng ổ đĩa : MBR(Master Boot Record) và GPT(GUID Partition Table).

<a name=31></a>
**MBR**:
- Là 1 hệ thống phân vùng truyền thống, tuy nhiên có nhiều hạn chế. Nó không thể sử dụng cho đĩa có dung lượng lớn hơn 2TB, nó chỉ có duy nhất 4 phân vùng chính bởi vậy phân vùng thứ 4 thường được sử dụng để làm extend partition, trong đó logical partition có thể được tạo ra.
- Phân vùng chưa MBR rất nhỏ chiếm dung lượng khoảng vài MB, nó là nơi lưu trữ các thông tin về phân vùng và một chương trình rất nhỏ (446 byte) chương trình này sẽ chịu trách nhiệm tìm và một chương trình nhỏ nhưng mạnh hơn nó trên phân vùng Active vào bộ nhớ, chương trình này sẽ tiến hành tìm và nạp hệ điều hành vào bộ nhớ. Thông thường, MBR được ghi ở Sector 0 nằm đầu tiên và bên ngoài cùng của đĩa cứng.

![fs](/HaiVD/Storage/images/mbr.png)

- Khi MBR bị hỏng thì chúng ta sẽ không thấy được các phân vùng đã được chia trước đó và cũng không thể chia lại chúng. Để làm việc với các phân vùng này bắt buộc chúng ta phải sửa MBR hay Rebuild MBR bằng các công cụ có trong HirenBoot.
- Phân vùng ổ cứng (Partition), là một không gian lưu trữ dữ liệu trên ổ cứng. Một ổ đĩa cứng vật lý có thể chia thành nhiều phân vùng cho việc cài đặt hệ điều hành, tổ chức lưu trữ dữ liệu mà chúng ta thường gọi chúng là các ổ đĩa. Có hai loại phân vùng trên ổ cứng định dạng MBR là Primary Partition và Extended Partition.

![fs](/HaiVD/Storage/images/mbr2.png)

- Primary Partition (Phân vùng chính): Primary Partition là phân vùng nơi hệ điều hành được cài vào đó, trong một ổ đĩa cứng có thể có tối đa 4 phân Primary Partition, nhưng chỉ có 1 trong 4 phân vùng này có thể là Active Partition. Phân vùng Active Partition là phân vùng chưa boot loader được nạp vào quá trình khởi động của hệ điều hành.
- Extended Partition (Phân vùng mở rộng): Vì trên ổ đĩa cứng (MBR) chỉ có thể có tối đa 4 phân vùng Primary, điều này làm cho việc phân chia phân vùng gặp nhiều hạng chế. Người ta đưa ra Extended Partition, với Extended Partition ta có thể chia nhỏ nó ra để tạo không giới hạn các Logical Partition, các Logical Partition ngầm định là các Extended Partion. Ngoài ra mỗi phân vùng không thể có kích thước lớn hơn 2TB, đây cũng là một nhược điểm của MBR.
- Partition table: là nơi lưu trữ các thông tin về phân vùng như Primary, Extended và Logical Partition. Nhiều hơn nữa là Create/delete/resize của bất kỳ thay đổi nào của phân vùng sẽ được lưu vào Partiton Table để ghi nhận những thay đổi. Nếu trong trường hợp bị virus tấn công hoặc một tác động khác làm hỏng Partition table thì tất cả các partition sẽ mất, thông tin trong Partiton table là rất quan trọng.

<a name=32></a>
**GPT**
- Là 1 kĩ thuật phân vùng mới, nó đã khắc phục hiệu quả những hạn chế của MBR. Hệ thống sử dụng GPT có thể có nhiều hơn những phân vùng trên 1 ổ đĩa. Hơn nữa, GPT không giới hạn kích thước đĩa và bảng partition table có sẵn tại multiple location nhằm chống lại sự cố hỏng hóc. Trong nhiều trường hợp, GPT là lựa chọn tốt hơn so với MBR.
- Trên ổ đĩa MBR , dữ liệu phân vùng và khởi động chỉ được lưu trữ ở một vị trí, chính vì vậy, khi xảy ra sự cố thì ổ cứng theo chuẩn MBR rất dễ bị hỏng.Ổ cứng chuẩn GPT đã giải quyết vấn đề này, các GPT srtore có nhiều bản sao của Dữ liệu ,vì vậy nó mạnh mẽ hơn và có thể phục hồi dữ liệu khi bị hỏng.GPT cũng lưu dự phòng giá  trị kiểm tra theo chu kì (CRC) để kiểm tra xem dữ liệu có nguyên vẹn hay không .,nếu dữ liệu bị mất GPT có thể nhận thấy và khắc phục vấn đề và cố gắng phục hồi dữ liệu bị hỏng.Nó cũng thực hiện checksum băng CRC32 để phát hiện lỗi và những sai hỏng của header và pation table.

![fs](/HaiVD/Storage/images/gpt.png)

- Theo như hình trên, có 1 primary GPT ở đầu và 1 secondary GPT ở cuối. Đây là ưu điểm hơn so với MBR .GPT lưu trữ và backup header và partition table ở cuối ổ đĩa để nó có thể được phục hồi nếu primary table bị hỏng.
- Có 1 phân vùng gọi là `protective MBR` ở sector đầu tiên của ổ cứng trong lược đồ.Thiết lập lại như vậy cho phép hệ thống BIOS có thể khởi động từ GPT sử dụng boot loader lưu trữ trong các vùng mã của Protective MBR .Ngoài ra nó còn dùng để bảo vệ GPT tránh những hư hại do các tiện ích đĩa không hiểu GPT.

![fs](/HaiVD/Storage/images/gpt2.png)

- Một ổ đĩa được chia làm nhiều LBA (Logical Block Addressing).

Thông thường, một LBA có kích thước là 512 byte, tuy nhiên kích thước có thể thay đổi lên đến 1024 byte hoặc 2048 byte :
<ul>
<li>LBA đầu tiên sẽ có cấu trúc giống một ổ đĩa dạng MBR, nhằm giúp các phần mềm dựa trên MBR có thể “hiểu” được GPT nhằm tránh ghi đè.</li>
<li>LBA 1 sẽ gồm các header chứa GUID và thông tin về dung lượng, vị trí phân vùng.</li>
<li>Các LBA tiếp theo (2-33) chứa các GUID tương ứng với các phân vùng.</li>
<li>Một phiên bản của các LBA 1-33 sẽ được sao lưu ở vùng dữ liệu cuối của ổ đĩa.</li>
</ul>

- Các phân vùng sẽ nằm sau LBA33, số lượng phân vùng trên lý thuyết có thể đạt đến vô hạn. Mỗi phân vùng sẽ được gán một GUID (Globally Unique Identifier) để đảm bảo tính duy nhất của các phân vùng.

<a name=tl></a>
## 4. Tài liệu tham khảo
- https://www.tienichmaytinh.com/tin-tuc/tim-hieu/742-tim-hieu-ve-dinh-dang-phan-vung-gpt-tren-dia-cung
- https://www.tienichmaytinh.com/tin-tuc/tim-hieu/741-tim-hieu-ve-mbr-cac-phan-vung-trong-dia-cung-dinh-dang-mbr
- http://2mit.org/forum/threads/cac-he-thong-file-trong-windows.2167/
- http://doc.edu.vn/tai-lieu/tieu-luan-cau-truc-he-thong-file-ext2-ext3-ext4-trong-ho-linux-7211/
