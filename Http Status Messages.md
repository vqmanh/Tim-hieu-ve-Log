## HTTP Status Messages

### Các HTTP Status Messages hay còn được biết đển là các http code có 5 loại chính và ý nghĩa các mã cơ bản đó là:
#### 1xx: Thông tin (yêu cầu (request) đã được nhận, tiếp tục tiến trình xử lí)
- 100 Continue: Server đã nhận được yêu cầu header
- 101 Switching Protocols
- 103 Checkpoint
#### 2xx: Thành công
- 200 OK: yêu cầu gửi lên server đã được tiếp nhận thành công
- 201 Created
- 202 Accepted
- 203 Non-Authoritative Information
- 204 No Content
- 205 Reset Content
- 206 Partial Content
#### 3xx: Điều hướng
- 300 Multiple Choices
- 301 Moved Permanently: Trang web yêu cầu đã được chuyển đến một địa chỉ URL mới
- 302 Found : Trang web yêu cầu đã được chuyển tạm thời đến một địa chỉ URL mới
- 303 See Other
- 304 Not Modified
- 306 Switch Proxy
- 307 Temporary Redirect
- 308 Resume Incomplete
#### 4xx: Lỗi Client
- 400 Bad Request: Lỗi cú pháp, yêu cầu không thể thực hiện được
- 401 Unauthorized
- 402 Payment Required
- 403 Forbidden: Không được phép truy cập vào đây
- 404 Not Found: Không tìm thấy trang địa chỉ với URL hiện tại
#### 5xx: Lỗi Server
- 500 Internal Server Error: Một thông báo lỗi chung, được đưa ra khi không có thông báo cụ thể nào khác phù hợp
- 501 Not Implemented: Máy chủ hoặc không nhận ra phương thức yêu cầu, hoặc nó không có khả năng thực hiện yêu cầu
- 502 Bad Gateway: Máy chủ đã hoạt động như một cổng hoặc proxy và nhận được phản hồi không hợp lệ từ máy chủ
- 503 Service Unavailable: Máy chủ hiện không có (quá tải)
- 504 Gateway Timeout: Máy chủ hoạt động như một gateway hoặc proxy và không nhận được phản hồi kịp thời từ máy chủ phía trên
