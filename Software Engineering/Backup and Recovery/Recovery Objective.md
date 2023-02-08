---
---

# Introduction



### RTO
RTO stand for recorvery time objectives

RTO is a measure of the maximum amount of time a system, application, or function can be unavailable after a disaster or disruption before the impact on business operations becomes unacceptable. In other words, it represents the target time for restoring normal system operation after an interruption.

### RPO

RPO mean Recovery Point Objective.

RPO, on the other hand with RTO, is a measure of the maximum amount of data loss that an organization is willing to accept as a result of a disaster or disruption. It represents the point in time to which data must be recovered from backups in order to minimize the impact of an interruption. -> Tức là RPO chỉ ra khoảng loss data chấp nhận được

=> In summary, RTO is focused on the time it takes to restore systems, while RPO is focused on the data that may be lost. Both RTO and RPO are important metrics in disaster recovery planning, as they help organizations ensure that they have a plan in place to quickly resume operations and minimize data loss in the event of a disaster.

# Define Recovery Objectives

Let do this through an example:

At 9 am, an application was impaired on the bank's main server, halting services locally and online for 5 minutes. The bank's RPO counted for 15 minutes of data loss, and their RTO counted for 10 minutes of recovery time to restore the systems and applications. Therefore, the bank was within the parameters of both objectives.

At 3 am, the same bank faced a shutdown of systems for one hour. As the RPO only counted for 15 minutes of data loss, and the Recovery Time Objective counted for only 10 minutes of downtime, it meant 50 minutes of the shutdown time was not accounted for. However, due to the time that the shutdown occurred, the loss of data was not exponential as the recovery process happened during a low-traffic period for the bank.