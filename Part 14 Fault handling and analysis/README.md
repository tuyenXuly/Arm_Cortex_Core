# ARM cortex M3/M4 fault handler
- Fault là một loại exception (ngoại lệ) do bộ xử lý sinh ra để báo hiệu có lỗi.
Fault thực chất chính là system exception (ngoại lệ hệ thống).
- Các exception liên quan đến lỗi (fault exception) là: **HardFault**, **MemManage Fault**, **BusFault**, **UsageFault**. Chúng được gọi là fault exceptions, và sẽ được kích hoạt bất cứ khi nào có lỗi trong bộ xử lý.
- **HardFault Exception**
    - HardFault exception là một loại exception đặc biệt. Nó luôn được **bật mặc định** trong vi xử lý, bạn không cần phải bật thủ công.
    - Độ ưu tiên (priority) của HardFault là không thể thay đổi là -1
- **UsageFault, MemManage Fault, BusFault**
    - Luôn tắt mặc định
    - Có priority có thể cấu hình được. Nếu bạn không cấu hình, chúng sẽ có priority mặc định là 0.
## HardFault Exception
- HardFault là một exception xảy ra do **có lỗi trong quá trình xử lý exception**,
hoặc vì **lỗi đó không thể được quản lý bởi bất kỳ cơ chế exception nào khác**.
- Ví dụ: Giả sử chương trình chia một số cho 0. Chia cho 0 thực chất thuộc về UsageFault exception. Nếu UsageFault được bật, thì handler của nó sẽ chạy. Nhưng mặc định UsageFault bị tắt, nên lỗi chia cho 0 sẽ bị leo thang thành HardFault → CPU gọi HardFault handler.
- Một số nguyên nhân cụ thể khác còn có như:
    - Bus error trong quá trình vector fetch từ vector table
    - Thực thi lệnh breakpoint khi halt mode và debug monitor đều bị tắt.
    - Thực thi lệnh SVC trong SVC handler cũng sẽ kích hoạt HardFault.
    - Nếu các configurable exception không được bật, mà có lỗi thuộc về chúng xảy ra, thì lỗi đó cũng sẽ được leo thang thành HardFault exception.
- **HardFault Status Register**
    - Có một thanh ghi đặc biệt gọi là HardFault Status Register.Thanh ghi này có 4 bit hợp lệ:
        - Bit 0: Reserved.
        - Bit 1 (VECTTBL): Nếu bit này được set → HardFault xảy ra do lỗi khi đọc vector table.
        - Bit 30 (FORCED): Nếu bit này được set → HardFault là do escalation (leo thang) từ một configurable fault.
        - Bit 31 (DEBUGEVT): Nếu bit này được set → HardFault được kích hoạt do sự kiện debug.
