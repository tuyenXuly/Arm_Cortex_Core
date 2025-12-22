# What is a task ?
- Một task thực chất **là một đoạn code, hoặc có thể gọi nó là một hàm (ngôn ngữ 'C')**, thực hiện một công việc cụ thể khi nó được phép chạy trên CPU.

- Một task có vùng ngăn xếp (Stack) riêng để tạo các biến cục bộ khi nó thực thi trên CPU. Ngoài ra, khi bộ lập lịch quyết định đưa một task ra khỏi CPU, trước tiên nó sẽ lưu lại ngữ cảnh (Context/State) của tác vụ đó vào vùng ngăn xếp riêng (private stack) của tác vụ.

=> Tóm lại, một đoạn code hoặc một hàm được gọi là một task khi nó có khả năng được lập lịch (schedulable) và không bao giờ bị mất đi "trạng thái" của mình trừ khi nó bị xóa vĩnh viễn.
# PSP and MSP in task scheduler
![](psp_and_msp.png)
- MSP (Main Stack Pointer): Luôn được sử dụng sau khi reset và trong các trình phục vụ ngắt (Handler Mode). Scheduler của chúng ta nằm trong ngắt SysTick nên dùng MSP.

- PSP (Process Stack Pointer): Được dùng bởi các tiến trình người dùng (Thread Mode). Việc tách biệt này giúp ngăn lỗi tràn stack của một ứng dụng làm hỏng vùng nhớ của hệ điều hành.
# Stack Assessment
- Trong lập trình C thông thường, hiếm khi phải lo lắng về Stack vì trình biên dịch và Startup code đã làm hết. Nhưng khi viết Scheduler, ta sẽ phải đóng vai trò là "người quản lý tài nguyên".
![](stack_assessment.png)
- Mỗi task có một stack riêng nằm cố định trong RAM và stack này sẽ không thay đổi trong quá trình chạy trừ khi task bị xóa. Khi xảy ra ngắt SysTick, CPU lưu context của task hiện tại vào stack của task thông qua PSP, sau đó chuyển sang Handler mode và sử dụng MSP để chạy scheduler trên stack riêng của kernel(private stack scheduler). Scheduler chọn task tiếp theo, nạp lại PSP để trỏ vào stack của task đó, rồi CPU quay về Thread mode để tiếp tục thực thi task.
- Tham khảo thêm: https://github.com/tuyenXuly/FreeRTOS_with_STM32_tutorials/blob/master/Part%2011%20Context%20Switching/README.md