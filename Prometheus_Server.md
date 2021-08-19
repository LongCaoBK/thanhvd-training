# Tổng quan Prometheus Server  
## Giới thiệu cơ bản về Prometheus  
### Monitoring là gì? Khác gì với Logging và Tracing  
Monitoring là quá trình thu thập, tổng hợp và phân tích các số liệu(metrics) để cải thiện hệ thống.  
Logging là hệ thống thu thập, tìm kiếm, phân tích,... log của ứng dụng  
Tracing là các hệ thống theo dõi đường đi của 1 request tới hệ thống. Nó đi qua những service nào, dừng ở mỗi service bao lâu, lỗi ở step nào,...  

### Prometheus là gì?
Prometheus là một bộ công cụ giám sát và cảnh báo hệ thống mã nguồn mở ban đầu được xây dựng bởi công ty SoundCloud.  
Prometheus bây giờ đã trở thành một dự án mã nguồn mở độc lập và được duy trì độc lập với bất kỳ công ty nào.  
Prometheus có khả năng thu thập thông số/số liệu (metric) từ các mục tiêu được cấu hình theo các khoảng thời gian nhất định, đánh giá các biểu thức quy tắc, hiển thị kết quả và có thể kích hoạt cảnh báo nếu một số điều kiện được thảo mãn yêu cầu.  
Prometheus là 100% mã nguồn mở. Có thể xem mã nguồn tại Git : https://github.com/prometheus/prometheus/  
Phần lớn các core tính năng của Prometheus được viết bằng ngôn ngữ Go. Một số còn lại thì được viết bằng Java, Python hoặc Ruby.  
Prometheus không dùng để lấy dữ liệu log, thay vì vậy thì Prometheus là dịch vụ giám sát, thu thập và xử lý dữ liệu dạng metric (thông số).  
Prometheus sử dụng cơ chế pull dữ liệu từ máy chủ remote là chính, chứ không sử dụng cơ chế đợi remote push dữ liệu lên ngoại trừ trường hợp sử dụng PushGateway.  
Prometheus sử dụng chương trình cảnh báo Alertmanager để xử lý và gửi cảnh báo đi.  

### Các khái niệm cơ bản của Prometheus    
**Data model**  
Prometheus về cơ bản lưu trữ tất cả dữ liệu dưới dạng time series. Time series là luồng các giá trị theo timestamp của các giá trị được đánh dấu cùng một label hoặc metrics.  
***Metric names and labels***  
Mỗi time series được xác định duy nhất bởi tên metric name và một bộ các cặp key-value được gọi là labels(nhãn).
Metric name chỉ định các thông số của của hệ thống. Vd: http_requests_total - tổng số request HTTP đã nhận). Nó có thể chứa các chữ cái và chữ số ASCII cũng như dấu gạch dưới và dấu hai chấm. Nó phải được match với [a-zA-Z_:][a-zA-Z0-9_:]*.  
Labels: bất kỳ sự kết hợp nhãn đã cho nào cho cùng một metric name xác định particular dimensional instantiation of that metric( Vd: tất cả HTTP requests đã sử dụng phương thức POST tới /api/tracks xử lý). Ngôn ngữ truy vấn cho phép lọc và tổng hợp dựa trên các labels này. Khi thay đổi giá tri label, bao gồm cả thêm hoặc xóa label sẽ tạo ta một time series mới.  
Ví dụ: 
``` 
api_http_requests_total{method="POST", handler="/messages"}  
```  
Một time series có metric name là api_http_requests_total và lable method="POST", handler="/messages   

**Metric types**  
***Có 4 kiểu metric được sử dụng trong prometheus:***  
- **Counter** là một bộ đếm tích lũy, được đặt về 0 khi restart. Ví dụ, có thể dùng counter để đếm số request được phục vụ, số lỗi, số task hoàn thành,... Không sử dụng cho gía trị có thể giảm như số tiến trình đang chạy. Trong trường hợp này, ta có thể sử dụng gauge.  
- **Gauge** đại diện cho số liệu duy nhất, nó có thể lên hoặc xuống. Nó thường được sử dụng cho các giá trị đo.  
- **Histogram** lấy mẫu quan sát (thường là những thứ như là thời lượng yêu cầu, kích thước phản hồi). Nó cũng cung cấp tổng của các giá trị đó.  
- **Summary** tương tự histogram, nó cung cấp tổng số các quan sát và tổng các giá trị đó, nó tính toán số lượng có thể cấu hình qua sliding time window.  

**Jobs và Instances**  
Trong prometheus quy định một endpoint mà chúng ta có thế thể scrape được gọi là một **instance**.  
Tập hợp của các instances có cùng một mục đích được gọi là một **job**.  
Ví dụ:  
job: api-server  
instance 1: 1.2.3.4:5670  
instance 2: 1.2.3.4:5671  
instance 3: 5.6.7.8:5670  
instance 4: 5.6.7.8:5671  

### Kiến trúc của Prometheus stack và các thành phần cơ bản.  

#### Các thành phần trong promtheus  

Hệ sinh thái của giải pháp bao gồm nhiều thành phần trong đó có một vài thành phần là tùy chọn ( tức là có thể có hoặc không )  

- **Prometheus server** để  scrapes và lưu trữ dữ liệu 
- **Client libraries** hỗ trợ các ngôn ngữ lập trình khác tương tác với prom 
- **Push gateway** là một gateway trung gian hỗ trợ short-lived jobs 
- Các **exporter** để giám sát các services như HAProxy, StatsD, Graphite, ... 
- Một **alertmanager** phụ vụ cho việc cảnh báo 

## 4. Kiến trúc của Prometheus  

Sơ đồ này minh họa kiến ​​trúc của Prometheus và một số thành phần trong hệ sinh thái của nó:

<div style="text-align:center"><img src="https://prometheus.io/assets/architecture.png"></div>

Cách làm việc của promtheus được mô tả như sau:  

- Metrics sẽ thu thập thông qua exporter 
- Prometheus sẽ tự discovery ra target của các jobs hoặc trong file cấu hình thủ công để pull metrics về 
- Sau đó prom server sẽ xử lý và lưu trữ xuống database của mình
- Cùng lúc đó AlertManager sẽ tiến hành kiểm tra điều kiện nếu match với các rules đã được người quản trị định nghĩa thì sẽ gửi cảnh báo thông qua các kênh mail, slack, ... 
- Prom UI là nơi hiển thị graph của các metrics đẩy về hệ thống. Ngoài ra garafa cũng hỗ trợ data source prom. 

Prom được viết bằng ngôn ngữ Go và được thiết kế theo kiến trúc microservices. Các thầnh phần của nó giao tiếp với nhau thông qua api 

#### Trường hợp nào sử dụng Prometheus ?

Prometheus hoạt động tốt để ghi các time series data dạng số. Nó phù hợp với việc giám sát các máy chủ cũng như các kiến trúc highly dynamic service-oriented.

Đối với microservice, nó hỗ trợ thu thập và truy vấn dữ liệu đa chiều.

#### Trường hợp nào không nên sử dụng?

Prometheus là đáng tin cậy. Có thể xem thống kê nào có sẵn về hệ thống của chúng ta, ngay cả trong điều kiện lỗi. Nếu chúng ta cần độ chính xác 100%, ví dụ cho mục đích thanh toán, Prometheus không phải là một lựa chọn tốt vì nó không chi tiết và đầy đủ.

### Cấu hình Prometheus  

#### Cấu hình Prometheus server  

#### Recording rules

Các recording rules cho phép ta định tuyến trước các biểu thức thường xuyên cần thiết hoặc expensive về mặt tính toán và lưu kết quả của chúng dưới dạng một chuỗi time series mới. Truy vấn kết quả được tính trước sau đó thường sẽ nhanh hơn nhiều so với thực hiện biểu thức ban đầu mỗi khi cần thiết. Điều này đặc biệt hữu ích cho bảng điều khiển, cần truy vấn cùng một biểu thức lặp đi lặp lại mỗi khi chúng làm mới. 

Các recording rules và alerting rules tồn tại trong 1 nhóm rule. Các rule trong một nhóm được chạy tuần tự trong một khoảng thời gian đều đặn, với cùng một evaluation time. Tên của các recording rules phải là metric name hợp lệ. Tên của các alerting rules phải là giá trị label(nhãn) hợp lệ. 

Cú pháp của tệp quy tắc là: 
```
groups:
  [ - <rule_group> ] 
```
Một tệp quy tắc ví dụ đơn giản : 
```
groups:
  - name: example
    rules:
    - record: job:http_inprogress_requests:sum
      expr: sum by (job) (http_inprogress_requests)
<rule_group>

name: <string>

[ interval: <duration> | default = global.evaluation_interval ]

rules:
  [ - <rule> ... ] 
```

Cú pháp cho recording rules là: 
```
record: <string>

expr: <string> 

labels:
  [ <labelname>: <labelvalue> ] 
```

#### Alerting rules

Cho phép ta xác định các điều kiện cảnh báo dựa trên biểu thức ngôn ngữ của Prometheus và gửi thông báo về  firing alerts đến dịch vụ bên ngoài. 

Xác định các alerting rules:  
Các alerting rules được cấu hình trong Prometheus giống như các recording rules. 

Một tệp quy tắc ví dụ với cảnh báo sẽ là:
```
groups:
- name: example
  rules:
  - alert: HighRequestLatency
    expr: job:request_latency_seconds:mean5m{job="myjob"} > 0.5
    for: 10m
    labels:
      severity: page
    annotations:
      summary: High request latency
```  

### Tìm hiểu về Alertmanager  

Alerting và Prometheus tách thành 2 phần. Các Alerting rule được Prometheus gửi đến Alertmanager, Alertmanager quản lý việc cảnh báo, bao gồm  silencing, inhibition, aggregation và gửi cảnh báo đi qua các kênh như email, HipChat, PagerDuty. 

3 bước để thiết lập cảnh báo:
- Cài đặt và cấu hình Alertmanager 
- Cấu hình Prometheus liên kết với Alertmanager 
- Tạo các rule alert trong Prometheus

***Alertmanager**  
Alertmanager xử lý cảnh báo được gửi bởi ứng dụng như là Prometheus server. Nó có các cơ chế Grouping, Inhibition, Silience

- Grouping :

Phân loại cảnh báo có những tính chất tương tự. Điều này thực sự hữu ích trong một hệ thống lớn với nhiều thông báo được gửi đông thời. 

Ví dụ: Một hệ thống với nhiều server mất kết nối đến cơ sở dữ liệu, thay vì rất nhiều cảnh báo được gửi về Alertmanager thì Grouping giúp cho việc giảm số lượng cảnh báo trùng lặp, thay vào đó là một cảnh báo để chúng ta có thể biết được chuyện gì đang xảy ra với hệ thống của bạn.

- Inhibition :

Inhibition  là một khái niệm về việc chặn thông báo cho một số cảnh báo nhất định nếu các cảnh báo khác đã được kích hoạt. 

Ví dụ: Một cảnh báo đang kích hoạt, thông báo là cluster không thể truy cập (not reachable). Alertmanager có thể được cấu hình là tắt các cảnh báo khác liên quan đến cluster này nếu cảnh báo đó đang kích hoạt. Điều này lọc bớt những cảnh báo không liên quan đến vấn đề hiện tại. 

- Silience : 

Silience là tắt cảnh báo trong một thời gian nhất định. Nó được cấu hình dựa trên các match, nếu nó match với các điều kiện thì sẽ không có cảnh báo nào được gửi khi đó. 

- High avability:

Alertmanager hỗ trợ cấu hình để tạo một cluster với độ khả dụng cao. 

