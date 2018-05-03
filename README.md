# find-linux

Sử dụng lệnh find trong linux

**Các lệnh ```find``` cơ bản:**

```
# câu lệnh đầy đủ
find / -name test.txt -type f -print             
 # -print là không cần thiết
find / -name test.txt -type f                   
# không có chỉ định "type==file"
find / -name test.txt                            
# tìm kiếm dưới thư mục hiện tại
find . -name foo.txt                            
# sử dụng ký tự đại diện. Ý nghĩa: tìm với tên là "test" với tất cả các đuôi file
find . -name "test.*"                            
# sử dụng ký tự đại diện. Ý nghĩa: tìm tất cả các file với đuôi txt
find . -name "*.txt"      
# tìm kiếm trên thư mục /users/al các thư mục có tên "book".                      
find /users/al -name book -type d
# Các type của tìm kiếm
d : (directory) thư mục
f : (file) tệp
```

**Tìm kiếm trên nhiều thư mục**

```
# tìm kiếm trên nhiều thư mục.
find /opt /usr /var -name foo.scala -type f     
```

**Tìm kiếm phân biệt chữ hoa, thường**

```
# tìm kiếm ra các file, thư mục như foo, Foo, FOo, FOO,vv...
find . -iname foo  
# tương tự như lệnh trên nhưng chỉ tìm các thư mục                             
find . -iname foo -type d   
# tương tự như lệnh trên nhưng chỉ tìm kiếm các tệp tin                     
find . -iname foo -type f                       
```

**Tìm kiếm các tệp tin với phần mở rộng khác nhau**

```
# tìm kiếm các file có phần mở rộng là ".c" và ".sh"
find . -type f \( -name "*.c" -o -name "*.sh" \)   
# tìm kiếm các file với 3 phần mở rông khác nhau                    
find . -type f \( -name "*cache" -o -name "*xml" -o -name "*html" \)   
```

**Tìm kiếm không giống với mẫu**

Sử dụng mệnh đề ``-not`` để tìm kiếm không giống với mẫu
```
# Tìm kiếm tất cả các file không có phần mở rộng là ".html"
find . -type f -not -name "*.html"                                

```

**Tìm kiếm các tệp với nội dung bên trong têp (find + grep)**

Để kết hợp lệnh find với các hoạt động khác sử dụng tùy chọn ```-exec```
```
# tìm kiếm các chuỗi StringBuffer trong tất cả các tệp có phần mở rộng là "*.java"
find . -type f -name "*.java" -exec grep -l StringBuffer {} \;    
# bỏ qua trường hợp với tùy chọn
find . -type f -name "*.java" -exec grep -il string {} \;         
# tìm kiếm một chuỗi trong các tệp gzip
find . -type f -name "*.gz" -exec zgrep 'GET /foo' {} \;          
```

**Tìm kiếm tệp và hoạt động trên tệp đó (find + exec)**

```
# tìm kiếm các tệp có phần mở rộng là ".html" và thay đổi quyền với các tệp đó thành 644
find /usr/local -name "*.html" -type f -exec chmod 644 {} \;    
# tìm kiếm các tệp có phần mở rộng là ".cgi" và thay đổi quyền với các tệp đó thành  755    
find htdocs cgi-bin -name "*.cgi" -type f -exec chmod 755 {} \;   
# chạy lệnh ls với các tệp có phần mở rộng là ".pl" tìm được  
find . -name "*.pl" -exec ls -ld {} \;                            
```

**Tìm kiếm và copy**

```
# tìm kiếm các tệp có phần mở rộng là ".mp3" và copy tới thư mục /tmp/MusicFiles
find . -type f -name "*.mp3" -exec cp {} /tmp/MusicFiles \;       
```

**Tìm kiếm và xóa**

```
# xóa tất cả các file có tên "Foo*" ở thư mục hiện tại
find . -type f -name "Foo*" -exec rm {} \;
# xóa tất cả các thư mục con trong thư mục hiện tại có tên "CVS"                       
find . -type d -name CVS -exec rm -r {} \;                        
```

**Tìm tệp với thời gian sửa đổi**

Để tìm với thời gian sửa đổi thì thay vì dùng tùy chọn ```-type``` thì sử dụng tùy chọn ```-mtime```
```
find . -mtime 1               # 24 giờ
find . -mtime -7              # 7 ngày qua
find . -mtime -7 -type f      # chỉ tìm các tệp
find . -mtime -7 -type d      # chỉ tìm các thư mục
```

**Tìm kiếm và tar**

```
# Tìm kiếm các tệp java và sử dụng lệnh tar lưu trữ thành tệp myfile.tar mới
find . -type f -name "*.java" | xargs tar cvf myfile.tar
# Tìm kiếm các tệp java và sử dụng lệnh tar lưu trữ thành tệp myfile.tar bằng cách nối tiếp vào file cũ
find . -type f -name "*.java" | xargs tar rvf myfile.tar
```
