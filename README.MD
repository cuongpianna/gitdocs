# Tài liệu git

### Git cơ bản
  
 ##### Clone 1 project
 
    `git clone https://github.com/user/repository.git`
 
 ##### Khởi tạo 1 repo:
 
    `git init`

##### Nhánh trong git
Khi sử dụng Git, ta có thể tạo ra nhiều nhánh (branch) khác nhau. Câu lệnh Git này dùng để kiểm tra branch hiện tại:

    `git branch`

Để tạo mới một branch:

    `git branch <name_branch>`

Để chuyển và tạo mới:

    `git branch -b <name_branch>`

Chuyển nhánh:

    `git checkout -n <name_branch>`
    
##### Cập nhật thay đổi
Sau khi thay đổi source code: thêm mới, sửa, xoá files,… Cần phải cập nhật lên Staging Area. Để cập nhật hết các files:

`git add .`

Sau lệnh add, ta cần sử dụng câu lệnh commit để ghi lại những thay đổi:

`git commit -m "Message"`

##### Cập nhật remote repo (Github)

Sau câu lệnh Commit, thông tin mới chỉ được cập nhật lên Local Repository. Nếu muốn cập nhật lên server thì phải sử dụng câu lệnh push:

`git push origin <name_branch>`

Note: origin là tên của remote repo. Để them 1 remote repo:

`git remote add origin <remote_url>`

##### Pull từ remote repository

`##### Pull từ remote repository`

### Quy trình làm việc với Git, Github

Thông thường 1 dự án được chia làm 2 nhánh chính:

* master
* dev

Nhánh master sẽ là nơi chứa phần code ổn định nhất, sẵn sàng để deploy bất cứ lúc nào. 
Trong khi đó, nhánh dev ban đầu được tách ra từ master, và sẽ chứa phần code mới nhất được phát triển.

##### Phân chia công việc

Mỗi khi phát triển tính năng mới, bạn sẽ tạo một nhánh từ dev.

`git checkout -b login dev`  (tạo nhánh login từ dev)

Note: Commit thường  xuyên với những thay đổi nhỏ nhất

##### Chuẩn bị merge vào dev

----
Sau khi code hoàn tất và tất cả unit tests đã chạy thành công, giờ là lúc merge/gửi code để review tính năng mới vào dev. Thông thường, sẽ có 2 trường hợp xảy ra:

**Trường hợp 1: Không có gì mới trong dev**

Giả sử Git history sẽ như thế này:

![alt](http://document.azthemes.net/images/rental/git%601.PNG)

Nhánh login màu vàng được rẽ ra từ nhánh dev màu xanh, và trong nhánh dev không có code gì mới. Đây là trường hợp lý tưởng, đảm bảo khi merge vào dev chúng ta sẽ không bị xung đột code

**Trường hợp 2: Có commits mới trong nhánh dev**

![alt](http://document.azthemes.net/images/rental/git2.PNG)

Trong trường hợp này, branch dev (màu xanh) đang có 2 commits phía trước branch login (màu vàng). Nếu trong 2 commits đó có chứa thay đổi liên quan đến dev, chẳng hạn như package.json, thì khả năng cao là sẽ xảy ra xung đột khi merge trực tiếp login vào. Mà dù có may mắn không xảy ra xung đột code, thì merge vào cũng sẽ làm history xấu đi.

Do đó, chúng ta sẽ cần sửa lại history của nhánh login bằng cách dùng git rebase

**git rebase là gì?**

git rebase sẽ đem những commits bên trong nhánh login và áp dụng lại vào sau commit mới nhất trong nhánh dev. Cú pháp của lệnh này là:

`git rebase <tên nhánh muốn áp dụng lại>`

Trong trường hợp hiện tại, chúng ta sẽ chạy những lệnh sau:

`# Cập nhật repo hiện tại, đồng thời lấy về commits mới nhất của dev`
`git pull origin dev`

`git checkout login`

`git rebase dev`


Nếu xảy ra xung đột code, có thể phát hiện và giải quyết chúng sớm. Nguyên tắc chung là không sửa code của người khác, và chỉ kết hợp thêm những gì mình làm. Việc thực hiện rebase tại nhánh chức năng do mình phụ trách giúp giảm thiểu khả năng mất code, vì mình là người hiểu rõ nhất code của mình.

Sau khi giải quyết hết các xung đột trong code, ta chạy git rebase --continue để tiếp tục tiến trình rebase. Bạn cũng có thể chạy git rebase --abort để hủy bỏ rebase và đưa nhánh login về lại trạng thái ban đầu

Sau khi rebase xong, history sẽ thành:

![alt](http://document.azthemes.net/images/rental/git3.PNG)

Note: có nhiều cách để giải quyết conflict, đây là cách cơ bản nhất
