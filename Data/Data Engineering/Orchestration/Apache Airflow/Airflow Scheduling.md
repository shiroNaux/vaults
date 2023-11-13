# Concept

Các khái niệm và tham số dưới đây được update đến phiên bản 2.7 (ngày 2023-11-13)

[[Airflow]] có các khái niệm sau khi nói đến việc schedule workflow:
- Data Interval
- Logical date
- Timetable
- Run After
- Backfill & catchup

Đó là các khái niệm liên quan đến schedule DAG, ngoài ra ta cũng cần hiểu 1 số options liên quan bao gồm:
- data_interval_start
- data_interval_end
- schedule (Các phiên bản trước là schedule_interval): 
- start_date: Ngày đầu tiên mà DAG sẽ được chạy -> Nếu nó là ngày trong tương lai thì DAG sẽ không run nếu chúng ta enable, cho đến đúng ngày __start_date__ thì DAG mới được chạy, còn nếu đó là 1 ngày trong quá khứ thì cần chú ý đến các option về catchup.
- end_date: Ngày cuối cùng DAG được chạy

# References
1. [DAG scheduling and timetables in Airflow | Astronomer Documentation](https://docs.astronomer.io/learn/scheduling-in-airflow)
2. 