# Data Obervability

- Data Obervability chỉ việc **giám sát(Observer)** sức khỏe toàn bộ hệ thống data của tổ chức.
- Là 1 bước quan trọng trong [[DataOps]]


- Có 5 yếu tố quyết định (5 pillars of data observability)
	- **Freshness**: How up-to-date data are.
	- **Distribution**(Phân bố):
		-  Khoảng giá trị chấp nhận được của data -> Chất lượng data, phát hiện anomaly. Ví dụ: Tuổi của người trong khoảng từ 0 -> 120, nếu ngoài phạm vi này cần xem xét lại.
		- Data được lấy từ đâu => sự tin cậy của nguồn dữ liệu
	- **Volume**: Completeness of data. Data có đầy đủ về mặt số lượng
	- **Lineage**: Tương tự [[Data Lineage]]. Là quy trình (không phải [[Data pipeline]]), đường đi của dữ liệu: Ai tạo ra data, downstream ingesters and upstream sources => Hiểu được dòng dữ liệu giúp hiểu rõ nguyên nhân xảy ra lỗi, các rủi ro, ...
	- **Schema**: Format của data. Thay đổi schema rất nguy hiểm => Cần observer

## Cách Implement

## Quy trình

## Quan hệ với DataOps

#### Các tools hỗ trợ (OSS)
	- Greate Expectations
	- Montecarlo data
	- Re data
	- Sentry.io

###### Reference:
	1. [What Is Data Observability? 5 Pillars You Need To Know (montecarlodata.com)](https://www.montecarlodata.com/what-is-data-observability/#Data-observability-vs-data-quality)
	2. [Data Observability: Everything you need to know | Databand](https://databand.ai/data-observability/)
	3. [What Is Data Observability, and Why Do You Need It? | by Abraham Enyo-one Musa | Towards Data Science](https://towardsdatascience.com/what-is-data-observability-and-why-do-you-need-it-87463f054bea)