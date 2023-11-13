# Concept

Các khái niệm và tham số dưới đây được update đến phiên bản 2.7 (ngày 2023-11-13)

[[Airflow]] có các khái niệm sau khi nói đến việc schedule workflow:
- Data Interval
- Logical date: Tên gọi ở các phiên bản cũ của nó là execution date
- Timetable
- Run After
- Backfill & catchup

Đó là các khái niệm liên quan đến schedule DAG, ngoài ra ta cũng cần hiểu 1 số options liên quan bao gồm:
- data_interval_start = logical date -> define thời điểm bắt đầu
- data_interval_end
- schedule (Các phiên bản trước là schedule_interval): 
- start_date: Ngày đầu tiên mà DAG sẽ được chạy -> Nếu nó là ngày trong tương lai thì DAG sẽ không run nếu chúng ta enable, cho đến đúng ngày __start_date__ thì DAG mới được chạy, còn nếu đó là 1 ngày trong quá khứ thì cần chú ý đến các option về catchup.
- end_date: Ngày cuối cùng DAG được chạy

# Trigger DAG

[[DAG]] sẽ được trigger vào thời điểm:

start_date/last_run + schedule_interval

=> tức là DAG sẽ được trigger vào **cuối** khoảng thời gian interval mà ta định nghĩa.

Ví dụ: 1 DAG được lên lịch chạy vào phút thứ 30 của mỗi giờ. Thì lúc 9h30 DAG sẽ chạy nhưng không phải là chạy cho kì 9h30 -> 10h30 mà là chạy cho kì 8h30 -> 9h30.
Để rõ hơn, nếu ta set thời điểm bắt đầu run DAG là là 1h ngày hôm nay và interval là 1 tiếng. Thì DAG sẽ được run lần đầu tiên không phải là lúc 1h mà là 
# References
1. [DAG scheduling and timetables in Airflow | Astronomer Documentation](https://docs.astronomer.io/learn/scheduling-in-airflow)
2. [Airflow DAG Scheduling in 5 mins - YouTube](https://www.youtube.com/watch?v=bwi_xkTmv4I)