---
---
# Introduction

Là tập hợp các nguyên tắc để đánh giá sự hoạt dộng ổn định của hệ thống 

## 1. SLA
- Viết tắt của service-level agreement, tức là cam kết khả năng hoạt động của hệ thống đối với người dùng cuối
### Ví dụ:
- Google’s SLA có cam kết với người dùng là hệ thống của họ sẽ luôn đảm bảo uptime > 99.99%
## 2. SLO

Viết tăt của service-level object. Nếu SLA là 1 contract giữa đơn vị cung cấp và người dùng cuối, thì SLO chính là các tiêu chí cụ thể trong SLA đó.

### Ví dụ:
Trong ví dụ của SLA ở trên, uptime 99.99% chính là 1 SLO, ngoài ra còn có rất nhiều SLO khác nữa như:
- Response rate under 2 second < 1%
- resolve reported issues with Product X within 24 hours.%

## 3. SLI

Viết tắt của service-level indicator. Đây là các chỉ số đo được để đánh giá SLO, xem chúng có được đảm bào như trong SLA hay ko?

### Ví dụ:
- Nếu SLA có cam kết uptime 99%, thì SLI sẽ là thời gian uptime của hệ thống được ghi lại.

# Conclusion

- SLA: -> là 1 contract, cam kết với người dùng cuối
- SLo: -> Là mục tiêu cụ thể được chỉ rõ ra trong contract(SLA)
- SLI: -> là các chỉ số đo đạc được của từng SLO


# Refenrences
1. [SRE at Google: SLOs, SLIs, SLAs, oh my | Google Cloud Blog](https://cloud.google.com/blog/products/devops-sre/availability-part-deux-cre-life-lessons)
2. [SLA vs. SLO vs. SLI - Differences | Atlassian](https://www.atlassian.com/incident-management/kpis/sla-vs-slo-vs-sli)