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
  - **behavior**/method/algorithm và logic .    
  -> Giúp tổ chức các thành phần trong chương trình,   
    ví dụ chương trình quản lý xe trong cửa hàng được tổ chức thành class Car, Bill...
 
- Object phản ánh một _thực thế cụ thể_ từ class, đóng gói 1 instance trong một _ngữ nghĩa cụ thể_.  
Được truy xuất thông qua biến, với biến là nơi lưu trữ tham chiếu đến địa chỉ của object được lưu trong vùng nhớ heap.  
VD: Car redCar = new Car("red", "2015");  
- **Code** là một tập hợp lệnh hiện thực: 
  - Behavior của object (của class) 
  - Tương tác giữa các object

##### Kết luận:
- Object chứa code
- Code chứa kịch bản/ logic hiện thực behavior của object

#### Code và thread
- **Thread** là **đơn vị thực thi** nhỏ nhất trong một bộ xử lý (process), mỗi chương trình (program) chạy trên một process độc lập.
- Code được viết bằng ngôn ngữ lập trình (vd: java, c+, python...)   
    * Chứa tập lệnh thực thi một nhiệm vụ (function) bằng logic và algorithm:
      * Cái con người hiểu được
      * Cái máy tính có thể thực thi   
- Ngoài ra, tập lệnh trong code có thể định nghĩa cách quản lý và hành vi của thread: create, delete, stop, start, tương tác.

##### Kết luận:

- Code đóng vai trò trung gian cho giao tiếp giữa người và bộ xử lý máy tính, phản ảnh ở mức cao tập lệnh hay kịch bản con người muốn máy làm:  
  - Code chạy trên thread
  - Thread chạy code để thực thi một nhiệm vụ cụ thể

#### Thread và object
- Nhưng thread muốn chạy thì code là không đủ vì muốn chạy code:
  - Không chỉ cần tập lệnh / kịch bản của code
  - Mà còn cần input/data/đối tượng chịu tác động/ đối tượng phản ánh thay đổi/ ngữ nghĩa để tương tác với máy

##### Kết luận:

  - Object chính là phần trung gian đó:  
    - Thread muốn chạy code thì cần truyền code vào thread nhưng code không đứng độc lập, 
    code cần ngữ nghĩa, code cần là hành vi của một object cụ thể, code cần input cho hành vi,
    cần nơi lưu trữ thay đổi, code cần nằm trong một object   
  -> _Thread thực thi code thông qua object_   
- Nếu đặt object trong multiple threads, thì object còn đóng vai trò lưu trữ dữ liệu cái được chia sử giữa các thread   
-> Object giúp threads chia sẻ tài nguyên   

### Tính ứng dụng
Vậy góc nhìn về thread-code-object mang lại cho ta lợi ích gì:  
- Hiểu hơn về phần nào làm nhiều vụ nào để dùng phù hợp: 
  - Thread là máy thực thi lệnh, 
  - Code định nghĩa tập lệnh, 
  - Object là ngữ nghĩa (semantic) phản ánh một đối tượng được trừu tượng hóa và đưa vào trong chương trình để xử lý, đóng gói một dữ liệu đặc trưng và mô phỏng hành vi của đối tượng 
- Hiểu hơn mối liên hệ giữa các phần để có một cái nhìn rõ hơn về cách chương trình hoạt động -> xác định những nguyên nhân có thể gây ra lỗi
- Hiểu hơn về tính reusable trong các tính huống khác nhau  

##### Giả sử: trong trường hợp nhiều threads chạy, ta khả năng thực hiện song song, đồng thời nhiều nhiệm vụ cùng lúc nhằm tăng hiệu suất chương trình

1. 2 threads chạy 2 objects khác nhau -> tương tác trên hai ngữ nghĩa khác nhau -> tạo kết quả độc lập cho từng object    
=> ta thấy code được tái sử dụng 2 lần
2. 2 threads chạy song song nhưng 2 lần chạy cần tác động chung trên một object (ngữ nghĩa)  
Phải đảm bảo kết quả đúng, là luôn ra cùng một kết quả có thể đoán định cho mọi lần chạy (consistent)  
Ta cần đồng bộ (synchronization mechanism)   
Ví dụ, ta có list 10 học sinh cần lưu vào một object Cource, trước khi lưu cần chuyển đổi một vài data   
Ta tách list trên làm 2 nhóm cho 2 thread xử lý đồng thời, giảm thời gian thực thi của phần convert data này xuống một nửa  
Khi cập nhật vào list, nếu để thread chạy tự nhiên thì không biết được thứ tự sinh viên bị thay đổi như nào, mà ta muốn giữ thứ tự học sinh theo nhóm,  
Vậy ta khóa list (synchronize) trong Cource để list này được thực hiện lần lượt  

##### Dùng nhiều threads
- Giúp tăng tốc độ phản hồi của chương trình   
VD: một máy chỉ xử lý được một require của một user,   
Giờ có 2 threads, giống như có 2 máy, bạn có thể xử lý cho 2 users cùng lúc  
Giảm bớt thời gian chờ của user

- Có 2 trường hợp: 
  - 2 nhiệm vụ không liên quan gì đến nhau như 2 users khác nhau cập nhật 2 mật khẩu độc lập
  - Nhưng nếu 2 nhiệm vụ liên quan hay ta cần 2 threads tương tác với nhau thì cần phải làm sao:
    - Cùng cập nhật trên một shared object -> đảm bảo task 1 xong là complete thì task 2 mới chạy trên the shared object hoặc ngược lại  
Đảm bảo tính toàn vẹn của một nhiệm vụ không bị thay đổi
    - Phải đảm bảo kết quả mỗi lần đến như nhau
    - Ví dụ: cập nhật tài khoản người chuyển và người nhận phải cùng được cập nhật thành công ở 2 object tài khoản khác nhau.  
  Ta dùng synchronize cho 2 objects -> nhưng sẽ tốn thời gian và tài nguyên chỗ này.


### Ứng dụng trong swing:
- Với những phần tương tác với giao diện ta phải đảm bảo tính phản hồi trên giao diện mượt mà.
- Swing GUI cần chạy trên 1 UI thread duy nhất (Event dispatcher thread) để:
  - Thay đổi được cập nhật trên nhiều giao diện đồng nhất
  - Luôn đảm bảo kết quả đoán định được, đúng mọi lúc
- Vậy làm sao mà chạy mượt nếu chỉ có 1 thread mà không bị đứng (hang: không phản hổi, chờ)


  
