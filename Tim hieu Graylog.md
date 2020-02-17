# TỔNG QUAN VỀ GRAYLOG

## 1. Giới thiệu chung

- Phần mềm mã nguồn mở quản lý log tập trung.
- Ra đời 2010 bởi Lennart Koopman với tên Graylog2.
- Tháng 2/2014, phát hành Graylog2 V0.20.0 Final.
- Tháng 1/2015, phát hành V1.x Beta, đổi tên thành Graylog, Graylog Inc được thành lập.
- Từ V1.0, đã trải qua 5 phiên bản, version mới nhất là Graylog 2.0 Alpha 5, stable version là 1.3
- Latest version: 3.1
## 2. Đặc điểm nổi bật
- Việc triển khai và cài đặt dễ dàng.
- Graylog có thể nhận log từ rất nhiều khác nhau : log của các server Linux, Window, thiết bị mạng như router, switch, firewall, các thiết bị lưu trữ như CEPH.
- Sử dụng công cụ chuyên dụng để tìm kiếm là Elastisearch, giúp việc tìm kiếm các bản tin log đuọc dễ dàng, nhanh chóng và chính xác.
- Phân tích các số liệu các được từ các file log thành dạng số liệu, biểu đồ thống kê, tổng hợp lại trong các dashboard.
- Cơ chế cảnh báo qua Email, Slack.
- Khả năng tích hợp mạnh mẽ với các phần mềm khác bằng cách sử dụng cơ chế plugin, bạn có thể dễ dàng tích hợp Graylog với các LDAP, Graphite/Ganglia, Logstash, NetFlow...
-	Mở rộng với REST API.
    -	Web-interface
    -	Graylog Collector
    -	Stream Alerts
## 3. Thành phần

**Graylog có 4 thành phần chính :**

- Graylog Server: Nhận, xử lý các bản tin và truyền thông với các thành phần khác – Cần CPU.
- Elasticsearch: Công cụ lưu trữ, tìm kiếm dữ liệu - tất cả phụ thuộc vào tốc độ I/O, Cần RAM.
- MongoDB: Lưu trữ metadata ( file cấu hình…). Chỉ cần cấu hình thấp.
- Web Interface: Cung cấp giao diện cho người dùng.

### 3.1 Elasticsearch

- ElasticSearch là một công cụ tìm kiếm cấp doanh nghiệp (enterprise-level search engine). Mục tiêu của nó là tạo ra một công cụ, nền tảng hay kỹ thuật tìm kiếm và phân tích trong thời gian thực (nhanh chóng và chính xác), cũng như cách để nó có thể áp dụng hay triển khai một cách dễ dàng vào nguồn dữ liệu (data sources) khác nhau.
- Nguồn dữ liệu nói ở trên trên bao gồm các cơ sở dữ liệu nổi tiếng như MS SQL, PostgreSQL, MySQL, ... mà nó có thể văn bản (text), thư điện tử (email), pdf, ... nói chung tất tần tật mọi thứ liên quan tới dữ liệu có văn bản.
- ElasticSearch được phát triển bởi Shay Banon và dựa trên Apache Lucene, ElasticSearch là một bản phân phối mã nguồn mở cho việc tìm kiếm dữ liệu trên máy chủ. Đây là một giải pháp mở rộng, hỗ trợ tìm kiếm thời gian thực mà không cần có một cấu hình đặc biệt. Nó đã được áp dụng bởi một số công ty, bao gồm cả StumbleUpon và Mozilla. ElasticSearch được phát hành theo Giấy phép Apache 2.0.

**Tóm lại :**

- Elasticsearch là một search engine.
- Elasticsearch được kế thừa từ Lucene Apache
- Elasticsearch thực chất hoặt động như 1 web server, có khả năng tìm kiếm nhanh chóng (near realtime) thông qua giao thức RESTful
- Elasticsearch có khả năng phân tích và thống kê dữ liệu
- Elasticsearch chạy trên server riêng và đồng thời giao tiếp thông qua RESTful do vậy nên nó không phụ thuộc vào client viết bằng gì hay hệ thống hiện tại của bạn viết bằng gì. Nên việc tích hợp nó vào hệ thống bạn là dễ dàng, bạn chỉ cần gửi request http lên là nó trả về kết quả.
- Elasticsearch là 1 hệ thống phân tán và có khả năng mở rộng tuyệt vời (horizontal scalability). Lắp thêm node cho nó là nó tự động auto mở rộng cho bạn.
- Elasticsearch là 1 open source được phát triển bằng Java

- ELASTIC-SEARCH có thể tích hợp được với tất cả các ứng dụng sử dụng các loại ngôn ngữ sau.
    -	Java
    -	JavaScript
    -	Groovy
    -	.NET
    -	PHP
    -	Perl
    -	Python
    -	Ruby

**Những ai đã dùng Elasticsearch:**
- Wikimedia
- athenahealth
- Adobe Systems
- Facebook
- StumbleUpon Mozilla,
- Amadeus IT Group
- Quora
- Foursquare
- Etsy
- SoundCloud
- GitHub
- FDA
- CERN
- Stack Exchange
- Center for Open Science
- Reverb
- Netflix
- Pixabay
- Motili
- Sophos
- Slurm Workload Manager

p