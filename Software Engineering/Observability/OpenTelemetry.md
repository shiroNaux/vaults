---
---

# Introduction

OpenTelemetry là 1 framework có chức năng collect và route các data sử dụng trong [[Observability]] (logs, metrics, traces). Là 1 general framwork, nên OpenTelemetry (OTel) hỗ trợ đa dạng các ngôn ngữ, có thể kể đến [[Java]], [[Python]], [[.NET]], [[Go lang]], ... -> OTel không sử dụng DSL, và hầu hết các ứng dụng đều có thể tích hợp được OTel vào để phục vụ [[Observability|giám sát]]. Ngoài ra, điều làm OTel trở nên phổ biến đó là nó cung cấp 1 standard specification gọi là __OpenTelemetry Protocol__ (OTLP). Tức là nó cung cấp 1 tiêu chuẩn chung cho các ứng dụng có thể trích data suất cho các nhiệm vụ Observability.

>OTel không phải là dạng backend giống như [[prometheus]] hay Jaeger. Nó đơn thuần là 1 framwork hỗ trợ export data và gửi chúng đến destination để xử lý.

OTel sử dụng plugable architecture cho nên nó có thể hỗ trợ rất nhiều công nghệ, [[protocol]] hay output format khác nhau -> Độ đa dạng cao

# References

1. [What is OpenTelemetry? How it Works & Use Cases | Datadog (datadoghq.com)](https://www.datadoghq.com/knowledge-center/opentelemetry/)
2. [Documentation | OpenTelemetry](https://opentelemetry.io/docs/)
3. [Guide to OpenTelemetry (logz.io)](https://logz.io/learn/opentelemetry-guide/#auto)