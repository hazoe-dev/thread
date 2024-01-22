# Thread
### Tổng quan 
- Nhận thức về thread tạo ra một bức tranh rõ ràng về cách một chương trình hoạt động.
- Thread, code và object có mối quan hệ chặt chẽ với nhau.
  ![overview](images/OverviewRelationship.svg)

### Mối quan hệ giữa thread - code - object
Cụ thể:

#### Object và code  
- **Object** là một thể hiện (instance) của một class
- **Class** là một bản thế kế.  
Trong OOP, class đóng gói của một loại thực thể trong thực tế vào chương trình gồm những:
  - **data**/properties/đặc trưng 
  - **behavior**/method/algorithm .    
  -> Giúp tổ chức các thành phần trong chương trình, ví dụ chương trình này có class Car, Bill...
  Ví dụ: class Car  
- Object phản ánh một _thực thế cụ thể_ từ class, cái đóng gói trong một _ngữ nghĩa cụ thể_.  
Được truy xuất thông qua biến, cái lưu trữ tham chiếu đến địa chỉ của object được lưu trong vùng nhớ heap.  
VD: Car redCar = new Car("red", "2015");  
- **Code** là một tập hợp lệnh hiện thực: 
  - Behavior của object (của class) 
  - Tương tác giữa các object

=> Kết luận:
- Object chứa code
- Code chứa kịch bản/ logic hiện thực behavior của object

#### Code và thread
- **Thread** là **đơn vị thực thi** nhỏ nhất trong một bộ xử lý (process), mỗi chương trình (program) chạy trên một process độc lập.
- Code được viết bằng ngôn ngữ lập trình (vd: java, c+, python...)   
    * Chứa tập lệnh thực thi một nhiệm vụ (function) bằng logic và algorithm:
      * Cái con người hiểu được
      * Cái máy tính có thể thực thi   
- Ngoài ra, tập lệnh trong code có thể định nghĩa cách quản lý và hành vi của thread: create, delete, stop, start, tương tác.
- => Kết luận:
- Code đóng vai trò trung gian cho giao tiếp giữa người và bộ xử lý máy tính, phản ảnh ở mức cao kịch bản con người muốn máy làm:  
  - Code chạy trên thread
  - Thread chạy code để thực thi


#### Thread và object
- Nhưng thread muốn chạy có thì code là không đủ vì muốn chạy code:
  - Cần kịch bản code
  - Cần input/data/đối tượng chịu tác động/ đối tượng phản ánh thay đổi
- Kết luận:
  - Object chính là phần trung gian đó:  
    - Thread muốn chạy code thì cần truyền code vào thread nhưng code không đứng độc lập, 
    code cần ngữ nghĩa, code cần là hành vi của một object cụ thể, code cần input cho hành vi,
    cần nơi lưu trữ thay đổi, code cần nằm trong một object   
  -> Thread thực thi code thông qua object   
- Nếu đặt object trong multiple threads, thì object còn đóng vai trò lưu trữ dữ liệu cái được chia sử giữa các thread   
-> Object giúp threads chia sẻ tài nguyên   

### Tính ứng dụng
Vậy góc nhìn về thread-code-object mang lại cho ta lợi ích gì:  
- Nếu bạn muốn hiện thực chức năng phần nào ta có thể chỉ quan tâm phần đó  
- Hiểu hơn mối liên hệ giữa các phần để có một cái nhìn rõ hơn về cách chương trình hoạt động -> xác định những nguyên nhân có thể gây ra lỗi
- Hiểu hơn về tính reusable trong các tính huống khác nhau  

##### Giả sử: trong trường hợp nhiều threads chạy để cho ta khả năng chạy song song, đồng thời nhằm tăng hiệu suất chương trình

- 2 threads chạy 2 objects khác nhau -> tương tác trên hai ngữ nghĩa khác nhau -> tạo kết quả độc lập cho từng object  
Nhưng ta thấy code được tái sử dụng 2 lần
- 2 threads chạy song song nhưng 2 lần chạy cần tác động chung kết quả trên một object (ngữ nghĩa)  
Phải đảm bảo kết quả đúng, là luôn ra cùng một kết quả có thể đoán định  
Ta cần đồng bộ (synchronization mechanism)    
- Ví dụ, ta có list 10 học sinh cần lưu vào một object Cource trước khi lưu cần chuyển đổi một vài data   
Ta tách list trên làm 2 nhóm cho 2 thread xử lý đồng thời, giảm thời gian thực thi của phần này xuống một nửa  
Nhưng phần cập nhật list không biết được thứ tự thực hiện mà ta muốn giữ thứ tự học sinh theo nhóm,  
Vậy ta khóa list trong Cource để list này được thực hiện lần lượt

  
