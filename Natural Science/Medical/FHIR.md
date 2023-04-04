---
---

# Abstract

FHIR (Fast Healthcare Interoperability Resource, pronounced “fire”) là chuẩn trao đổi data giữa các hệ thống CNTT áp dụng trong y tế. Giao thức (tiêu chuẩn) này được phát triển bởi HL7 (-> HL7 là 1 tổ chức). 

Hầu hết các nhà cung cấp dịch vụ chăm sóc sức khỏe(Bệnh viện, phòng khám, ...) sử dụng nhiều ứng dụng khác nhau cho mọi việc, từ thanh toán đến cập nhật thông tin bệnh nhân. Vấn đề là mỗi loại phần mềm này được

![[hl7-fhir-diagram.svg]]

## Vì sao cần chuẩn hóa dữ liệu

Để có được bộ dữ tiệu tốt cho công việc khai thác dữ liệu thì cần phải chuẩn hóa dữ liệu từ đầu vào.

> Chú ý: Khai thác dữ liệu ở đây là tất cả các công việc liên quan đến sử dụng dữ liệu. Nó bao gồm cả việc chẩn đoán của bác sĩ, theo dõi, đánh giá dịch bệnh, ...

Nhằm có được bộ dữ liệu tốt, cần có một bộ dữ liệu danh mục được chuẩn hóa. Hiện tại đã có nhiều danh mục chuẩn hóa ở các cấp độ khác nhau: quốc tế, cấp quốc gia và cũng có danh mục chuẩn hóa tại từng bệnh viện

- Danh mục quốc tế: Tên quốc gia, mã số quốc gia, bộ mã chẩn đoán ICD, bộ mã thuốc men ATP, LOINC, SNOMED CT, …
- Danh mục cấp quốc gia: Danh mục thuốc, danh mục tên dịch vụ y tế, danh mục bệnh viện, danh mục tên chuyên khoa, danh mục tỉnh thành…
- Danh mục tự tạo: các khoa phòng, danh mục user, danh mục kho thuốc, danh mục thuốc… là các danh mục do bệnh viện tự tạo.

Việc quy chuẩn danh mục giúp cho việc nhập liệu được nhanh chóng và dễ cho việc phân loại dữ liệu, giúp ích cho việc thống kê và giao tiếp giữa các cơ quan y tế. Tuy nhiên, việc chuẩn hóa các danh mục này cũng chỉ mang tính tương đối vì dữ liệu có thể biến đổi và không bao trùm được hết các khía cạnh thực tế.

Ví dụ:

- ICD 10 có thể ghi chẩn đoán “Viêm Xoang Hàm”, tuy nhiên thực tế cần ghi rõ hơn là viêm xoang hàm trái hay phải, mức độ nặng nhẹ thế nào, ở giai đoạn nào của diễn tiến bệnh.
- Danh mục nghề nghiệp không thể có các nghề như lơ xe, phụ hồ, xe ôm… Khi phát sinh các “nghề” mới, cần phải quy chuẩn các nghề đó vào 1 danh mục đã được định chuẩn.
- Danh mục thuốc cũng phải được cập nhật vì nhiều thuốc mới phát sinh.
- ...

With the emergence of electronic health record systems (EHRs), direct system-to-system secure digital transfer and portability of patient records become possible. Yet, even as we usher out 2022 we’ve still seen data formatting and availability as the wild, wild west. Each system implementer provides different ways to share data, or in some cases, prevent the sharing of data.

Since there had been no mandated universal standard for data exchange, each EHR had its own data specifications – often a hybrid approach using some industry standards and their own custom formats. This format and integration fragmentation put a lot of pressure on vendor integration teams and required custom API implementations for each platform they wished to integrate with. Because every implementation required a time and monetary investment, this meant that the APIs intended to provide data portability, ironically became hurdles for larger integration strategies.

Nhưng có lẽ, câu chuyện về kết nối dữ liệu chỉ thực sự trở nên cấp thiết hơn bao giờ hết khi y tế dự phòng được đẩy mạnh với sự ra đời của PHR - hồ sơ dữ liệu sức khoẻ cá nhân. Dữ liệu không còn nằm ở phía cơ sở y tế, dữ liệu được chia sẻ đến mỗi cá nhân để họ nắm được tình trạng sức khoẻ của mình, điều chỉnh sinh hoạt và phòng bệnh. Bản chất của hệ thống PHR - Hồ sơ sức khoẻ cá nhân là kết nối. PHR kết nối đến tất cả những nơi nào có dữ liệu y tế và sức khoẻ của cá nhân, tập hợp chúng lại để mỗi cá nhân có thể tự quản lý thông tin sức khoẻ của mình đầy đủ nhất. Và cũng nhờ những thông tin y tế đầy đủ đó, dưới sự hỗ trợ của công nghệ số hiện đại, máy tính có thể trợ giúp con người phòng bệnh tốt hơn, đưa ra những lời khuyên về sức khoẻ như những trợ lý sức khoẻ ảo của riêng họ.

## Chuẩn hóa dữ liệu

### Các dạng dữ liệu y tế

Trong các phần mềm hay hệ thống IT phục vụ trong ngành y tế có nhiều tiêu chuẩn dữ liệu khác nhau đã được đưa ra

- EHR cho phép cải thiện tất cả các khía cạnh của việc chăm sóc bệnh nhân, mức độ an toàn, kịp lúc, tính hiệu quả và công bằng.
- Claims data cung cấp thông tin chi tiết bổ sung về phương pháp điều trị và các loại thuốc cho bệnh nhân.
- Hồ sơ quan trọng (Vital record) thông báo các mục tiêu và chính sách y tế công cộng.
- Dữ liệu về đơn thuốc, hình ảnh, công việc xét nghiệm và tất cả các thành phần khác của dịch vụ khám chữa bệnh.
- Nghiên cứu được thực hiện nhằm cải thiện hệ thống hệ thống cung ứng dịch vụ y tế.
- Khảo sát thu thập dữ liệu để thúc đẩy nghiên cứu chính sách khám, chữa bệnh công cộng.
- Thiết bị đeo trên người (wearable device) cung cấp dữ liệu y tế tức thì nhằm cho biết thông tin chi tiết về sức khỏe hàng ngày của bệnh nhân.
## Chuẩn hóa trao đổi dữ liệu

# HL7
**HL7**,  một tổ chức quốc tế phi lợi nhuận được thành lập năm 1987, chuyên về chuẩn hóa giao tiếp trong ngành y tế, chăm sóc sức khỏe.

# FHIR


## Specification

FHIR sử dụng [[API]] để trao đổi dữ liệu

## Benefits

## Apply

# Hiện trạng tại Việt Nam
[[2023-04-04]]

Một số vấn đề về danh mục chuẩn theo Bộ Y tế Việt Nam, nhằm phục vụ cho công tác thanh toán viện phí Online của Bảo Hiểm Xã Hội.

- Danh mục dịch vụ: các danh mục dịch vụ hiện tại do Bộ Y tế Việt Nam ban hành (2016) không theo một chuẩn quốc tế nào. Nhiều tên dịch vụ bị xếp loại trùng lắp nhau. Một tên dịch vụ có nhiều mã số… Những điều này không tạo thành chuẩn dữ liệu cho y tế quốc gia. Mặt khác tên gọi trong chuẩn không dùng được hoặc bất tiện trong việc ghi chỉ định hàng ngày. Ví dụ: một tên gọi là “Chụp Cắt Lớp Vi Tính mạch máu não có cản quang” khá dài dòng thì nên được ghi ngắn gọn là “CT mạch máu não CE”.
- Danh mục thuốc: Danh mục thuốc nên được tuân theo chuẩn quốc tế. Cần phân biệt rõ tên thuốc (tên thương mại, tên biệt dược) với thành phần hoạt chất. Hàm lượng thuốc chính là hàm lượng của hoạt chất. Một thuốc có nhiều hoạt chất khác nhau thì tên hoạt chất phải đi kèm hàm lượng của nó. Tên thuốc thì có thể tùy gọi nhưng hoạt chất và hàm lượng cần phải được tuyệt đối chính xác. Cần có một bảng con đi kèm tên thuốc để ghi rõ hoạt chất và hàm lượng trong trường hợp thuốc có nhiều hoạt chất. Điều này rất quan trọng vì tổng hàm lượng của từng hoạt chất trong một đơn thuốc có thể ảnh hưởng đến kết quả điều trị và việc tách riêng từng hoạt chất sẽ giúp cảnh báo tương tác thuốc.

# References
1. [YKHOANET - Chuẩn dữ liệu](https://www.ykhoanet.com/c%E1%BA%A9m-nang-cntt-y-t%E1%BA%BF/chu%E1%BA%A9n-d%E1%BB%AF-li%E1%BB%87u)