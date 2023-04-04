---
---

# Abstract

FHIR (Fast Healthcare Interoperability Resource, pronounced “fire”) là chuẩn trao đổi data giữa các hệ thống CNTT áp dụng trong y tế. Giao thức (tiêu chuẩn) này được phát triển bởi HL7 (-> HL7 là 1 tổ chức). 


![[hl7-fhir-diagram.svg]]

## Vì sao cần chuẩn hóa dữ liệu

Để có được bộ dữ tiệu tốt cho công việc khai thác dữ liệu thì cần phải chuẩn hóa dữ liệu từ đầu vào. 

> Chú ý: Khai thác dữ liệu ở đây là tất cả các công việc liên quan đến sử dụng dữ liệu. Nó bao gồm cả việc chẩn đoán của bác sĩ, theo dõi, đánh giá dịch bệnh, ...

Hầu hết các nhà cung cấp dịch vụ chăm sóc sức khỏe(Bệnh viện, phòng khám, ...) sử dụng nhiều ứng dụng khác nhau cho mọi việc, từ thanh toán đến cập nhật thông tin bệnh nhân. Vấn đề là mỗi loại phần mềm này được phát triển bởi những đơn vị khác nhau, cho nên việc lưu trữ hay chia sẻ dữ liệu cũng khác nhau cho từng loại phần mềm, hệ thống.

Nhưng có lẽ, câu chuyện về kết nối dữ liệu chỉ thực sự trở nên cấp thiết hơn bao giờ hết khi y tế dự phòng được đẩy mạnh với sự ra đời của PHR - hồ sơ dữ liệu sức khoẻ cá nhân. Dữ liệu không còn nằm ở phía cơ sở y tế, dữ liệu được chia sẻ đến mỗi cá nhân để họ nắm được tình trạng sức khoẻ của mình, điều chỉnh sinh hoạt và phòng bệnh. Bản chất của hệ thống PHR - Hồ sơ sức khoẻ cá nhân là kết nối. PHR kết nối đến tất cả những nơi nào có dữ liệu y tế và sức khoẻ của cá nhân, tập hợp chúng lại để mỗi cá nhân có thể tự quản lý thông tin sức khoẻ của mình đầy đủ nhất. Và cũng nhờ những thông tin y tế đầy đủ đó, dưới sự hỗ trợ của công nghệ số hiện đại, máy tính có thể trợ giúp con người phòng bệnh tốt hơn, đưa ra những lời khuyên về sức khoẻ như những trợ lý sức khoẻ ảo của riêng họ.

Việc quy chuẩn danh mục giúp cho việc nhập liệu được nhanh chóng và dễ cho việc phân loại dữ liệu, giúp ích cho việc thống kê và giao tiếp giữa các cơ quan y tế. Tuy nhiên, việc chuẩn hóa các danh mục này cũng chỉ mang tính tương đối vì dữ liệu có thể biến đổi và không bao trùm được hết các khía cạnh thực tế.

Ví dụ:

- ICD 10 có thể ghi chẩn đoán “Viêm Xoang Hàm”, tuy nhiên thực tế cần ghi rõ hơn là viêm xoang hàm trái hay phải, mức độ nặng nhẹ thế nào, ở giai đoạn nào của diễn tiến bệnh.
- Danh mục nghề nghiệp không thể có các nghề như lơ xe, phụ hồ, xe ôm… Khi phát sinh các “nghề” mới, cần phải quy chuẩn các nghề đó vào 1 danh mục đã được định chuẩn.
- Danh mục thuốc cũng phải được cập nhật vì nhiều thuốc mới phát sinh.
- ...

With the emergence of electronic health record systems (EHRs), direct system-to-system secure digital transfer and portability of patient records become possible. Yet, even as we usher out 2022 we’ve still seen data formatting and availability as the wild, wild west. Each system implementer provides different ways to share data, or in some cases, prevent the sharing of data.

Since there had been no mandated universal standard for data exchange, each EHR had its own data specifications – often a hybrid approach using some industry standards and their own custom formats. This format and integration fragmentation put a lot of pressure on vendor integration teams and required custom API implementations for each platform they wished to integrate with. Because every implementation required a time and monetary investment, this meant that the APIs intended to provide data portability, ironically became hurdles for larger integration strategies.



Như những phân tích ở trên thì lợi ích của việc kết nối dữ liệu là vô cùng to lớn.

Bệnh viện cũng có nhu cầu, người dân cũng có nhu cầu và nhà nước cũng có nhu cầu vì lợi ích kinh tế mà nó mang lại.

Những kết quả xét nghiệm được chấp nhận liên viện, dữ liệu được chia sẻ thì sẽ không phải thực hiện thừa các xét nghiệm, tiết kiệm được rất nhiều tiền của nhà nước, bảo hiểm y tế và của chính những người dân, đồng thời tiết kiệm thời gian, đẩy nhanh quá trình khám chữa bệnh.

Dữ liệu được chuẩn hoá, kết nối và có thể tạo thành các data warehouse thì con đường làm Y tế thông minh sẽ ngắn lại rất nhiều. Công nghệ số tiên tiến sẽ biến dữ liệu thành tri thức phục vụ y tế dự phòng thông minh và quản lý y tế thông minh.

The main object of FHIR is to address the growing digitization needs in the healthcare industry and simplify the data exchange without compromising the integrity of the information. FHIR is determined to make electronic health records (EHRs) available, discoverable, and easily understandable to stakeholders as patients move within the healthcare ecosystem. This standard not only makes it easier for the patient to keep track of their own health but supports automated clinical decision support and the use of other artificial intelligence or machine-based processes.

There is a need for consistent, simple-to-implement, and thorough mechanisms that exchange data between disparate healthcare applications. FHIR resources have the capability to be incorporated in existing systems, which makes it easier and faster for healthcare application developers to implement. FHIR improves data exchange capability—creating a common set of APIs so healthcare platforms can connect and share data across systems in a format that each can understand.


## Chuẩn hóa dữ liệu

Việc quy chuẩn danh mục giúp cho việc nhập liệu được nhanh chóng và dễ cho việc phân loại dữ liệu, giúp ích cho việc thống kê và giao tiếp giữa các cơ quan y tế

Hiện tại đã có nhiều danh mục chuẩn hóa ở các cấp độ khác nhau: quốc tế, cấp quốc gia và cũng có danh mục chuẩn hóa tại từng bệnh viện

- Danh mục quốc tế: Tên quốc gia, mã số quốc gia, bộ mã chẩn đoán ICD, bộ mã thuốc men ATP, LOINC, SNOMED CT, …
- Danh mục cấp quốc gia: Danh mục thuốc, danh mục tên dịch vụ y tế, danh mục bệnh viện, danh mục tên chuyên khoa, danh mục tỉnh thành…
- Danh mục tự tạo: các khoa phòng, danh mục user, danh mục kho thuốc, danh mục thuốc… là các danh mục do bệnh viện tự tạo.


## Chuẩn hóa trao đổi dữ liệu

# FHIR (release 5)

## Specification

FHIR sử dụng API để trao đổi thông tin giữa các ứng dụng. Mà cụ thể hơn chính là REST API.

FHIR specifications are relatively straightforward, but it can be difficult to know where to begin to implement a solution. For that reason, FHIR is broken up into 13 modules to organize and guide set-up. There are roughly three groups of modules, including infrastructure, content, and modeling (or reasoning). Each of these modules is based on the resources described above.

HL7 has also outlined various steps (called “levels”) to show where each module is used in the set-up of your FHIR implementation:

FHIR bao gồm 5 cấp độ và 13 modules. Các cấp độ có thể được nhóm lại theo cấu trúc
1. Cơ sở hạ tầng (Level 1 & 2)
2. Nội dung (Level 3 & 4)
3. Suy luận (Level 5)

### Level 1:  Basic framework on which the specification is built

#### Foundation Module
The foundation module is the baseline infrastructure or framework on which the rest of the modules are developed. This framework defines the base documentations. The foundation module includes the following resources:

- Foundation framework
- Content management resource
- Data exchange resource

### Level 2: Supporting implementation and binding to external specifications

Có 5 Module trong level này

#### Implementation Support Module
This module supports the system with resources required for implementation, including available libraries, tools, helpline, and other similar resources.

#### Security and Privacy Module
This module provides guidance on how to protect FHIR servers through access control and authorization, user permission documentation, audit login, and provenance to keep records about events within the system. It controls access, identifies users and the context of access, audits the use of the system, and ensures privacy consent is enabled.

#### Conformance Module
The FHIR conformance module is all about the metadata for data types, resources, and API features. Conformance resources typically include an implementation guide that describes how FHIR is adapted to support certain use cases.

#### Terminology Module
This module provides terminologies that are used for representing and communicating coded and structured data in the FHIR core specification and profiles. These terminologies are used to create or reference a code system or value set, record data using pre-coordinated codes, record data using post-coordinated expressions, expand a value set, or validate a code.

#### Exchange Module
FHIR specifies which data is exchanged between healthcare apps. It also controls how the exchange is managed and implemented.

### Level 3: Linking to real-world concepts in the healthcare system

#### Administration Module
The administration module describes the base data to track patients and their care and links it to external sources. It is linked into the other modules for finance and billing, workflow, and clinical content. It performs a number of tasks, including:

Managing a master record of a patient
Managing administrative records
Enabling patient profiles
Enabling clinical reporting and connecting clinical records
Enabling clinical grouping and financial reporting

### Level 4: Record-keeping and Data Exchange for the healthcare process

Level này bao gồm 5 module

#### Clinical Module
This module focuses on the clinical information of patients. It is critical in terms of data breaches because it stores patient data. Users need to have security and privacy access in order to search and pull any information. This information includes:

1. Documenting the patient’s health condition
2. Retrieving patient’s condition
3. Documenting family history
4. Documenting and retrieving patient allergies

#### Diagnostics Module
This diagnostics module describes ordering and reporting of clinical diagnostics, which includes laboratory testing and imaging. Because diagnostic resources are patient data, users need to have a required security and private policy for searching and pulling any information related to:

- Recording a patient’s temperature
- Reporting a image study
- Reporting a laboratory test
- Medications Module
- Medication covers three main parts:

Ordering, dispensing, administering medications, and recording statements of medication use
Recording immunizations given (or not given), evaluation of given immunizations, and recommendations for individual patients at a point in time
Creating or querying medications as part of drug information or drug knowledge

#### Workflow Module
This module focuses on coordinating activities within the healthcare ecosystem. Some of the common use cases are:

Creating a lab order or medicine prescription
Communicating information about order status changes
Creating an appointment

#### Financial Module
The financial module supports billing and transactions that occur within the healthcare ecosystem. It covers patient account management, enrollment requests and responses, payment notice, and reconciliation.

### Level 5 – Clinical Reasoning

#### Clinical Reasoning
The clinical reasoning module provides the reasoning to deal with the healthcare process. The resources used enable the representation, distribution, and evaluation of clinical knowledge. It includes clinical decision-support rules, quality measures, public health indicators, order sets, and clinical protocols.

There are dependencies between modules, but they follow the steps listed above. Healthcare developers should only use the modules that apply to them.

#### Medication Definition Module

This module is concerned with resources and functionality in areas such as:

The regulation and manufacturing of drugs, medicinal products and other medically-related substances or co-packaged devices.
Detailed description of these same items, typically for regulators but also for any context where this level of detail is necessary, such as pharmacopoeias, or prescribing support.
Indications and contra-indications for medications.
The details of the chemical substances that may be ingredients for medications.
These resources are not typically used in direct patient care or day-to-day prescribing functionality (for which see the Medications Module, and further down on this page: Relationship to Prescribing), but play a supporting role. They can specify the "full" version of the product type, with all its information, including the licensed packages available, with their physical details and containers, through to individual tablet level and on to ingredients and component substances.


---
## LOINC

Bảng mã dữ liệu LOINC bao gồm Mã đại diện cho một tên dịch vụ y tế hoặc một bộ dịch vụ y tế, áp dụng cho khu vực cận lâm sàng (xét nghiệm và chấn đoán hình ảnh).

Tên một xét nghiệm có thể đọc khác nhau giữa các vùng miền, địa phương hay giữa các quốc gia. Ví dụ: đường huyết hay Glucose huyết thanh?

Tên một xét nghiệm có thể nghe giống nhau nhưng khác nhau về yêu cầu định tính hay định lượng, hoặc khác nhau về cách tiến hành xét nghiệm. Ví dụ: đường huyết lúc đói hay đường huyết ngẫu nhiên? Xét nghiệm HIV theo phương pháp Westtern blot hay Elisa?

Do đó, các tên xét nghiệm đó phải được phân biệt một cách rõ ràng và được gán cho một mã số xác định.

Bằng cách dùng chung mã số, việc gửi nhận thông tin cho nhau bằng điện tử sẽ trở nên dễ dàng.

Ứng dụng bộ mã LOINC nhiều nhất là nhúng vào thành phần bản tin HL7. Khi đó, thay vì gọi tên xét nghiệm thì người ta dùng mã đại diện để gọi tên xét nghiệm.

Tên dịch vụ y tế bao gồm tên dài (long terms), tên ngắn (short terms) và tên tương đương (synonym).

Một mã LOINC bao gồm 6 thành phần:

- component
- property
- time
- system
- scale
- method

Mỗi thành phần được quy ước thành các bộ mã riêng để thuận tiện trong việc sắp xếp. Nhìn vào dãy mã số, người ta không thể xác nhận được đó là xét nghiệm gì mà phải dùng một tiện ích phiên dịch là RELMA.

Cho đến nay thì LOINC chưa được ứng dụng rộng rãi và hữu dụng trong CNTT y tế ở cả thế giới và Việt Nam.

---
Resources (aka Atomic Data)
Resources are the fundamental building blocks of FHIR. All exchangeable content in the FHIR standard is contained in a resource. These lightweight JSON snippets can be created for every aspect of a patient encounter. 

Instead of sharing a patient’s fixed, bloated XML record with everyone who requests it, FHIR permits requests for targeted, relevant resources. 

Think of old standards as if someone handed you an entire file cabinet to read through. With FHIR, it’s as if they gave you an index card with the information you requested instead. It’s simply the preferred modern practice.

Common resource types include Observation for vital signs, or Encounter for a visit. Different resources can also combine to form new use cases or standardized document types.

All resources have metadata and a human-readable component. Each resource of the same type is formatted in the same way. 

Resources can refer to each other, and to a URL. Resources can stand on their own or be made to support event workflows. They can come with customizable permissions, many of which have been suggested by the FHIR community.

Taken together, resources begin to form a valuable store of healthcare information.

Interoperable, Bi-Directional RESTful APIs
The FHIR standard includes RESTful API protocols as well as data.

REST architecture is familiar to developers, as it’s the same architecture that underlies enterprise tools, the internet, and modern APIs. It means that familiar HTTP and OAuth protocols play a part in its use. Yes, health infrastructure has finally reached the cutting edge!

As a RESTful system, FHIR is far more straightforward than previous approaches to health data. It supports atomic data access, meaning that FHIR-based tools can allow access to specific parts of a health record. It’s a standard that encourages privacy and is generally easier to work with.

Fast and Simple Design
FHIR is focused on ease of developer use and adoption. FHIR empowers a more robust and flexible data model of the actual patient experiences and treatment plans than predecessors in the field.

FHIR was built with the Pareto principle, or 80/20 design, in mind. The idea is that 80% of common use cases will be covered by the FHIR standard. In turn, individual FHIR implementations will handle the other less common 20%.

This was partially in response to the HL7 v3 standard, which was released, and flopped, in 2005. It was too comprehensive to understand.

Thanks to its focus on human-readable data, FHIR resources can also be read by non-developers with minimal effort!

Support for Legacy Health Systems
FHIR can be expressed as XML, JSON or RDF. It’s possible for basic FHIR interfaces to be implemented in a day, for free, and with support from vendors from Apple to Epic. 

Your implementation of FHIR need not support all FHIR resources, but must support what your organization does.

The FHIR standard doesn’t bring a system into or out of HIPAA compliance, but it can be implemented in HIPAA-compliant networks.

FHIR Has Friends In High Places
Legally speaking, FHIR is an important part of health tech.

In the US, the federal government has thrown its weight behind FHIR. The 21st Century Cures Act directed two major agencies - ONC and CMS - to promote nationwide interoperability using FHIR APIs.

Consumer-directed apps and portals are increasingly reliant on FHIR data. FHIR compatibility is also mandatory for payors and providers alike. FHIR implementations make it considerably easier to comply with Cures Act anti-information blocking rules.

FHIR’s Huge Community
There’s an active, worldwide community around FHIR. We’ve watched this community develop in real time, after the first draft of an API-based health data exchange was initiated by Australian developer Grahame Grieve in 2012 under the name Resources for Healthcare.

FHIR’s development was then overseen by Health Level 7 (HL7), the standards development organization dedicated to digital health interoperability. It continues to evolve with the help of volunteers and HL7 working groups.

FHIR is popular with developers globally. The standard’s open source nature, robust documentation and culture of collaboration contributes to its appeal.

You can find FHIR penetration in countries with health systems at various stages of development. For example, FHIR sees significant use in Brazil, Australia, Europe, Canada and India.

FHIR vs. CCDA
C-CDA, or Consolidated Clinical Document Architecture, is the main alternative to FHIR for patient medical record exports.

C-CDA is an XML-exclusive markup standard, not a complete API. C-CDA exports a patient’s medical history in a way that is difficult to query, and is generally read-only. It defines how documents are encoded, and can be exported - but does not define how they should be transported.


## Lợi ích
---
### Đối với xã hội

##### 1. FHIR giúp chia sẻ dữ liệu linh hoạt hơn

Việc chia sẻ dữ liệu tạo ra giá trị trong lĩnh vực chăm sóc sức khỏe bằng cách:
- Trao quyền cho bệnh nhân
	Trước tiên, hãy xem xét việc trao cho bệnh nhân quyền được chủ động đối với việc chăm sóc sức khỏe. Chúng ta biết rằng khi chia sẻ dữ liệu với bệnh nhân, họ có thể kiểm soát nhiều hơn vấn đề sức khỏe của bản thân. Qua đó, bệnh nhân đưa ra quyết định chăm sóc y tế tốt hơn cho chính mình, từ chế độ ăn uống, tập luyện thể dục đến việc điều trị. Tăng cường sự hợp tác giữa người thanh toán và nhà cung cấp dịch vụ y tế

- FHIR đem lại những lợi ích gì cho hệ thống y tế?
	Ngành Y tế đang hướng tới mô hình hoàn trả dựa trên giá trị. Đây là mô hình chủ yếu dựa vào việc chia sẻ dữ liệu giữa người trả tiền và nhà cung cấp dịch vụ y tế. Giá trị là sự phối hợp chăm sóc, phòng ngừa tốt hơn và quản lý các điều kiện tốt hơn, nhưng không làm tăng chi phí. Giúp nhà nghiên cứu lâm sàng đưa ra phương pháp điều trị hiệu quả. Nhà nghiên cứu lâm sàng có thể cải thiện các phương pháp điều trị bệnh, khi được quyền tiếp cận tốt hơn đối với EHR và dữ liệu có nguồn gốc từ bệnh nhân. Điều làm cho FHIR khác biệt với các cơ chế chia sẻ dữ liệu y tế trước đây là: FHIR rất phù hợp để triển khai các RESTful web service (nghĩa là các web service được viết dựa trên kiến trúc REST). REST (Viết tắt của từ: REpresentational State Transfer) gọn nhẹ và có thể được triển khai với các công nghệ web service nguồn mở phổ biến. Những điều này sẽ giúp bạn thực hiện chúng trong chia sẻ dữ liệu dễ dàng và tiết kiệm chi phí hơn. FHIR cung cấp 145 API riêng lẻ để chia sẻ dữ liệu trên toàn bộ hệ thống y tế. Thông số FHIR được thành phần hóa, nên bạn chỉ có thể triển khai các API cần thiết cho trường hợp sử dụng cụ thể. Một khi API đã khai triển, rất dễ dàng để đạt được khả năng tương tác. Bạn có thể kết nối bất kỳ nhà cung cấp hoặc hệ thống API nào phù hợp với đặc tả API. Điều này sẽ mang lại sự linh hoạt cho kiến ​​trúc của bạn. Hơn nữa, FHIR đang trở thành nền tảng cho các tiêu chuẩn khác giúp gia tăng giá trị dữ liệu của bạn. Chẳng hạn như SMART trên FHIR, nó xác định cách tích hợp các ứng dụng y tế sử dụng dữ liệu FHIR vào hệ thống EHR.


##### 2. FHIR giúp quản lý dữ liệu dễ dàng hơn
Các dạng dữ liệu y tế
Dữ liệu thúc đẩy hệ thống y tế có nhiều nguồn, định dạng khác nhau, với khối lượng và tần suất khác nhau. Ví dụ là:

- EHR cho phép cải thiện tất cả các khía cạnh của việc chăm sóc bệnh nhân, mức độ an toàn, kịp lúc, tính hiệu quả và công bằng.
- Claims data cung cấp thông tin chi tiết bổ sung về phương pháp điều trị và các loại thuốc cho bệnh nhân.
- Hồ sơ quan trọng (Vital record) thông báo các mục tiêu và chính sách y tế công cộng.
- Dữ liệu về đơn thuốc, hình ảnh, công việc xét nghiệm và tất cả các thành phần khác của dịch vụ khám chữa bệnh.
- Nghiên cứu được thực hiện nhằm cải thiện hệ thống hệ thống cung ứng dịch vụ y tế.
- Khảo sát thu thập dữ liệu để thúc đẩy nghiên cứu chính sách khám, chữa bệnh công cộng.
- Thiết bị đeo trên người với mục đích cung cấp dữ liệu y tế tức thì nhằm cho biết thông tin chi tiết về sức khỏe hàng ngày của bệnh nhân.

FHIR cung cấp một format dữ liệu mục tiêu chung tiêu chuẩn, chuyển đổi thông tin thành dạng có thể sử dụng được. 
- Nhờ format tiêu chuẩn này, tính toàn vẹn, chính xác và nhất quán của dữ liệu được đảm bảo. 
- Đồng thời, nó làm rõ các ý nghĩa khó hiểu và giảm thiểu dữ liệu thừa. 
- Mặt khác, nó cũng giúp chúng ta dễ dàng áp dụng nhất quán các quy tắc kinh doanh cụ thể.

Chuẩn hóa dữ liệu như vậy giúp việc quản lý trở nên đơn giản hơn, với danh mục dữ liệu có thể theo dõi được nguồn gốc, người sở hữu, người được phép sử dụng, người đã sử dụng và cho mục đích gì.

Bất chấp những lợi ích to lớn trong quản lý dữ liệu, khi áp dụng FHIR, bạn vẫn sẽ cần hiểu cách nhập dữ liệu, chuyển đổi, quản lý dữ liệu và cách giải quyết các vấn đề như bảo mật dữ liệu.

##### 3. FHIR giúp khai thác dữ liệu hiệu quả hơn
AI/máy học có thể khiến việc chăm sóc sức khỏe tốt hơn ra sao?
Khoa học dữ liệu đề cập đến cách thức dữ liệu được khai thác tự động để có thông tin chi tiết. Đây đích thị là nơi mà lợi ích thực tiễn của FHIR đối với ngành y tế đáp ứng được lời hứa hẹn của trí tuệ nhân tạo và máy học.

Rất nhiều giá trị phong phú ẩn chứa trong dữ liệu có thể được tiết lộ bằng AI hay máy học:

Đào tạo mô hình AI/máy học để chẩn đoán tình trạng bệnh. “Mặt trận” này đang có những tiến bộ, và nhiều nghiên cứu ngoài kia cho chúng ta biết về những học viên được hướng dẫn hoặc hỗ trợ thành công bởi mô hình AI/máy học.
Đào tạo mô hình AI/máy học để đề xuất các quy trình khám, chữa bệnh hiệu quả nhất hoặc các loại thuốc có thể được dùng điều trị cho bệnh nhân.
Đào tạo mô hình AI/máy học để giảm bớt gánh nặng tài chính cho hệ thống y tế, bằng cách phát hiện những nơi nguồn lực y tế đang bị sử dụng sai mục đích, do gian lận hoặc thiếu hiệu quả.
Vậy điều này có liên quan đến HL7 FHIR như thế nào?
4 Lợi ích tuyệt vời của FHIR đối với hệ thống y tế
HL7 FHIR giúp khai thác dữ liệu hiệu quả hơn – Ảnh: mohamed_hassan – pixabay
Như chúng ta đã thấy, FHIR không chỉ cung cấp tiêu chuẩn cho các API và dữ liệu. Nó còn cho biết cách dữ liệu này kết hợp cùng nhau một cách nhất quán. Định dạng tiêu chuẩn này thật sự tuyệt vời cho việc học máy.

Nếu “mớm” dữ liệu xấu cho mô hình AI/máy học, bạn sẽ nhận kết quả không tốt. Còn nếu không cung cấp đủ dữ liệu, thông tin bạn nhận về cũng sẽ bị hạn chế.

Thế thì, điều tạo nên những mô hình AI/machine learning tốt là gì?

Đó chính là:

Dữ liệu chất lượng cao
Dữ liệu cần có độ bao phủ rộng và đại diện cho toàn bộ không gian dữ liệu mà bạn cần bao phủ.

Số lượng dữ liệu đủ
Machine learning không hiệu quả với megabyte dữ liệu như với gigabyte, terabyte hoặc hơn. Càng có nhiều dữ liệu thì các dự đoán của machine learning càng đáng tin cậy.

Dữ liệu được định dạng thích hợp
Dữ liệu cũng cần được định dạng tốt, dán nhãn chính xác và mang ý nghĩa nhất quán.

FHIR cho phép bạn tạo ra các tập dữ liệu đào tạo khá lớn, chất lượng cao từ nhiều nguồn dữ liệu. 



##### 4. FHIR giúp tích hợp dữ liệu tốt hơn

Các điểm tích hợp hiện có
Chuỗi giá trị có thể bao gồm các hệ thống kế thừa hoặc cloud native mới vẫn đang trong quá trình phát triển. Trong nhiều trường hợp, bạn cần tích hợp thời gian thực với các hệ thống. Và FHIR có thể làm cho sự tích hợp này hợp lý hơn, với chi phí giảm mà hiệu quả lại được cải thiện.

Format trao đổi dữ liệu bắt buộc
Có những trao đổi dữ liệu bị ảnh hưởng bởi các quy định bắt buộc một số loại format trao đổi dữ liệu. 

Công nghệ Sổ cái phân tán
Hãy xem xét lời hứa của blockchain (sổ cái phân tán), cũng như ý nghĩa của nó đối với sự tích hợp giữa các hệ thống hoặc tổ chức. 

Chúng ta chuyển từ mô hình tập trung (tất cả dữ liệu và chức năng nằm trong máy tính lớn) sang mô hình phi tập trung (dữ liệu và chức năng tồn tại trong các hệ thống phi tập trung).

Và bây giờ là tới Sổ cái phân tán (Distributed Ledger), nghĩa là mọi người có cùng dữ liệu, chức năng, dữ liệu luôn được cập nhật ở tất cả các địa điểm và cho tất cả các bên.

Sổ cái phân tán có những tác động gì đến hệ thống thông tin y tế?
Đầu tiên là nó đồng nghĩa với việc bạn có quyền truy cập dữ liệu tức thì tại cùng một điểm với những người khác trong blockchain (với quyền bảo mật). Như thế, dữ liệu của chúng ta có chất lượng tốt hơn và lúc nào cũng có sẵn. Với những thứ như minh bạch chi phí, đây thực sự là một điều rất ý nghĩa! 

Thứ hai, nó có nghĩa là chúng ta không cần phải chỉnh phiên bản khác nhau của cùng một dữ liệu. Qua đó, các hệ thống và chuỗi giá trị có thể được đơn giản hóa đáng kể, giúp ích cho những lĩnh vực như xử lý yêu cầu bồi thường. 

HL7 FHIR trợ giúp như thế nào với những tích hợp dữ liệu này? 
Các API tiêu chuẩn do FHIR cung cấp có thể góp phần đơn giản hóa mọi thứ. Tương tự tiêu chuẩn dữ liệu, các API FHIR cung cấp một mục tiêu tiêu chuẩn chung để sắp đặt các tích hợp. 

Cho dù bạn đang tích hợp với những hệ thống không phải FHIR cần hỗ trợ các format trao đổi dữ liệu bắt buộc, hay đang tích hợp với blockchain, các API này đều sẽ cung cấp những API tiêu chuẩn nhằm xây dựng các tích hợp đó. Tất cả các lợi ích của khả năng tương tác là ở đây. Các điểm tích hợp có thể được tái sử dụng trên nhiều hệ thống đã được tích hợp với một tiêu chuẩn API chung.

TPH mong rằng bài viết này đã giúp các bạn hiểu thêm FHIR là gì, cũng như những lợi ích tuyệt vời mà HL7 FHIR mang lại cho hệ thống chăm sóc sức khỏe.

---

### Đối với Development team

##### 1. Faster Go-to-Market
Do đã có sẵn các tiêu chuẩn nên thời gian phát triển được rút ngắn lại. Bởi vì nó được dựa trên các công nghệ phát triển phần mềm mới nhất, phù hợp với hầu hết các team hiện tại. Ngoài ra
FHIR offers customers out-of-the-box APIs, reusable tools, and excellent security models. One of the major benefits of FHIR is that it is fast and easy to implement. Multiple developers can work on a project, and when using the compatible interaction engine, can have a simple interface working in just one day. The go-to-market speed greatly increases with FHIR.

##### 2. Better Data Management
FHIR provides a standard common target data format that transforms data into usable formats. These standard common formats offer excellent data integrity, accuracy, clarity, and consistency.

##### 3. Better Data Sharing and Collaboration
FHIR provides easy, fast, and secure data sharing capability and collaboration among healthcare systems’ stakeholders. It offers 145 APIs created for sharing information between healthcare systems. Most importantly, the componentized specifications of FHIR enables implementation of only the APIs required for specific use cases. After implementing APIs, it is easy to get the full benefit of interoperability at scale.

##### 4. Patient Empowerment
Patients get easy and secure access to their complete data, both real-time and historical. A patient can take a complete picture of their data and make informed decisions about their health as a result. It also accelerates the clinical decision-making process by giving healthcare practitioners complete and accurate data about the patient.

##### 5. Better Data Integration
With FHIR, it is possible to enable fast and efficient integration with legacy and modern systems. FHIR APIs provide a common way to create integrations. When a provider is integrating with non-FHIR systems, it provides standard APIs that offer interoperability. These integration points can be reused at scale across multiple systems.

##### 6. Developer Efficiency and Experience Improvement
FHIR improves an application developer’s efficiency by giving them a superior user experience. As healthcare app developers, they have granular data access to individual items within resources, such as patient demographics or lab results observations.

##### 7. FHIR is Open Source
FHIR is open source and free of cost. With open APIs, FHIR enables continuous real-time data exchange and makes it easy for developers to access healthcare data from healthcare applications. Open source means that any healthcare provider can use it, which provides an affordable way for the entire medical system to join together.

##### 8. Data Analytics and FHIR
FHIR modules can be used to build analytical, business information, and artificial intelligence systems. FHIR supports modern databases and allows data to be analyzed, which means that artificial intelligence and machine learning systems can be used to provide accurate estimates and predictions about future volumes, workflows, and trends.

##### 9. Mobile App Support
Mobile app adoption in healthcare is massive because many providers and patients increasingly move toward using apps for healthcare. FHIR supports all the technologies that mobile devices also use, increasing the compatibility with modern technology.

---


## Chalenges

Có một số thách thức cần phải đối mặt để có thể áp dụng và triển khai FHIR trong thực tiễn

1. Different versions of FHIR can be implemented in different systems. Migration between FHIR versions is painful because of absent backward compatibility. In this case, the interoperability will fail. In order to resolve this issue, the most up-to-date version of FHIR must be implemented in all possible provider systems.
2. Inconsistent implementation of APIs within the system means not all software will work seamlessly within the FHIR framework. The solution is complete adoption and integration with FHIR.
3. Many healthcare providers don’t understand the resources required to develop compliant and consistent standards across the system. The solution for this problem is dedicated medical IT teams within the organization that can correctly make judgements on what is needed and where, when, and how to implement it. Changes in the infrastructure will be required, and ongoing training, support, and education is also needed.


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
3. [HL7 FHIR là gì và đem lại lợi ích như thế nào cho hệ thống y tế? - TPH Solutions (tphsoft.com.vn)](https://tphsoft.com.vn/hl7-fhir-la-gi/)
4. [Kết nối dữ liệu y tế, câu chuyện của cả chục năm nữa? | OmiCare.vn](https://omicare.vn/goc-ceo/ket-noi-du-lieu-y-te-cau-chuyen-cua-ca-chuc-nam-nua)
7. [What is HL7 FHIR? | TIBCO Software](https://www.tibco.com/reference-center/what-is-hl7-fhir)
8. [What Is FHIR? Here's What Makes HL7 FHIR Special (particlehealth.com)](https://www.particlehealth.com/blog/what-is-fhir)
9. [Interoperability 3.0 (particlehealth.com)](https://www.particlehealth.com/blog/interoperability-3-0)