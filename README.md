
# A. Tổng quan về Git
## I. Git là gì?
**Git** là một hệ thống điều khiển **phiên bản phân tán** (Distributed Version Control System - DVCS) ghi lại các thay đổi đối với một tệp hoặc tập hợp các tệp theo thời gian, cho phép bạn dễ dàng xem lại hoặc khôi phục các phiên bản cụ thể sau này.

## II. Ba trạng thái tệp chính:
- `Committed (Đã cam kết)`: Phiên bản của tệp đã được lưu trữ vĩnh viễn trong thư mục Git (.git directory)
- `Staged (Đã dàn dựng)`: Tệp đã được sửa đổi và được đánh dấu để đưa vào commit tiếp theo, nằm trong khu vực dàn dựng (staging area)
- `Modified (Đã sửa đổi)`: Tệp đã thay đổi kể từ lần cuối được kiểm tra nhưng chưa được dàn dựng

---

# B. Lệnh git
## I. Thiết lập ban đầu và Cấu hình:
### 1. Cấu hình
- Lệnh: **git config** 
    ```bash
    git config [--global | --system | --local] <key> <value> 
    ```
- `--local`: (mặc định) áp dụng cho repo hiện tại. Lưu trong .git/config.
- `--global`: áp dụng cho tất cả repo của người dùng. Lưu trong ~/.gitconfig`.
- `--system`: áp dụng cho toàn bộ hệ thống. Lưu trong /etc/gitconfig (cần quyền admin).
- 🧪 **VD:**
    ```bash
    git config --global user.name "Your Name"
    git config --global user.email "your.email@example.com"
   ``` 
  - **Lệnh hữu ích:**
      ```bash
      git config --list                       # Xem tất cả config hiện tại
      git config user.name                    # Xem một config cụ thể
      git config --unset user.name            # Xóa một cấu hình
      git config --list --show-origin         # Hiển thị tất cả cài đặt và nơi chúng được định nghĩa
     ```
      
## II. Lấy hoặc Tạo dự án      
### 1. Tạo git
- Lệnh: **git init**
    ```bash
    git init
    ```
   
### 2. **Lấy dự án:**
- Lệnh: **git clone**
   ```bash
   git clone [options] <repo> [<directory>]
    ```
- `<repo>`: Địa chỉ URL của repository (HTTPS, SSH, Git, v.v.)
- `<directory>` (tùy chọn): Tên thư mục để lưu repo. Nếu không có, Git sẽ dùng tên repo.
  - **VD:** 
      ```bash
      git clone https://github.com/user/project.git my-folder
      ```
    
  | Option                             | Ý nghĩa                                         |
  |------------------------------------|-------------------------------------------------|
  | `--branch <name>` hoặc `-b <name>` | Chỉ clone nhánh cụ thể                          |
  | `--depth <n>`                      | **Shallow clone** – chỉ lấy `n` commit mới nhất |
  | `--single-branch`                  | Chỉ lấy nhánh hiện tại (giảm dung lượng)        |
  | `--recurse-submodules`             | Clone luôn các submodules (nếu có)              |
  | `--origin <name>`                  | Đặt tên khác cho remote (mặc định là `origin`)  |

  
## III. Thay đổi và quản lý tệp
### 1. Thêm Thay Đổi:
- Lệnh: **Git add**
     ```bash
    git add <file>
    git add <folder>/
    git add .                          # Thêm tất cả thay đổi hiện tại
    ```  
- 🧪**VD**:
    ```bash
    git add index.html                 # Thêm file cụ thể
    git add src/                       # Thêm cả thư mục
    git add .                          # Thêm tất cả thay đổi (trong thư mục hiện tại)
    git add -A                         # Thêm tất cả: thêm, sửa, xóa (giống `.`)
    git add -u                         # Thêm các file đã theo dõi (bỏ qua file mới)
    ``` 
### 2. Xem trạng thái
- Lệnh: **git status**
    ```bash
   git status [options]
    ```   
- Các tuỳ chọn phổ biến (`[options]`):  
  - `-s` hoặc `--short`: Cung cấp đầu ra trạng thái tóm tắt, ngắn gọn hơn

### 3. Ghi lại các thay đổi đã dàn dựng
- Lệnh: **Git commit**
    ```bash
    git commit [options] 
    ```
- Các tuỳ chọn phổ biến (`[options]`):

  | Tuỳ chọn                 | Ý nghĩa                                                                               |
  |--------------------------|---------------------------------------------------------------------------------------|
  | `-m "message"`           | Thêm thông điệp commit trực tiếp                                                      |
  | `-a`                     | Tự động `git add` tất cả file đã được Git theo dõi (tracked), rồi commit              |
  | `--amend`                | Sửa lại commit gần nhất (rất hữu ích khi bạn quên thêm file hoặc viết sai thông điệp) |
  | `--no-edit`              | Dùng lại thông điệp cũ khi sửa commit (`--amend`)                                     |
  | `--allow-empty`          | Tạo commit dù không có thay đổi gì                                                    |
  | `-v`                     | Hiện nội dung diff khi commit                                                         |
  | `--author="Tên <email>"` | Ghi đè tác giả của commit                                                             |
- 🧪**VD:**
  ```bash
    git commit -am "Commit tất cả file đã chỉnh sửa/tracked (không cần git add)"
    git commit --amend --no-edit  # Sửa lại commit nhưng không đổi message
    ``` 
### 4. Hiển thị những thay đổi chưa dàn dựng
- Lệnh: **Git diff**
     ```bash
    git diff
    ```     
  | Lệnh                                | So sánh giữa                     | Ý nghĩa                                         |
  |-------------------------------------|----------------------------------|-------------------------------------------------|
  | `git diff`                          | Working Directory ↔ Staging Area | Những thay đổi **chưa `git add`**               |
  | `git diff --staged` hoặc `--cached` | Staging Area ↔ Last Commit       | Những thay đổi **đã `add` nhưng chưa `commit`** |
  | `git diff <commit>`                 | <commit> ↔ Working Directory     | So sánh từ 1 commit đến hiện tại                |
  | `git diff <commit1> <commit2>`      | commit1 ↔ commit2                | So sánh giữa 2 commit                           |
  | `git diff <branch1> <branch2>`      | branch1 ↔ branch2                | So sánh giữa 2 nhánh                            |
  | `git diff HEAD`                     | Last Commit ↔ Working Directory  | Tất cả thay đổi từ lần commit gần nhất          |
- 🧪**VD:**
  ```bash
  git diff main develop             # So sánh 2 nhánh
  git diff 1a2b3c 4d5e6f            # So sánh 2 commit
  git diff --staged                 # Xem thay đổi đã add
  ```
### 5. Xóa tệp khỏi Git:
- Lệnh: **Git rm**
     ```bash
    git rm [options] <file>
    ```   
 | Tuỳ chọn   | Ý nghĩa                                                               |
 |------------|:----------------------------------------------------------------------|
 | `-f`       | (force) Bắt buộc xóa nếu file đã chỉnh sửa (đã thay đổi chưa commit)  |
 | `--cached` | ❗Chỉ xóa khỏi Git (staging/index), **giữ lại file trên máy**          |
 | `-r`       | Xóa **thư mục** (recursive)                                           |
- 🧪 **VD:**
    ```bash
   git rm -r --cached .
   ``` 
### 6. Hoàn tác các thay đổi:
- Lệnh: **Git restore**
     ```bash
    git restore <file>
    ```
| Tuỳ chọn            | Ý nghĩa                                             |
|---------------------|-----------------------------------------------------|
| `<file>`            | Tên file cần khôi phục                              |
| `--staged`          | Bỏ file khỏi staging (chưa commit)                  |
| `--worktree`        | Khôi phục file trong thư mục làm việc               |
| `--source=<commit>` | Khôi phục từ một commit cụ thể (mặc định là `HEAD`) |
| `-s`                | Viết tắt của `--source`                             |

- 🧪**VD:**
  ```bash
  git restore index.html                                      # Khôi phục file về trạng thái commit gần nhất
  git restore --staged app.js                                 # Bỏ file ra khỏi staging
  git restore --source=HEAD --staged --worktree script.js     # Khôi phục cả file ở working directory và staging
  ```

## IV. Xem lịch sử dự án
- Lệnh: **Git log**
  ```bash
  git log
   ```
| Tuỳ chọn     | Ý nghĩa                                                 |
|--------------|---------------------------------------------------------|
| `--oneline`  | Hiển thị gọn mỗi commit trên 1 dòng                     |
| `--graph`    | Vẽ sơ đồ nhánh (rất hữu ích khi nhiều nhánh)            |
| `--decorate` | Hiện nhánh/tag tương ứng với commit                     |
| `--all`      | Hiện commit từ **tất cả các nhánh**                     |
| `-p`         | Hiện chi tiết diff (nội dung đã thay đổi trong commit)  |
| `--stat`     | Hiện thống kê file nào bị sửa, thêm, xóa bao nhiêu dòng |
- **VD:**
  ```bash
  git log --oneline
  git log --oneline --graph --all --decorate
  ```
  | Lọc theo  | Lệnh                           |
  |-----------|--------------------------------|
  | Tác giả   | `git log --author="tên"`       |
  | Từ ngày   | `git log --since="2024-01-01"` |
  | Đến ngày  | `git log --until="2024-12-31"` |
  | Theo file | `git log <file>`               |
-  🧪**VD:**
  ```bash
  git log --author="Name" --since="1 month ago" index.html
  ```

## V. Làm việc với các kho lưu trữ từ xa
### 1. Quản lý các kho lưu trữ từ xa:
- Lệnh: **git remote**
     ```bash
    git remote
    ```
  | Lệnh                                  | Ý nghĩa                              |
  |---------------------------------------|--------------------------------------|
  | `git remote`                          | Liệt kê tên các remote đã thiết lập  |
  | `git remote -v`                       | Hiển thị URL chi tiết của các remote |
  | `git remote add <name> <url>`         | Thêm remote mới                      |
  | `git remote remove <name>`            | Xoá remote                           |
  | `git remote rename <old> <new>`       | Đổi tên remote                       |
  | `git remote set-url <name> <new-url>` | Thay đổi URL của remote              |

- 🧪**VD:**
  ```bash
  git remote add origin https://github.com/username/project.git
  ```
### 2. Lấy dữ liệu mới nhất
- Lệnh **git fetch**: dùng để lấy dữ liệu mới nhất từ remote repository về máy, nhưng không tự động gộp (merge) vào nhánh hiện tại.
     ```bash 
    git fetch [<options>] [<remote> [<refspec>...]]
    ```
- `<remote>`: Tên remote (thường là origin)
- `<refspec>`: Chỉ định rõ nhánh hoặc tag nào cần fetch
- `<options>`: Các tuỳ chọn mở rộng để điều khiển hành vi fetch

  | Tuỳ chọn               | Ý nghĩa                                        |
  |------------------------|------------------------------------------------|
  | `--all`                | Fetch từ **tất cả remotes**                    |
  | `--prune`              | Xoá những nhánh remote không còn tồn tại nữa   |
  | `--dry-run`            | Mô phỏng fetch, **không thật sự thực hiện**    |
  | `--verbose` hoặc `-v`  | Hiển thị thông tin chi tiết khi fetch          |
  | `--depth=<n>`          | Shallow fetch – chỉ lấy `n` commit gần nhất    |
  | `--tags`               | Lấy tất cả tag từ remote                       |
  | `--recurse-submodules` | Fetch cả submodules nếu có                     |
  | `--update-head-ok`     | Cho phép fetch khi `HEAD` đang ở detached mode |
  | `--refmap=<refspec>`   | Tuỳ chỉnh cách ánh xạ các ref về local         |

- 🧪**VD:**
  ```bash
   git fetch              # Lấy tất cả thay đổi từ remote mặc định (`origin`) 
   git fetch origin       # Tương tự như trên                                 
   git fetch origin main  # Lấy chỉ nhánh `main` từ `origin`   
   git fetch origin dev   # Fetch riêng nhánh dev từ origin            
   git fetch --all        # Lấy dữ liệu từ **tất cả remote**                  
   git fetch --prune      # Xóa nhánh remote không còn tồn tại trên server    
  ```


### 3. Tải cà hợp nhất chúng vào nhánh hiện tại
- Lệnh **git pull**: Là sự kết hợp của git fetch và git merge
     ```bash
    #git pull origin main
    git pull [<options>] [<remote>] [<branch>]
    ```
  | Tuỳ chọn      | Ý nghĩa                                         |
  |---------------|-------------------------------------------------|
  | `--rebase`    | Thay vì merge, sẽ rebase lịch sử (gọn gàng hơn) |
  | `--no-commit` | Không commit tự động sau khi merge              |
  | `--verbose`   | Hiện thêm thông tin chi tiết                    |

### 4. Đẩy lên kho lưu trữ từ xa
- Lệnh: **git push**
     ```bash
    git push [<options>] [<remote> [<refspec>...]]
    ```
- `<remote>` : Tên remote (thường là origin)
- `<refspec>` : Chỉ định rõ nhánh hoặc tag nào cần push
- `<options>` : Các tuỳ chọn mở rộng để điều khiển hành vi push

  | Tuỳ chọn               | Mô tả                                                            |
  |------------------------|------------------------------------------------------------------|
  | `-u`, `--set-upstream` | Gắn nhánh local với nhánh remote (để lần sau chỉ cần `git push`) |
  | `--force` hoặc `-f`    | **Ghi đè** nhánh trên remote (cẩn thận, dễ mất dữ liệu)          |
  | `--force-with-lease`   | Force push an toàn hơn (chỉ ghi đè nếu remote chưa thay đổi)     |
  | `--all`                | Push tất cả các nhánh                                            |
  | `--tags`               | Push tất cả tag                                                  |
  | `--delete`             | Xoá nhánh hoặc tag trên remote                                   |
  | `--dry-run`            | Mô phỏng `push` nhưng không thực hiện thật                       |
  | `--no-verify`          | Bỏ qua hook kiểm tra khi push                                    |

- 🧪**VD:**
   ```bash
  git push origin main    # Push nhánh hiện tại lên origin
  git push                # Push mặc định (nhánh đang làm việc, lên remote mặc định)
  ```


## VI. Phân nhánh và Hợp nhất
### 1. Quản lý các nhánh
- Lệnh: **git branch**
     ```bash
   git branch [<options>] [<branch-name>] [<start-point>]
    ```
  | Cú pháp                           | Tác dụng                                  |
  |-----------------------------------|-------------------------------------------|
  | `git branch`                      | Hiển thị danh sách tất cả các nhánh local |
  | `git branch <tên-nhánh>`          | Tạo nhánh mới từ commit hiện tại          |
  | `git branch <tên-nhánh> <commit>` | Tạo nhánh mới từ commit chỉ định          |
  | `git branch -d <tên-nhánh>`       | Xoá nhánh (an toàn – nếu đã được merge)   |
  | `git branch -D <tên-nhánh>`       | Xoá nhánh (ép buộc – dù chưa merge)       |
  | `git branch -m <tên-mới>`         | Đổi tên nhánh hiện tại                    |
  | `git branch -m <cũ> <mới>`        | Đổi tên nhánh cụ thể                      |
  | `git branch -a`                   | Hiển thị cả nhánh local **và remote**     |
  | `git branch -r`                   | Hiển thị **chỉ nhánh remote**             |

  | Tuỳ chọn      | Mô tả                                       |
  |---------------|---------------------------------------------|
  | `--merged`    | Hiển thị các nhánh đã được merge            |
  | `--no-merged` | Hiển thị các nhánh **chưa** được merge      |
  | `-vv`         | Hiển thị chi tiết nhánh và commit cuối cùng |

- 🧪**VD:**
   ```bash
  git branch dev             # Tạo nhánh mới
  git checkout dev           # Chuyển sang nhánh
  # hoặc git checkout -b dev  (Tạo và chuyển sang nhánh mới, gộp 2 bước)
  ```
> [!NOTE]
> Dấu `*` trong `git branch` chỉ nhánh hiện tại.
> Bạn `không thể xoá nhánh đang đứng`. Phải `checkout` sang nhánh khác trước.
> Để làm việc với nhánh remote, bạn thường cần `git fetch` trước để cập nhật danh sách nhánh.

### 2. Chuyển nhánh và khôi phục file về trạng thái cũ
- Lệnh: **git checkout**
     ```bash
   git checkout [options] <branch_or_commit> [--] [<path>...]
    ```
  | Tuỳ chọn    | Mô tả                                                          |
  |-------------|----------------------------------------------------------------|
  | `-b <name>` | Tạo và chuyển sang nhánh mới                                   |
  | `-- <file>` | Phân biệt rõ bạn đang chỉ định file (khi trùng tên nhánh/file) |
  | `<commit>`  | Checkout commit cụ thể (sẽ vào trạng thái "detached HEAD")     |

- `Detached HEAD`: Khi bạn checkout một commit cụ thể (không phải nhánh), bạn sẽ rơi vào trạng thái: `HEAD is now in 'detached' state` 
  - Bạn vẫn có thể xem code, chạy thử, thậm chí commit, nhưng các commit không gắn vào nhánh nào, rất dễ mất nếu không tạo nhánh mới.

### 3. Chuyển nhánh
- Lệnh: **git switch**
     ```bash
   git switch [<options>] <branch>
    ```
  | Tuỳ chọn            | Mô tả                                        |
  |---------------------|----------------------------------------------|
  | `-c <name>`         | Tạo nhánh mới và chuyển sang                 |
  | `-C <name>`         | Tạo lại nhánh (nếu đã tồn tại thì ghi đè)    |
  | `--create <name>`   | Giống `-c` (tường minh hơn)                  |
  | `--detach`          | Chuyển sang chế độ detached HEAD             |
  | `--force` hoặc `-f` | Ép chuyển nhánh, bỏ qua thay đổi chưa commit |

- 🧪**VD:**
   ```bash
  git switch main             # Chuyển sang một nhánh đã có
  git switch -c dev          # Tạo nhánh mới và chuyển sang nhánh đó
  ```
  
### 4. Gộp nội dung của một nhánh vào nhánh hiện tại.
- Lệnh: **git merge**
     ```bash
   git merge [<options>] <branch>
    ```
- `<branch>`: Tên nhánh (hoặc commit) bạn muốn gộp vào nhánh hiện tại
- `<options>`: Tuỳ chọn điều khiển cách merge hoạt động

| Tuỳ chọn               | Ý nghĩa                                                                         |
|------------------------|---------------------------------------------------------------------------------|
| `--no-commit`          | Merge nhưng **không tạo commit tự động** (để bạn kiểm tra lại trước khi commit) |
| `--no-ff`              | Luôn tạo commit merge, kể cả khi có thể fast-forward                            |
| `--ff-only`            | Chỉ cho phép fast-forward, **báo lỗi nếu cần commit merge**                     |
| `--squash`             | Gộp tất cả commit thành **1 commit duy nhất**, nhưng **không tự commit**        |
| `--abort`              | **Hủy merge đang diễn ra** (nếu đang xung đột)                                  |
| `--continue`           | Tiếp tục merge sau khi đã xử lý conflict                                        |
| `--strategy=recursive` | Chọn chiến lược merge                                                           |
| `--edit`               | Cho phép sửa thông điệp commit khi merge                                        |

- 🧪**VD:**
   ```bash
  git merge dev              # Gộp nhánh `dev` vào nhánh hiện tại
  ```

### 5. Tái áp dụng commit
- Lệnh: **git rebase **:  Tái áp dụng các commit hiện tại lên một nền mới
     ```bash
    git rebase [<options>] [<upstream> [<branch>]]
    ```
- `<upstream>`: Nhánh (hoặc commit) mà bạn muốn rebase lên
- `<branch>`: Nhánh bạn muốn rebase (mặc định là nhánh hiện tại)
- `<options>`: Các tùy chọn điều khiển quá trình rebase

  | Tùy chọn              | Ý nghĩa                                          |
  |-----------------------|--------------------------------------------------|
  | `-i`, `--interactive` | Rebase tương tác (chỉnh sửa lịch sử)             |
  | `--onto <newbase>`    | Chỉ định điểm mới để rebase (nâng cao)           |
  | `--continue`          | Tiếp tục sau khi xử lý conflict                  |
  | `--abort`             | Hủy rebase nếu đang conflict                     |
  | `--skip`              | Bỏ qua 1 commit đang lỗi/conflict                |
  | `--keep-empty`        | Giữ lại commit trống khi rebase                  |
  | `--autosquash`        | Gộp commit tự động theo cú pháp `fixup!/squash!` |
- 🧪**VD:**
   ```bash
  # Rebase nhánh hiện tại lên main
  git checkout feature/login
  git rebase main
  
  git rebase main feature/cart     # Rebase một nhánh khác 
  git rebase -i HEAD~4             # Rebase tương tác (interactive) để chỉnh sửa commit
  ```


### 6. Chuyển commit vào nhánh hiện tại.
- Lệnh **git cherry-pick**: chọn và áp dụng một commit cụ thể từ nhánh khác vào nhánh hiện tại.
     ```bash
   git cherry-pick [<options>] <commit>
    ```
- `<commit>`: Mã commit (SHA) mà bạn muốn áp dụng vào nhánh hiện tại
- `<options>`: Tuỳ chọn điều khiển quá trình cherry-pick

| Tùy chọn      | Mô tả                                                      |
|---------------|------------------------------------------------------------|
| `-x`          | Thêm dòng “(cherry picked from …)” vào commit message      |
| `--edit`      | Mở editor để sửa lại message                               |
| `--no-commit` | Áp dụng thay đổi nhưng không tự commit (cho phép sửa tiếp) |
| `--continue`  | Tiếp tục cherry-pick sau khi giải quyết xung đột           |
| `--abort`     | Hủy cherry-pick khi có xung đột                            |
| `--skip`      | Bỏ qua commit đang lỗi                                     |

- **VD:**
  ```bash
  git cherry-pick a1b2c3d         # Áp dụng một commit vào nhánh hiện tại
  git cherry-pick a1b2c3d 4d5e6f7 # Áp dụng nhiều commit vào nhánh hiện tại
  git cherry-pick branch~2        # Áp dụng commit từ một nhánh khác
  git cherry-pick a1b2c3d^..8d9e10f11 # Áp dụng khoảng commit vào nhánh hiện tại
  git cherry-pick -m 1 --no-commit a1b2c3d # Nếu có pull request
  ```

### 7. Đảo ngược commit
- Lệnh **git revert**:  tạo ra một commit mới nhằm "đảo ngược" (undo) một commit trước đó, mà không làm thay đổi lịch sử Git.
    ```bash
    git revert [<options>] <commit>
    ```
- `<commit>`: Mã SHA (hoặc range commit) bạn muốn đảo ngược
- `<options>`: Các tùy chọn điều khiển quá trình đảo ngược commit

| Tuỳ chọn      | Ý nghĩa                                                                                 |
|---------------|-----------------------------------------------------------------------------------------|
| `--no-commit` | Áp dụng thay đổi nhưng **không tạo commit ngay** (để bạn kiểm tra hoặc chỉnh sửa trước) |
| `--no-edit`   | Dùng commit message mặc định, **không mở editor**                                       |
| `--edit`      | Mở trình soạn thảo để bạn viết lại commit message                                       |
| `--continue`  | Tiếp tục revert sau khi xử lý conflict                                                  |
| `--abort`     | Hủy revert đang thực hiện khi bị xung đột                                               |

- **VD:**
  ```bash
  git revert  a1b2c3d           # Đảo ngược một commit
  git revert  a1b2c3d 4d5e6f7   # Đảo ngược nhiều commit
  git revert HEAD~3..HEAD     # Đảo ngược một dải commit
  ```


| Tính năng                             | Mô tả                                                                                                                                                                                                                                                                                                                                                              | 
|:--------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| I. Thiết lập ban đầu và Cấu hình      | [`git config`](#1-cấu-hình)                                                                                                                                                                                                                                                                                                                                        | 
| II. Lấy hoặc Tạo dự án                | [`git init`](#1-Tạo-git)  [`git clone`](#2-Lấy-dự-án)                                                                                                                                                                                                                                                                                                              | 
| III. Thay đổi và quản lý tệp          | [`git add`](#1-Thêm-Thay-Đổi)  [`git status`](#2-Xem-trạng-thái)  [`git commit`](#3-Ghi-lại-các-thay-đổi-đã-dàn-dựng)  <br/>   [`git diff`](#4-Hiển-thị-những-thay-đổi-chưa-dàn-dựng)  [`git rm`](#5-Xóa-tệp-khỏi-Git)  [`git restore`](#6-Hoàn-tác-các-thay-đổi)                                                                                                  |
| IV. Xem lịch sử dự án                 | [`Git log`](#IV-Xem-lịch-sử-dự-án)                                                                                                                                                                                                                                                                                                                                 |
| V.Làm việc với các kho lưu trữ từ xat | [`git remote`](#1-Quản-lý-các-kho-lưu-trữ-từ-xa)   [`git fetch`](#2-Lấy-dữ-liệu-mới-nhất)  <br/>      [`git pull`](#3-Tải-cà-hợp-nhất-chúng-vào-nhánh-hiện-tại)   [`git push`](#4-Đẩy-lên-kho-lưu-trữ-từ-xa)                                                                                                                                                       |
| VI. Phân nhánh và Hợp nhất            | [` git branch`](#1-Quản-lý-các-nhánh)   [`git checkout`](#2-Chuyển-nhánh-và-khôi-phục-file-về-trạng-thái-cũ)  [`git switch`](#3-Chuyển-nhánh) <br/>      [`git merge`](#4-Gộp-nội-dung-của-một-nhánh-vào-nhánh-hiện-tại)   [`git rebase`](#5-Tái-áp-dụng-commit)     [`git cherry-pick`](#6-Chuyển-commit-vào-nhánh-hiện-tại)  [`git revest`](#7-Đảo-ngược-commit) |

<!--suppress ALL -->
<div align="center">

---
`END`
</div>
