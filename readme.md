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

**3. Using natural rather than surrogate (technical) primary keys**

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

**4. Viết những câu truy vấn cần thiết

sử dụng DISTINCT để làm việc**

Bạn thường thấy cái này trong những câu truy vấn được tạo bởi ORM. Kiểm tra các log đầu ra từ Hibernate và bạn sẽ thấy các câu truy vấn bắt đầu với:

SELECT DISTINCT ...

This is a bit of a shortcut to ensuring you don't return duplicate rows and thus get duplicate objects. You'll sometimes see people doing this as well. If you see it too much it's a real red flag. Not that 

DISTINCT is bad or doesn't have valid applications. It does (on both counts) but it's not a surrogate or a stopgap for writing correct queries.

From [Why I Hate DISTINCT](http://weblogs.sqlteam.com/markc/archive/2008/11/11/60752.aspx):

> Where things start to go sour in my opinion is when a developer is building substantial query, joining tables together, and all of a sudden he realizes that it **looks** like he is getting duplicate (or even more) rows and his immediate response...his "solution" to this "problem" is to throw on the DISTINCT keyword and **POOF** all his troubles go away.

**5. Favouring aggregation over joins**

Another common mistake by database application developers is to not realize how much more expensive aggregation (ie the 

GROUP BY clause) can be compared to joins.

To give you an idea of how widespread this is, I've written on this topic several times here and been downvoted a lot for it. For example:

From [SQL statement - “join” vs “group by and having”](https://stackoverflow.com/questions/477006/sql-statement-join-vs-group-by-and-having/477013#477013):

> First query:

SELECT userid
FROM userrole
WHERE roleid IN (1, 2, 3)
GROUP by userid
HAVING COUNT(1) = 3

> Query time: 0.312 s

> Second query:

SELECT t1.userid
FROM userrole t1
JOIN userrole t2 ON t1.userid = t2.userid AND t2.roleid = 2
JOIN userrole t3 ON t2.userid = t3.userid AND t3.roleid = 3
AND t1.roleid = 1

> Query time: 0.016 s

> That's right. The join version I proposed is **twenty times faster than the aggregate version.**

**6. Not simplifying complex queries through views**

Not all database vendors support views but for those that do, they can greatly simplify queries if used judiciously. For example, on one project I used a [generic Party model](http://www.tdan.com/view-articles/5014/) for CRM. This is an extremely powerful and flexible modelling technique but can lead to many joins. In this model there were:

- **Party**: people and organisations;
- **Party Role**: things those parties did, for example Employee and Employer;
- **Party Role Relationship**: how those roles related to each other.

Example:

- Ted is a Person, being a subtype of Party;
- Ted has many roles, one of which is Employee;
- Intel is an organisation, being a subtype of a Party;
- Intel has many roles, one of which is Employer;
- Intel employs Ted, meaning there is a relationship between their respective roles.

So there are five tables joined to link Ted to his employer. You assume all employees are Persons (not organisations) and provide this helper view:

CREATE VIEW vw_employee AS
SELECT p.title, p.given_names, p.surname, p.date_of_birth, p2.party_name employer_name
FROM person p
JOIN party py ON py.id = p.id
JOIN party_role child ON p.id = child.party_id
JOIN party_role_relationship prr ON child.id = prr.child_id AND prr.type = 'EMPLOYMENT'
JOIN party_role parent ON parent.id = prr.parent_id = parent.id
JOIN party p2 ON parent.party_id = p2.id

And suddenly you have a very simple view of the data you want but on a highly flexible data model.

**7. Not sanitizing input**

This is a huge one. Now I like PHP but if you don't know what you're doing it's really easy to create sites vulnerable to attack. Nothing sums it up better than the [story of little Bobby Tables](http://xkcd.com/327/).

Data provided by the user by way of URLs, form data **and cookies** should always be treated as hostile and sanitized. Make sure you're getting what you expect.

**8. Not using prepared statements**

Prepared statements are when you compile a query minus the data used in inserts, updates and 

WHERE clauses and then supply that later. For example:

SELECT * FROM users WHERE username = 'bob'

vs

SELECT * FROM users WHERE username = ?

or

SELECT * FROM users WHERE username = :username

depending on your platform.

I've seen databases brought to their knees by doing this. Basically, each time any modern database encounters a new query it has to compile it. If it encounters a query it's seen before, you're giving the database the opportunity to cache the compiled query and the execution plan. By doing the query a lot you're giving the database the opportunity to figure that out and optimize accordingly (for example, by pinning the compiled query in memory).

Using prepared statements will also give you meaningful statistics about how often certain queries are used.

Prepared statements will also better protect you against SQL injection attacks.

**9. Not normalizing enough**

[Database normalization](http://en.wikipedia.org/wiki/Database_normalization) is basically the process of optimizing database design or how you organize your data into tables.

Just this week I ran across some code where someone had imploded an array and inserted it into a single field in a database. Normalizing that would be to treat element of that array as a separate row in a child table (ie a one-to-many relationship).

This also came up in [Best method for storing a list of user IDs](https://stackoverflow.com/questions/620645/best-method-for-storing-a-list-of-user-ids):

> I've seen in other systems that the list is stored in a serialized PHP array.

But lack of normalization comes in many forms.

More:

- [Normalization: How far is far enough?](http://www.techrepublic.com/article/normalization-how-far-is-far-enough/)
- [SQL by Design: Why You Need Database Normalization ](http://www.sqlmag.com/Article/ArticleID/4887/sql_server_4887.html)

**10. Normalizing too much**

This may seem like a contradiction to the previous point but normalization, like many things, is a tool. It is a means to an end and not an end in and of itself. I think many developers forget this and start treating a "means" as an "end". Unit testing is a prime example of this.

I once worked on a system that had a huge hierarchy for clients that went something like:

Licensee -&gt;  Dealer Group -&gt; Company -&gt; Practice -&gt; ...

such that you had to join about 11 tables together before you could get any meaningful data. It was a good example of normalization taken too far.

More to the point, careful and considered denormalization can have huge performance benefits but you have to be really careful when doing this.

More:

- [Why too much Database Normalization can be a Bad Thing](http://www.selikoff.net/blog/2008/11/19/why-too-much-database-normalization-can-be-a-bad-thing/)
- [How far to take normalization in database design?](https://stackoverflow.com/questions/496508/how-far-to-take-normalization-in-database-design)
- [When Not to Normalize your SQL Database](http://www.25hoursaday.com/weblog/CommentView.aspx?guid=cc0e740c-a828-4b9d-b244-4ee96e2fad4b)
- [Maybe Normalizing Isn't Normal](http://www.codinghorror.com/blog/archives/001152.html)
- [The Mother of All Database Normalization Debates on Coding Horror](http://highscalability.com/mother-all-database-normalization-debates-coding-horror)

**11. Using exclusive arcs**

An exclusive arc is a common mistake where a table is created with two or more foreign keys where one and only one of them can be non-null.  **Big mistake.** For one thing it becomes that much harder to maintain data integrity. After all, even with referential integrity, nothing is preventing two or more of these foreign keys from being set (complex check constraints notwithstanding).

From [A Practical Guide to Relational Database Design](http://books.google.com.au/books?id=7ZAk0YiKQV0C&pg=PA110&lpg=PA110&dq=%22exclusive+arc%22+database&source=bl&ots=AyNPWsac__&sig=gBFIerXckQlVpRdd6ycI5JEgq3U&hl=en&ei=PzGzSZfrFcPVkAWWyZDZBA&sa=X&oi=book_result&resnum=1&ct=result):

> We have strongly advised against exclusive arc construction wherever possible, for the good reason that they can be awkward to write code and pose more maintenance difficulties.

**12. Not doing performance analysis on queries at all**

Pragmatism reigns supreme, particularly in the database world. If you're sticking to principles to the point that they've become a dogma then you've quite probably made mistakes. Take the example of the aggregate queries from above. The aggregate version might look "nice" but its performance is woeful. A performance comparison should've ended the debate (but it didn't) but more to the point: spouting such ill-informed views in the first place is ignorant, even dangerous.

**13. Over-reliance on UNION ALL and particularly UNION constructs**

A UNION in SQL terms merely concatenates congruent data sets, meaning they have the same type and number of columns. The difference between them is that UNION ALL is a simple concatenation and should be preferred wherever possible whereas a UNION will implicitly do a DISTINCT to remove duplicate tuples.

UNIONs, like DISTINCT, have their place. There are valid applications. But if you find yourself doing a lot of them, particularly in subqueries, then you're probably doing something wrong. That might be a case of poor query construction or a poorly designed data model forcing you to do such things.

UNIONs, particularly when used in joins or dependent subqueries, can cripple a database. Try to avoid them whenever possible.

**14. Using OR conditions in queries**

This might seem harmless. After all, ANDs are OK. OR should be OK too right? Wrong. Basically an AND condition **restricts** the data set whereas an OR condition **grows** it but not in a way that lends itself to optimisation. Particularly when the different OR conditions might intersect thus forcing the optimizer to effectively to a DISTINCT operation on the result.

Bad:

... WHERE a = 2 OR a = 5 OR a = 11

Better:

... WHERE a IN (2, 5, 11)

Now your SQL optimizer may effectively turn the first query into the second. But it might not. Just don't do it.

**15. Not designing their data model to lend itself to high-performing solutions**

This is a hard point to quantify. It is typically observed by its effect. If you find yourself writing gnarly queries for relatively simple tasks or that queries for finding out relatively straightforward information are not efficient, then you probably have a poor data model.

In some ways this point summarizes all the earlier ones but it's more of a cautionary tale that doing things like query optimisation is often done first when it should be done second. First and foremost you should ensure you have a good data model before trying to optimize the performance. As Knuth said:

> Premature optimization is the root of all evil

**16. Incorrect use of Database Transactions**

All data changes for a specific process should be atomic. I.e. If the operation succeeds, it does so fully. If it fails, the data is left unchanged. - There should be no possibility of 'half-done' changes.

Ideally, the simplest way to achieve this is that the entire system design should strive to support all data changes through single INSERT/UPDATE/DELETE statements. In this case, no special transaction handling is needed, as your database engine should do so automatically.

However, if any processes do require multiple statements be performed as a unit to keep the data in a consistent state, then appropriate Transaction Control is necessary.

- Begin a Transaction before the first statement.
- Commit the Transaction after the last statement.
- On any error, Rollback the Transaction. And very NB! Don't forget to skip/abort all statements that follow after the error.

Also recommended to pay careful attention to the subtelties of how your database connectivity layer, and database engine interact in this regard. 

**17. Not understanding the 'set-based' paradigm**

The SQL language follows a specific paradigm suited to specific kinds of problems. Various vendor-specific extensions notwithstanding, the language struggles to deal with problems that are trivial in langues like Java, C#, Delphi etc.

This lack of understanding manifests itself in a few ways.

- Inappropriately imposing too much procedural or imperative logic on the databse.
- Inappropriate or excessive use of cursors. Especially when a single query would suffice.
- Incorrectly assuming that triggers fire once per row affected in multi-row updates.

Determine clear division of responsibility, and strive to use the appropriate tool to solve each problem.