# Những sai lầm phát triển cơ sở dữ liệu phổ biến của các nhà phiển triển phần mềm?  
* nguồn https://stackoverflow.com/questions/621884/database-development-mistakes-made-by-application-developers*  
1. Không sử dụng chỉ mục thích hợp  
Nó là một thứ tương đối dễ nhưng vẫn luôn xảy ra. Các khóa ngoại nên có chỉ mục. Nếu bạn sử dụng một trường trong WHERE, bạn nên (có lẽ) có chỉ mục ở đó. Các chỉ mục như vậy thường bao gồm nhiều cột dựa trên truy vấn mà bạn cần thực thi.  
2. Không thực thi các tham chiếu ràng buộc
Cơ sở dữ liệu của bạn có thể thay đổi tại đây, nhưng nếu cơ sở dữ liệu của bạn hỗ trợ các tham chiếu ràng buộc--có nghĩa là tất cả khóa ngoại được đảm bảo để trỏ đến một thực thể tồn tại--bạn nên sử dụng nó.  
Khá là hay gặp lỗi này trong các cơ sở dữ liệu SQL. Tôi không tin rằng MyISAM hỗ trợ nó. InnoDB thì có. Bạn sẽ tìm thấy những người sử dụng MyISAM hoặc ai đó sử dụng InnoDB, nhưng dù thế nào đi nữa cũng không sử dụng nó.  
Đọc thêm ở đây:  
    * [Những giàng buộc như NOT NULL và FOREIGN KEY quan trọng thế nào nếu tôi luôn kiểm soát đầu vào với php?](https://stackoverflow.com/questions/382309/how-important-are-constraints-like-not-null-and-foreign-key-if-ill-always-contr)  
    * [Khóa ngoại có thật sự cần thiết trong thiết kế cơ sở dữ liệu?](https://stackoverflow.com/questions/18717/are-foreign-keys-really-necessary-in-a-database-design)  
    * [Khóa ngoại có thật sự cần thiết trong thiết kế cơ sở dữ liệu?](https://www.diovo.com/2008/08/are-foreign-keys-really-necessary-in-a-database-design/)
3. Sử dụng các khóa tự nhiên thay vì các khóa chính đại diện (kỹ thuật)  
Khóa tự nhiên là khóa dựa trên dữ liệu thực tế bên ngoài mà (có vẻ) duy nhất. Ví dụ điển hình là mã sản phẩm, 2 chữ mã tiểu bang(US), mã số bảo mật xã hội, v.v... Các khóa đại diên hoặc kỹ thuật là những khóa mà tuyệt đối không có ý nghĩa bên ngoài hệ thống. Nó là những sáng tạo chỉ dùng để xác định thực thể và thường là trường tự động tăng (SQL Server, MySQL, khác nữa) hoặc là trình tự (đáng chú ý nhất là Oracle).  
Theo quan điểm của tôi thì bạn nên *luôn luôn* sử dụng khóa đại diện. Vấn đề này được đưa ra trong các câu hỏi:  
    * [Bạn thích khóa chính của bạn như thế nào?](https://stackoverflow.com/questions/404040/how-do-you-like-your-primary-keys)  
    * [Cách làm nào tốt nhất cho khóa chính trong bảng là gì?](https://stackoverflow.com/questions/337503/whats-the-best-practice-for-primary-keys-in-tables)  
    * [Sử dụng định dạng khóa chính nào trong trường hợp này.](https://stackoverflow.com/questions/506164/use-item-specific-prefixes-and-autonumber-for-primary-keys)  
    * [Khóa đại diện và khóa tự nhiên/kinh tế](https://stackoverflow.com/questions/63090/surrogate-vs-natural-business-keys)  
    * [Tôi có nên có một trường khóa chính cho lĩnh vực riêng](https://stackoverflow.com/questions/166750/should-i-have-a-dedicated-primary-key-field)  
    
    Nó là một phần của vấn đề đang gây tranh cãi mà bạn không nhận được sự đồng tình chung. Trong khi bạn có thể tìm thấy một số người cho rằng khóa tự nhiên trong một số trường hợp là OK, bạn sẽ không tìm thấy bất cứ sự chỉ trích nào của khóa đại diện cũng như cho rằng nó không cần thiết. Đó là một nhược điểm khá nhỏ nếu bạn hỏi tôi.  
    Hãy nhớ rằng, [các quốc gia có thể ngưng tồn tại](https://en.wikipedia.org/wiki/ISO_3166-1) (ví dụ như Nam Tư)
  
\*\* 4. Viết các câu truy vấn yêu cầu DISTINCT để làm việc \*\*  
Bạn thường nhìn thấy nó trong truy vấn tạo bởi ORM. Nhìn vào đầu ra log từ Hibernate và bạn sẽ nhìn thấy tất cả các truy vấn bắt đầu với SELECT DISTINCT ...  
Nó là một chút của lối tắt để đảm bảo bạn không trả lại dãy bản sao và vì vậy lấy được các đối tượng bản sao. Đôi khi bạn sẽ thấy ai đó làm nó rất tốt. Nếu bạn nhìn nó quá nhiều, nó là thực sự đáng báo động. Không phải DISTINCT là xấu hoặc không phải là ứng dụng có hiệu lực. Nó đúng (về 2 mặt) nhưng không phải là một đại diện hay một sự thay thế tạm thời để viết các câu truy vấn chính xác.  
Từ [Why I Hate DISTINCT]:  
> Trường hợp mọi thứ bắt đầu trở nên chua theo ý kiến của tôi là khi một nhà phát triển đang xây dựng truy vấn đáng kể, joining bảng lại với nhau, và bất thình lình anh ta nhận ra rằng nó trông giống như anh đang nhận được hàng lặp lại (hoặc thậm chí nhiều hơn) hàng và phản ứng ngay lập tức của anh ấy và 'giải pháp' của anh đối với 'vấn đề' này là ném vào từ khóa DISTINCT và POOF tất cả những rắc rối của anh ấy biến mất .  

5.Khuyến khích tập hợp các kết nối  
Một lỗi thường gặp khác trong phát triển ứng dụng cơ sở dữ liệu là không nhận ra được tập hợp tốn kém hơn bao nhiêu so với kết nối.  
Để cho bạn thấy ý tưởng này phổ biến như thế nào, tôi đã viết nó trong chủ đề này một vài lần ở đây và đã bị vote down nhiều lần. Ví dụ như:  
Từ [SQL statement - “join” vs “group by and having](https://stackoverflow.com/questions/477006/select-values-that-meet-different-conditions-on-different-rows/477013#477013)  
> Truy vẫn đầu tiên:
  
SELECT userid FROM userrole WHERE roleid IN (1, 2, 3) GROUP by userid HAVING COUNT(1) = 3
> Query time: 0.312 s
  
> Truy vấn thứ 2:  

SELECT t1.userid FROM userrole t1 JOIN userrole t2 ON t1.userid = t2.userid AND t2.roleid = 2 JOIN userrole t3 ON t2.userid = t3.userid AND t3.roleid = 3 AND t1.roleid = 1
> Query time: 0.016 s  

> Đúng rồi. Phương pháp join do tôi đề xuất là nhanh **gấp 20 lần so với phiên bản tổng hợp**. 

6.Không đơn giản hóa các truy vấn phức tạp thông qua view.
Không phải tất cả các nhà cung cấp cơ sở dữ liệu đều hỗ trợ lượt xem, nhưng chúng có thể đơn giản hóa truy vấn rất nhiều được sử dụng một cách khôn ngoan. Ví dụ như trong một dự án tôi sử dụng [a generic Party model ](http://tdan.com/a-universal-person-and-organization-data-model/5014) cho CRM. Đó là một mô hình kỹ thuật rất mạnh mẽ và linh hoạt nhưng có thể sử dụng rất nhiều kết nối. Trong mô hình đó có:  
* **Party**: con người và tổ chức  
* **Party Role**: Những thứ mà các bên làm, ví dụ như Employee và Employer;  
* **Party Role Relationship**: các vai trò đó liên quan đến nhau như thế nào.  

Ví dụ:  
* Ted là 1 con người, là 1 loại phụ của bữa tiệc;
* Ted có nhiều vai trò , 1 trong số chúng là nhân viên;
* Intel là 1 tổ chức, là 1 tập con của bữa tiệc;
* Intel có nhiều luật, 1 trong số chúng là nhà tuyển dụng;
* Intel tuyển Ted, nghĩa là có mối quan hệ giữa các vai trò tương ứng của chúng.

Vì vậy có 5 bảng để kết nối Ted với nhà tuyển dụng của anh ấy. Giả định rằng các nhân viên là Persion (không phải tổ chức) và cung cấp khung nhìn hỗ trợ này:  
CREATE VIEW vw_employee AS SELECT p.title, p.given_names, p.surname, p.date_of_birth, p2.party_name employer_name FROM person p JOIN party py ON py.id = p.id JOIN party_role child ON p.id = child.party_id JOIN party_role_relationship prr ON child.id = prr.child_id AND prr.type = 'EMPLOYMENT' JOIN party_role parent ON parent.id = prr.parent_id = parent.id JOIN party p2 ON parent.party_id = p2.id  
Và tự dưng bạn có một cách nhìn rất đơn giản về dữ liệu bạn muốn nhưng trên một mô hình dữ liệu linh hoạt hơn.  
7.Không lọc đầu vào  
Đây là một vấn đề lớn. Hiện tại tôi thích PHP nhưng bạn sẽ không biết bạn đang làm cái gì, nó thực sự dễ dàng để tạo web site có thể bị tấn công. Không có điều gì tổng kết nó tốt hơn là [story of little Bobby Tables](https://xkcd.com/327/)  
Dữ liệu được cung cấp bởi người sử dụng bằng các URL, hình thức dữ liệu và cookie nên luôn được coi là có tính thù địch và lọc. Đảm bảo bạn đang nhận được những gì bạn mong đợi.  
8.Không sử dụng các lệnh được chuẩn bị (soạn sẵn)
Các câu lệnh chuẩn bị(soạn sẵn) là khi bạn biên dịch 1 truy vấn khuyết dữ liệu sử dụng trong các lệnh thêm, cập nhật và mệnh đề WHERE  và sẽ cung cấp dữ liệu cho nó sau. Ví dụ:  
SELECT * FROM users WHERE username = 'bob'

vs

SELECT * FROM users WHERE username = ?

or

SELECT * FROM users WHERE username = :username  

dựa trên nền tảng của bạn.  
Tôi đã từng thấy các cơ sở dữ liệu bị mang đến đầu gối của họ bằng cách làm điều này. Thông thường, mỗi lần bất kỳ 1 cơ sở dữ liệu hiện đại nào gặp 1 câu truy vấn mới, nó phải biên dịch. Nếu nó gặp 1 câu truy vấn đã nhìn thấy trước đó, thì cơ sở dữ liệu cache câu truy vấn đã biên dịch và thực hiện nó. Bằng cách thực hiện câu truy vấn nhiều lần bạn đang cho phép cơ sở dữ liệu tìm kiếm và tối ưu thích hợp ( ví dụ, bằng cách ghim các câu truy vấn đã biên dịch trong bộ nhớ)  
Sử dụng các câu lệnh được chuẩn bị cũng sẽ cho bạn thống kê ý nghĩa về việc các câu truy vấn cụ thể được sử dụng với tần suất nào.  
Các câu lệnh được chuẩn bị cũng bảo vệ bạn tốt hơn với cách tấn công SQL injection.  
