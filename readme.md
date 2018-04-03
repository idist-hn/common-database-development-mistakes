## What are common database development mistakes made by application developers?

## Các sai lầm cơ bản trong phát triển cơ sở dữ liệu của các nhà phát triển ứng dụng?

_nguồn https://stackoverflow.com/questions/621884/database-development-mistakes-made-by-application-developers_

**1. Không sử dụng các chỉ số thích hợp**

Đây là điều khá đơn giản nhưng nó lại diễn ra khá thường xuyên. Các khoá ngoại thì nên được đánh index (chỉ mục). Nếu bạn sử dụng 1 trường vào mệnh đề WHERE thì bạn nên đánh index cho trường đó. Các chỉ mục như vậy thường chứa cả các cột trong câu truy vấn mà bạn cần thực thi.

**2. Không thực thi đầy đủ các tham chiếu**

Cơ sở dữ liệu của bạn có thể thay đổi ở bất cứ đâu nhưng nếu cơ sở dữ liệu của bạn có hỗ trợ tham chiếu đầy đủ khi tất cả các khoá ngoại đều trỏ đến một thực thể tồn tại, bạn nên sử dụng nó. Nó khá là bình thường khi gặp lỗi này trên cơ sở dữ liệu MySQL. Tôi không tin là MyISAM có hỗ trợ nó. nhưng InnoDB thì có. Bạn sẽ phải thì những người có sử dụng MyISAM hoặc những người sử dụng InnoDB nhưng lại không có ai sử dụng nó cả. ( nó ở đây là tham chiếu đầy đủ).

Xem thêm:

- [Các ràng buộc như NOT NULL và FOREIGN KEY quan trọng như thế nào nếu bạn luôn luôn phải kiểm soát dữ liệu đầu vào cơ sở dữ liệu với php?](https://stackoverflow.com/questions/382309/how-important-are-constraints-like-not-null-and-foreign-key-if-ill-always-contr)
- [Các khoá ngoại có thực sự cần thiết trong việc thiết kế cơ sở dữ liệu?](https://stackoverflow.com/questions/18717/are-foreign-keys-really-necessary-in-a-database-design)
- [Các khoá ngoại có thực sự cần thiết trong việc thiết kế cơ sở dữ liệu?](http://www.diovo.com/2008/08/are-foreign-keys-really-necessary-in-a-database-design/)

**3. Sử dụng các khoá tự nhiên tốt hơn các khoá đại diện ( các khoá kĩ thuật )**

Các khoá tự nhiên là những khoá cơ bản nhất dựa trên các dữ liệu đời thực có ý nghĩa duy nhất. Ví dụ như các mã sản phẩm, 2 kí tự trong mã bang của Mỹ, hoặc ví dụ như mã số an sinh xã hội ( giống số chứng minh nhân dân). Các khoá chính thay thế hoặc các khoá chính kĩ thuật là các khoá hoàn toàn không có ý nghĩa bên ngoài hệ thống. Chúng được tạo ra chỉ để xác định các thực thể và thường là những trường tự động tăng ( ví dụ như SQL Server, MySQL,...) hoặc sử dụng các chuỗi (đáng chú ý nhất như Oracle).

Theo ý kiến của tôi thì bạn nên **luôn luôn** sử dụng các khoá đại diện. Vấn đề này đã được đề cập trong những câu hỏi sau:

- [Bạn cảm thấy khoá chính của mình như thế nào?](https://stackoverflow.com/questions/404040/how-do-you-like-your-primary-keys)
- [Thói quen tốt nhất của bạn về khoá chính trong bảng là gì?](https://stackoverflow.com/questions/337503/whats-the-best-practice-for-primary-keys-in-tables)
- [Bạn sử dụng định dạng nào cho khoá chính trong trường hợp này?](https://stackoverflow.com/questions/506164/which-format-of-primary-key-would-you-use-in-this-situation)
- [So sánh khoá thay thế với khoá tự nhiên](https://stackoverflow.com/questions/63090/surrogate-vs-natural-business-keys)
- [Tôi có nên sử dụng 1 trường riêng để làm khoá chính không?](https://stackoverflow.com/questions/166750/should-i-have-a-dedicated-primary-key-field)

Đây là vấn đề gây nhiều tranh cãi mà bạn sẽ không thể tìm được quan điểm chung. Trong khi bạn có thể tìm được một số người nghĩ là khoá tự nhiên rất tốt trong một số trường hợp, bạn cũng sẽ không tìm thấy bất kì quan điểm trái ngược nào như việc khoá đại diện không cần thiết. Đây là 1 nhược điểm nhỏ nếu bạn hỏi tôi.

Nhớ rằng, [các quốc gia còn có thể không tồn tại](http://en.wikipedia.org/wiki/ISO_3166-1) (ví dụ như Yugoslavia).

**4. Viết những câu truy vấn cần thiết sử dụng DISTINCT để làm việc**

Bạn thường thấy cái này trong những câu truy vấn được tạo bởi ORM. Kiểm tra các log đầu ra từ Hibernate và bạn sẽ thấy các câu truy vấn bắt đầu với:

SELECT DISTINCT ...

Đây là một đoạn ngắn để đảm bảo không trả về những bản ghi bị lặp và lấy những đối tượng lặp. Đôi khi bạn sẽ gặp những người làm việc này. Nếu bạn thấy nó nhiều lần thì đó thực sự đáng báo động. Không phải DISTINCT không tốt hay nó không có những ứng dụng phù hợp. Nó thật sự không phải 1 trường đại diện hoặc trường thay thế để viết các câu truy vấn chính xác.


Từ bài viết [Tại sao tôi ghép DISTINCT](http://weblogs.sqlteam.com/markc/archive/2008/11/11/60752.aspx):

> Where things start to go sour in my opinion is when a developer is building substantial query, joining tables together, and all of a sudden he realizes that it **looks** like he is getting duplicate (or even more) rows and his immediate response...his "solution" to this "problem" is to throw on the DISTINCT keyword and **POOF** all his troubles go away.

> Theo tôi lúc mà mọi thứ trở nên khó chịu khi các lập trình viên xây dựng 1 lượng truy vấn lớn, kết hợp các bảng với nhau, và bất ngờ khi anh ta nhận ra là anh ta đang lấy cả các bản ghi bị trùng lặp (thậm trí rất nhiều) và anh ta phản ứng lại ngay lập tức... Các giải pháp của anh ta cho vấn đề này là sử dụng khoá DISTINCT và khiến tất cả các lỗi này biến mất.

**5. Khuyến khích tập hợp các kết nối**

Một lỗi thường gặp nữa về cơ sở dữ liệu của các lập trình viên là không phát hiện thường mất nhiều công sức hơn (mệnh đề GROUP BY) mệnh đề join.

Để tôi gợi ý 1 ý tưởng về vấn đề phổ biến này, tôi đã viết về chủ đề này vài lần những lại bị vote down. Ví dụ:

Từ  [Câu lệnh SQL - “join” và “group by and having”](https://stackoverflow.com/questions/477006/sql-statement-join-vs-group-by-and-having/477013#477013):

> First query:

SELECT userid
FROM userrole
WHERE roleid IN (1, 2, 3)
GROUP by userid
HAVING COUNT(1) = 3

> Thời gian truy vấn: 0.312 s

> Second query:

SELECT t1.userid
FROM userrole t1
JOIN userrole t2 ON t1.userid = t2.userid AND t2.roleid = 2
JOIN userrole t3 ON t2.userid = t3.userid AND t3.roleid = 3
AND t1.roleid = 1

> Thời gian truy vấn: 0.016 s

> Đúng vậy. Phiên bản JOIN tôi thực hiện **nhanh gấp 2 lần so với phiên bản GROUP.**

**6.Không đơn giản hoá các truy vấn phức tạp qua chế độ view**

Không phải tất cả các nhà cung cấp cơ sở dữ liệu hỗ trợ chế độ view nhưng chúng đều có thể đơn giản hóa các câu truy vấn nếu sử dụng 1 cách khôn ngoan. Ví dụ một trong những dự án của tôi có sử dụng [generic Party model](http://www.tdan.com/view-articles/5014/) cho CRM. Đấy là 1 kỹ thuật mô hình rất mạnh và linh hoạt có thể sử dụng nhiều phép nối. Trong mô hình này có:

- **Party**: con người và các tổ chức;
- **Party Role**: các việc mà nhóm có thể làm, như nhân viên hay nhà tuyển dụng;
- **Party Role Relationship**: mối quan hệ các luật như thế nào.

Ví dụ:

- Ted là 1 Person, là 1 tập con của Party;
- Ted có nhiều quyền, 1 trong số đó là Employee;
- Intel là 1 tổ chức, là 1 tập con của Party;
- Intel có nhiều quyền, 1 trong số đó là Employer;
- Intel tuyển dụng Ted, nghĩa là có quan hệ giữa các luật tương ứng của chúng.

Do vậy cần 5 bảng để kết nối Ted với nhà tuyển dụng. Giả sử các nhân viên là Persons (không phải 1 tổ chức) và cung cấp cho anh ấy các view hỗ trợ:

CREATE VIEW vw_employee AS
SELECT p.title, p.given_names, p.surname, p.date_of_birth, p2.party_name employer_name
FROM person p
JOIN party py ON py.id = p.id
JOIN party_role child ON p.id = child.party_id
JOIN party_role_relationship prr ON child.id = prr.child_id AND prr.type = 'EMPLOYMENT'
JOIN party_role parent ON parent.id = prr.parent_id = parent.id
JOIN party p2 ON parent.party_id = p2.id

Và bạn có 1 view rất đơn giản về các dữ liệu bạn muốn những ở mô hình dữ liệu linh hoạt cao.

**7. Không kiểm soát đầu vào**

Đây là vấn đề lớn. Bây giờ tôi thích PHP nhưng nếu bạn không biết những gì bạn đang làm, nó rất dễ khiến site của bạn bị tấn công. Không có cái gì tổng quan hơn là [câu chuyện về Bobby Table bé nhỏ](http://xkcd.com/327/).

Dữ liệu được cung cấp cho người dùng thông qua các Urls, các form dữ liệu **và cookies** nên luôn luôn được coi là không tốt và cần được lọc. Chắc chắn là bạn đang nhận được những gì mình mong đợi

**8. Không sử dụng các câu lệnh dạng chờ**

Các câu lệnh chuẩn bị là khi bạn biên dịch 1 truy vấn khuyết dữ liệu được sử dụng trong các lệnh thêm, cập nhật và mệnh đề WHERE sẽ cung cấp dữ liệu cho nó sau. Ví dụ:

SELECT * FROM users WHERE username = 'bob'

vs

SELECT * FROM users WHERE username = ?

hoặc

SELECT * FROM users WHERE username = :username

phụ thuộc vào nền tảng bạn đang sử dụng.

Tôi đã từng thấy các cơ sở dữ liệu bị phá hủy vì điều này. Thông thường, mỗi lần bất kỳ 1 cơ sở dữ liệu hiện đại nào thực hiện 1 câu truy vấn mới, nó phải biên dịch câu truy vấn đó. Nếu nó gặp 1 câu truy vấn đã nhìn thấy trước đó, thì cơ sở dữ liệu có cơ hội cache câu truy vấn đã biên dịch và thực hiện. Bằng cách thực hiện câu truy vấn nhiều lần, bạn đang chó phép cơ sở dữ liệu để tìm kiếm và tối ưu thích hợp ( ví dụ, bằng cách ghim các câu truy vấn đã biên dịch trong bộ nhớ)
Sử dụng các câu truy vấn chờ cũng sẽ cung cấp cho bạn các số liệu thống kê về tần suất các câu truy vấn nhất định được sử dụng.
Các câu lệnh được chuẩn bị cũng bảo vệ bạn tốt hơn với các tấn công như SQL injection.


**9. Thiếu chuẩn hoá**

[Chuẩn hoá cơ sở dữ liệu](http://en.wikipedia.org/wiki/Database_normalization) về cơ bản thì đây là quá trình tối ưu hoá việc thiết kế cơ sở dữ liệu hoặc tổ chức các dữ liệu của bạn vào các bảng.

Chỉ trong tuần vừa rồi tôi gặp 1 đoạn code mà họ tách nó ra thành 1 mảng và thêm chúng vào như 1 trường trong cơ sở dữ liệu. Quá trình chuẩn hoá là xử lý từng phần tử của mảng đó như 1 dòng riêng biệt trong bảng 
con (tương đương 1 mối quan hệ 1 nhiều).

Điều này cũng được nhắc tới tại [Phương thức tốt nhất để lưu 1 danh sách các ID của người dùng](https://stackoverflow.com/questions/620645/best-method-for-storing-a-list-of-user-ids):

> Tôi cũng đã từng thấy 1 hệ thống khác lưu trữ danh sách trong 1 mảng tuần tự của PHP.

Tuy nhiên vấn đề thiếu chuẩn hoá vẫn còn nhiều dạng.

Xem thêm:

- [Chuẩn hoá: bao nhiêu là đủ?](http://www.techrepublic.com/article/normalization-how-far-is-far-enough/)
- [SQL by Design: Tại sao bạn cần chuẩn hoá cơ sở dữ liệu ](http://www.sqlmag.com/Article/ArticleID/4887/sql_server_4887.html)

**10. Chuẩn hoá quá nhiều**

Nhìn chung vấn đề này trái ngước với vấn đề trước nhưng việc chuẩn hoá cũng như là những việc khác, nó được xem là một công cụ. Nó không có điểm kết thúc cụ thể nào cả. Tôi nghĩ là rất nhiều những lập trình viên quên mất điều này, bắt đầu hiểu sai 1 "công cụ" thành 1 thứ có thể "kết thúc" vấn đề. Unit testing là một ví dụ điển hình về điều này.

Lúc tôi làm việc trên 1 hệ thống có hệ thống phân cấp cho người dùng cực lớn, tôi đã gặp 1 vài điều như này:

Licensee -&gt;  Dealer Group -&gt; Company -&gt; Practice -&gt; ...

giống như kiểu bạn phải join khoảng 11 bảng khác nhau trước khi bạn muốn lấy bất kỳ dữ liệu gì. Nó là 1 ví dụ rất tốt cho việc chuẩn hoá quá nhiều.

Một điều nữa, hãy cẩn thận và xem xét việc tối ưu chuẩn hoá có thể mang lại những lợi ích đáng kể nhưng bạn cũng phải cẩn thận khi làm điều này.

Xem thêm:

- [Vì sao chuẩn hoá cơ sở dữ liệu quá nhiều cũng không tốt](http://www.selikoff.net/blog/2008/11/19/why-too-much-database-normalization-can-be-a-bad-thing/)
- [Thiết kế cơ sở dữ liệu tới mức nào là đủ chuẩn?](https://stackoverflow.com/questions/496508/how-far-to-take-normalization-in-database-design)
- [Khi nào bạn không cần chuẩn hoá cơ sở dữ liệu](http://www.25hoursaday.com/weblog/CommentView.aspx?guid=cc0e740c-a828-4b9d-b244-4ee96e2fad4b)
- [Chuẩn hoá có thể không chuẩn](http://www.codinghorror.com/blog/archives/001152.html)
- [Nguyên nhân các cuộc tranh luận về chuẩn hoá trên Coding Horror](http://highscalability.com/mother-all-database-normalization-debates-coding-horror)

**11. Sử dụng các exclusive arcs ( truy vấn lòng vòng )**

Việc truy vấn lòng vòng là 1 trong những lỗi thường gặp khi 1 bảng được tạo ra với 2 hay nhiều khoá ngoại và 1 trong số đó lại có thể không null.  **Lỗi lớn đấy.** Đây là điều khiến nó trở lên khó khăn hơn nhiều trong việc duy trì tính toàn vẹn dữ liệu. Sau tất cả, ngay cả khi sử dụng các ràng buộc tham chiếu vẫn không có gì ngăn được việc đặt 2 hay nhiều hơn các khoá ngoại (mặc dù đã kiểm tra các ràng buộc phức tạp).

từ bài viết [Hướng dẫn thực hành thiết kế cơ sở dữ liệu quan hệ](http://books.google.com.au/books?id=7ZAk0YiKQV0C&pg=PA110&lpg=PA110&dq=%22exclusive+arc%22+database&source=bl&ots=AyNPWsac__&sig=gBFIerXckQlVpRdd6ycI5JEgq3U&hl=en&ei=PzGzSZfrFcPVkAWWyZDZBA&sa=X&oi=book_result&resnum=1&ct=result):

  > Chúng tôi không khuyến khích xây dựng các câu truy vấn phức tạp bất cứ khi nào có thể, vì 1 lý do rằng nó có thể rất lộn xộn khi viết code và khiến việc bảo trì khó khăn.

**12. Không phân tích hiệu suất các lệnh truy vấn**

Pragmatism reigns supreme, particularly in the database world. If you're sticking to principles to the point that they've become a dogma then you've quite probably made mistakes. Take the example of the aggregate queries from above. The aggregate version might look "nice" but its performance is woeful. A performance comparison should've ended the debate (but it didn't) but more to the point: spouting such ill-informed views in the first place is ignorant, even dangerous.

Theo chủ nghĩa thực dụng, đặc biệt là trong môi trường cơ sở dữ liệu. Nếu bạn vẫn đang cố giữ nguyên tắc khi chúng đã trở nên độc đoán thì bạn thực sự đang mắc sai lầm đấy. Lấy 1 ví dụ về các truy vấn sử dụng GROUP bên trên. Phiên bản sử dụng GROUP có vẻ nhìn "ổn" nhưng hiệu suất của nó thì tệ. Việc tranh luận về so sánh về hiệu suất nên kết thúc(nhưng nó không) nhưng nhớ thêm 1 điều rằng : việc sử dụng quá nhiều view thông báo xấu ngay trong vị trí đầu tiên là không ổn, thậm chí nó còn nguy hiểm.


**13. Quá tin vào UNION ALL và đặc biệt là cấu trúc UNION**

Một UNION trong SQL chỉ đơn thuần nối các tập dữ liệu đồng nhất, có nghĩa là chúng phải có cùng kiểu và số cột. Sự khác biệt giữa chúng là UNION ALL là một sự ghép nối đơn giản và nên được ưu tiên hơn bất cứ khi nào có thể, trong khi một UNION ngầm sẽ sử dụng một DISTINCT để loại bỏ các bản sao trùng lặp.

UNIONs, như DISTINCT, có chỗ của chúng. Có các ứng dụng hợp lệ. Nhưng nếu bạn thấy mình đang sử dụng chúng rất nhiều, đặc biệt trong các truy vấn phụ, thì có thể bạn đang làm sai gì đó. Đó có thể là trường hợp xây dựng truy vấn kém hoặc mô hình dữ liệu được thiết kế kém buộc bạn phải làm những việc như vậy.

UNION, đặc biệt khi sử dụng trong các kết nối hoặc các truy vấn phụ phụ thuộc, có thể làm tê liệt cơ sở dữ liệu. Cố gắng tránh sử dụng chúng bất cứ khi nào có thể.

**14. sử dụng các điều kiện or trong câu truy vấn**

Cái này có vẻ vô hại. Và sau tất cả, ANDs cũng OK. Vậy mệnh đề OR liệu có thực sự tốt? Sai rồi. Thông thường điều kiện AND sẽ **hạn chế** tập dữ liệu trong khi điều kiện OR **làm tăng** tập dữ liệu nhưng không phải theo cách khiến chúng được tối ưu. Đặc biệt là khi các điều kiện OR khác nhau có thể giao nhau khiến cho việc tối ưu hóa hiệu quả hơn với các phép DISTINCT.

Bad:

... WHERE a = 2 OR a = 5 OR a = 11

Better:

... WHERE a IN (2, 5, 11)

Bây giờ việc tối ưu lênh SQL của bạn có thể hiệu quả từ câu truy vấn đầu tiên tới câu thứ 2. Nhưng cũng có thể không. Đừng làm như thế.

**15. Không thiết kế các mô hình dữ liệu để vay mượn các giải pháp hiệu suất cao**

Đây là 1 điểm khó đánh giá. Nó thường được đánh giá thông qua hiệu quả cửa nó. Nếu bạn thấy mình đang viết các lệnh truy vấn cho các việc đơn giản hoặc các truy vấn chỉ để tìm kiếm các thông tin tương đối đơn giản nhưng lại không có hiệu quả, bạn chắc chắn đang sử dụng một mô hình dữ liệu tệ ( mô hình dữ liệu có hiệu suất kém).

Bằng 1 cách nào đó điều này bao gồm cả các điều trước đó nhưng nó lại có thêm 1 cảnh báo là các câu truy vấn đên được tối ưu đầu tiên trong khi nó chỉ nên hoàn thành thứ 2. Đầu tiên và cũng là quan trọng nhất, bạn nên chắc chắn mô hình dữ liệu của bạn tốt trước khi cố gắng tối ưu hiệu suất. Giống như Knuth đã nói:

> Tối ưu hoá sớm là nguồn gốc của mọi thứ tệ hại.

**16. Sử dụng Database Transactions sai**

Tất cả các sự kiện thay đổi dữ liệu trong 1 tiến trình nên tuân thủ mô hình nguyên tử (atomic). Ví dụ nếu các hành động thực hiện thành công, nó sẽ cập nhật đầy đủ. Nếu nó không thành công, dữ liệu sẽ không thay đổi. Không để các thay đổi kiểu "hoàn thành 1 nửa".

Lý tưởng nhất, cách tốt nhất để đạt được điều này là thiết kế toàn bộ hệ thống nên cố gắng hỗ trợ toàn bộ thay đổi dữ liệu qua từng câu lệnh INSERT/UPDATE/DELETE. Trong trường hợp này, không có xử lý transaction đặc biệt nào cần thiết cả, vì engine cơ sở dữ liệu của bạn nên làm điều này tự động.

tuy nhiên, nếu bất kì tiến trình nào yêu cầu nhiều câu lệnh thực hiện như là 1 đơn vị lưu trữ dữ liệu trong trạng thái thích hợp, khi đó việc quản lý các giao tác (Transition Control) là cần thiết.

- Khởi động Giao tác trước câu lệnh đầu tiên.
- Commit Giao tác sau câu lệnh cuối cùng.
- Khi gặp bất kỳ lỗi nào, hoàn tác lại giao tác đó ngay. Và chú ý quan trọng nữa! Đừng quên bỏ qua/ dừng tất cả các câu lệnh sau các lỗi.

 *And very NB = And very Note Bene = And very Note Well*
 
Also recommended to pay careful attention to the subtelties of how your database connectivity layer, and database engine interact in this regard. 
Gợi ý khác nữa là hãy chú ý tới các dịch vụ phụ khác trong việc kết nối các lớp và engine tương tác của cơ sở dữ liệu trong vấn đề này.

**17. Không hiểu mô hình 'set-based'**

  Ngôn ngữ SQL tuân theo mô hình cụ thể phù hợp với các loại vấn đề cụ thể. Mặc dù cung cấp nhiều phần mở rộng cụ thể, việc xung đột ngôn ngữ đẻ giải quyết vấn đề này là không đáng kể trong các ngôn ngữ như Java, C#, Delphi,...

Phần còn lại thì có vài cách  biểu thị:

- Áp dụng quá nhiều các phương pháp hoặc các logic bắt buộc không cần thiết lên cơ sở dữ liệu.
- Sử dụng con trỏ không phù hợp quá nhiều. Nhất là khi câu truy vấn đơn giản đã là đủ.
- Một giả định không chính xác rằng các trigger sẽ thực hiện trên mỗi dòng trong khi cập nhập cùng 1 lúc nhiều dòng.

Để xác định phân chia các trách nhiệm rõ ràng, và cố gắng sử dụng các công cụ thích hợp để giải quyết từng vấn đề.