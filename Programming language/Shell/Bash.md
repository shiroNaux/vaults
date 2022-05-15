#### Tips & tricks

- Tạo file trong mỗi folder con
  
  ```
  find . -type d -exec touch {}/__init__.py \;
  ```
  Câu trên sẽ tạo file ____init____.py trong mỗi thư mục con trong folder hiện tại