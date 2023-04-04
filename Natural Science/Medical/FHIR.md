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

Như những phân tích ở trên thì lợi ích của việc kết nối dữ liệu là vô cùng to lớn. 

Bệnh viện cũng có nhu cầu, người dân cũng có nhu cầu và nhà nước cũng có nhu cầu vì lợi ích kinh tế mà nó mang lại. 

Những kết quả xét nghiệm được chấp nhận liên viện, dữ liệu được chia sẻ thì sẽ không phải thực hiện thừa các xét nghiệm, tiết kiệm được rất nhiều tiền của nhà nước, bảo hiểm y tế và của chính những người dân, đồng thời tiết kiệm thời gian, đẩy nhanh quá trình khám chữa bệnh. 

Dữ liệu được chuẩn hoá, kết nối và có thể tạo thành các data warehouse thì con đường làm Y tế thông minh sẽ ngắn lại rất nhiều. Công nghệ số tiên tiến sẽ biến dữ liệu thành tri thức phục vụ y tế dự phòng thông minh và quản lý y tế thông minh.


The main object of FHIR is to address the growing digitization needs in the healthcare industry and simplify the data exchange without compromising the integrity of the information. FHIR is determined to make electronic health records (EHRs) available, discoverable, and easily understandable to stakeholders as patients move within the healthcare ecosystem. This standard not only makes it easier for the patient to keep track of their own health but supports automated clinical decision support and the use of other artificial intelligence or machine-based processes.

There is a need for consistent, simple-to-implement, and thorough mechanisms that exchange data between disparate healthcare applications. FHIR resources have the capability to be incorporated in existing systems, which makes it easier and faster for healthcare application developers to implement. FHIR improves data exchange capability—creating a common set of APIs so healthcare platforms can connect and share data across systems in a format that each can understand.


## Chuẩn hóa dữ liệu

### Các dạng dữ liệu y tế

Trong các phần mềm hay hệ thống IT phục vụ trong ngành y tế có nhiều tiêu chuẩn dữ liệu khác nhau đã được đưa ra.
Với việc ứng dụng CNTT vào trong y tế

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

FHIR sử dụng [[API]], nói cách khác: dữ liệu sẽ được lưu theo đúng format và trao đổi giữa các hệ thống với nhau thông qua API


FHIR specifications are relatively straightforward, but it can be difficult to know where to begin to implement a solution. For that reason, FHIR is broken up into 13 modules to organize and guide set-up. There are roughly three groups of modules, including infrastructure, content, and modeling (or reasoning). Each of these modules is based on the resources described above.

HL7 has also outlined various steps (called “levels”) to show where each module is used in the set-up of your FHIR implementation:

Level 1, Foundation Module
The foundation module is the baseline infrastructure or framework on which the rest of the modules are developed. This framework defines the base documentations. The foundation module includes the following resources:

Foundation framework
Content management resource
Data exchange resource
Level 2, Implementation and Set-Up
There are several modules included in this step. They are designed to help you build the system and bind them to the proper external resources.

Implementation Support Module
This module supports the system with resources required for implementation, including available libraries, tools, helpline, and other similar resources.

Security and Privacy Module
This module provides guidance on how to protect FHIR servers through access control and authorization, user permission documentation, audit login, and provenance to keep records about events within the system. It controls access, identifies users and the context of access, audits the use of the system, and ensures privacy consent is enabled.

Conformance Module
The FHIR conformance module is all about the metadata for data types, resources, and API features. Conformance resources typically include an implementation guide that describes how FHIR is adapted to support certain use cases.

Terminology Module
This module provides terminologies that are used for representing and communicating coded and structured data in the FHIR core specification and profiles. These terminologies are used to create or reference a code system or value set, record data using pre-coordinated codes, record data using post-coordinated expressions, expand a value set, or validate a code.

Exchange Module
FHIR specifies which data is exchanged between healthcare apps. It also controls how the exchange is managed and implemented.

Level 3, Administration
The administration module describes the base data to track patients and their care and links it to external sources. It is linked into the other modules for finance and billing, workflow, and clinical content. It performs a number of tasks, including:

Managing a master record of a patient
Managing administrative records
Enabling patient profiles
Enabling clinical reporting and connecting clinical records
Enabling clinical grouping and financial reporting
Level 4, Record-Keeping and Data Exchange
Again, there are several modules used in this level.

Clinical Module
This module focuses on the clinical information of patients. It is critical in terms of data breaches because it stores patient data. Users need to have security and privacy access in order to search and pull any information. This information includes:

Documenting the patient’s health condition
Retrieving patient’s condition
Documenting family history
Documenting and retrieving patient allergies
Diagnostics Module
This diagnostics module describes ordering and reporting of clinical diagnostics, which includes laboratory testing and imaging. Because diagnostic resources are patient data, users need to have a required security and private policy for searching and pulling any information related to:

Recording a patient’s temperature
Reporting a image study
Reporting a laboratory test
Medications Module
Medication covers three main parts:

Ordering, dispensing, administering medications, and recording statements of medication use
Recording immunizations given (or not given), evaluation of given immunizations, and recommendations for individual patients at a point in time
Creating or querying medications as part of drug information or drug knowledge
Workflow Module
This module focuses on coordinating activities within the healthcare ecosystem. Some of the common use cases are:

Creating a lab order or medicine prescription
Communicating information about order status changes
Creating an appointment
Financial Module
The financial module supports billing and transactions that occur within the healthcare ecosystem. It covers patient account management, enrollment requests and responses, payment notice, and reconciliation.

Level 5 – Clinical Reasoning
The clinical reasoning module provides the reasoning to deal with the healthcare process. The resources used enable the representation, distribution, and evaluation of clinical knowledge. It includes clinical decision-support rules, quality measures, public health indicators, order sets, and clinical protocols.

There are dependencies between modules, but they follow the steps listed above. Healthcare developers should only use the modules that apply to them.

## Benefits


Faster Go-to-Market
FHIR offers customers out-of-the-box APIs, reusable tools, and excellent security models. One of the major benefits of FHIR is that it is fast and easy to implement. Multiple developers can work on a project, and when using the compatible interaction engine, can have a simple interface working in just one day. The go-to-market speed greatly increases with FHIR.

Better Data Management
FHIR provides a standard common target data format that transforms data into usable formats. These standard common formats offer excellent data integrity, accuracy, clarity, and consistency.

Better Data Sharing and Collaboration
FHIR provides easy, fast, and secure data sharing capability and collaboration among healthcare systems’ stakeholders. It offers 145 APIs created for sharing information between healthcare systems. Most importantly, the componentized specifications of FHIR enables implementation of only the APIs required for specific use cases. After implementing APIs, it is easy to get the full benefit of interoperability at scale.

Patient Empowerment
Patients get easy and secure access to their complete data, both real-time and historical. A patient can take a complete picture of their data and make informed decisions about their health as a result. It also accelerates the clinical decision-making process by giving healthcare practitioners complete and accurate data about the patient.

Better Data Integration
With FHIR, it is possible to enable fast and efficient integration with legacy and modern systems. FHIR APIs provide a common way to create integrations. When a provider is integrating with non-FHIR systems, it provides standard APIs that offer interoperability. These integration points can be reused at scale across multiple systems.

Developer Efficiency and Experience Improvement
FHIR improves an application developer’s efficiency by giving them a superior user experience. As healthcare app developers, they have granular data access to individual items within resources, such as patient demographics or lab results observations.

FHIR is Open Source
FHIR is open source and free of cost. With open APIs, FHIR enables continuous real-time data exchange and makes it easy for developers to access healthcare data from healthcare applications. Open source means that any healthcare provider can use it, which provides an affordable way for the entire medical system to join together.

Data Analytics and FHIR
FHIR modules can be used to build analytical, business information, and artificial intelligence systems. FHIR supports modern databases and allows data to be analyzed, which means that artificial intelligence and machine learning systems can be used to provide accurate estimates and predictions about future volumes, workflows, and trends.

Mobile App Support
Mobile app adoption in healthcare is massive because many providers and patients increasingly move toward using apps for healthcare. FHIR supports all the technologies that mobile devices also use, increasing the compatibility with modern technology.

## Chalenges


FHIR is not free of challenges. There are three main issues that FHIR faces.

1. Different versions of FHIR can be implemented in different systems. Migration between FHIR versions is painful because of absent backward compatibility. In this case, the interoperability will fail. In order to resolve this issue, the most up-to-date version of FHIR must be implemented in all possible provider systems.
2. Inconsistent implementation of APIs within the system means not all software will work seamlessly within the FHIR framework. The solution is complete adoption and integration with FHIR.
3. Many healthcare providers don’t understand the resources required to develop compliant and consistent standards across the system. The solution for this problem is dedicated medical IT teams within the organization that can correctly make judgements on what is needed and where, when, and how to implement it. Changes in the infrastructure will be required, and ongoing training, support, and education is also needed.

## Apply

# Hiện trạng tại Việt Nam
[[2023-04-04]]

Một số vấn đề về danh mục chuẩn theo Bộ Y tế Việt Nam, nhằm phục vụ cho công tác thanh toán viện phí Online của Bảo Hiểm Xã Hội.

- Danh mục dịch vụ: các danh mục dịch vụ hiện tại do Bộ Y tế Việt Nam ban hành (2016) không theo một chuẩn quốc tế nào. Nhiều tên dịch vụ bị xếp loại trùng lắp nhau. Một tên dịch vụ có nhiều mã số… Những điều này không tạo thành chuẩn dữ liệu cho y tế quốc gia. Mặt khác tên gọi trong chuẩn không dùng được hoặc bất tiện trong việc ghi chỉ định hàng ngày. Ví dụ: một tên gọi là “Chụp Cắt Lớp Vi Tính mạch máu não có cản quang” khá dài dòng thì nên được ghi ngắn gọn là “CT mạch máu não CE”.
- Danh mục thuốc: Danh mục thuốc nên được tuân theo chuẩn quốc tế. Cần phân biệt rõ tên thuốc (tên thương mại, tên biệt dược) với thành phần hoạt chất. Hàm lượng thuốc chính là hàm lượng của hoạt chất. Một thuốc có nhiều hoạt chất khác nhau thì tên hoạt chất phải đi kèm hàm lượng của nó. Tên thuốc thì có thể tùy gọi nhưng hoạt chất và hàm lượng cần phải được tuyệt đối chính xác. Cần có một bảng con đi kèm tên thuốc để ghi rõ hoạt chất và hàm lượng trong trường hợp thuốc có nhiều hoạt chất. Điều này rất quan trọng vì tổng hàm lượng của từng hoạt chất trong một đơn thuốc có thể ảnh hưởng đến kết quả điều trị và việc tách riêng từng hoạt chất sẽ giúp cảnh báo tương tác thuốc.

Dữ liệu y tế nằm ở các hệ thống khác nhau. Muốn kết nối, chia sẻ dữ liệu thì cần được chuẩn hoá. Chuẩn lưu trữ và chia sẻ dữ liệu quốc tế HL7 ra đời với mục đích như vậy và đã có khoảng 55 quốc gia tham gia vào việc ứng dụng chuẩn này. 
Trải qua  hơn 30 năm phát triển, HL7 đã thay đổi nhiều phiên bản từ dạng đơn giản truyền message, đến dạng document XML và kiểu REST API phù hợp với công nghệ phát triển web. 
Ở Việt Nam, theo thông tư 46 của Bộ Y Tế, phần mềm hồ sơ bệnh án điện tử phải lưu trữ dữ liệu y tế theo chuẩn HL7 CDA hoặc FHIR là 2 dạng chuẩn mới của HL7. (Như giới thiệu ở trên, Nhật Bản đang sử dụng dạng message HL7 v2.5)
Tuy nhiên, vấn đề nằm ở chỗ dữ liệu có đang thực sự được lưu dưới dạng HL7 CDA, FHIR hay không thì không ai kiểm chứng được. 
Tôi không dám tin là các phần mềm bệnh án điện tử của Việt Nam hiện nay đã làm đúng được yêu cầu này. Bản thân FHIR là một chuẩn mới, guideline của nó lên tới hơn 100 nghìn tài liệu. Mới đầu năm 2021, Nhật Bản tính toán nếu phổ biến chuẩn FHIR thì trước hết Nhật Bản cần 2 triệu USD chỉ để dịch guideline hướng dẫn lưu trữ liệu y tế đúng. 
Với một quốc gia nhỏ thì việc triển khai 1 hệ thống đồng loạt lên tất cả các cơ sở y tế là một có thể làm được (giống như Estonia đã làm thành công). Nhưng với quốc gia dân số đông như Việt Nam, với hệ thống y tế nhiều tầng, nhiều lớp thì việc triển khai đồng loạt là khó có thể thực hiện được. Việt Nam đang đi theo đúng con đường của Nhật Bản trước đây, sử dụng nhiều hệ thống khác nhau. FPT, VNPT, Viettel, Vietba...các công ty lớn, nhỏ đều đã tham gia và triển khai ở các bệnh viện khác nhau. Do đó, con đường chuẩn hoá là con đường tất yếu. 
Chuẩn hoá thì cần có sự tham gia của quản lý nhà nước, cụ thể là bộ Y Tế. Hi vọng là bộ sẽ sớm có định hướng và chỉ đạo quyết liệt, mở con đường cao tốc đi đến chuyển đổi số Y Tế thành công. Nếu chậm trễ, chúng ta sẽ lại mất hàng chục năm nữa giống như trường hợp của Nhật Bản hiện nay. 

# References
1. [YKHOANET - Chuẩn dữ liệu](https://www.ykhoanet.com/c%E1%BA%A9m-nang-cntt-y-t%E1%BA%BF/chu%E1%BA%A9n-d%E1%BB%AF-li%E1%BB%87u)
2. [Tiêu chuẩn HL7 phiên bản 2.5 có những điểm nổi bật nào? - TPH Solutions (tphsoft.com.vn)](https://tphsoft.com.vn/tieu-chuan-hl7-phien-ban-2-5/)
3. [Kết nối dữ liệu y tế, câu chuyện của cả chục năm nữa? | OmiCare.vn](https://omicare.vn/goc-ceo/ket-noi-du-lieu-y-te-cau-chuyen-cua-ca-chuc-nam-nua)
4. [What Is FHIR? Here's What Makes HL7 FHIR Special (particlehealth.com)](https://www.particlehealth.com/blog/what-is-fhir)
5. [Tech Talk: What is an API? (particlehealth.com)](https://www.particlehealth.com/blog/tech-talk-what-is-an-api)
6. [What is HL7 FHIR? | TIBCO Software](https://www.tibco.com/reference-center/what-is-hl7-fhir)