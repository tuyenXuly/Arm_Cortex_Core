# Taget of this lesson
- Cài đặt chuỗi công cụ (toolchain)
- Hiểu quá trình biên dịch một chương trình C cho mục tiêu nhúng mà không sử dụng IDE
- Viết file khởi động (startup file) cho vi điều khiển STM32F4 MCU
- Viết mã khởi động bằng C của riêng bạn (đoạn mã chạy trước hàm main())
- Hiểu các phần (section) khác nhau của file đối tượng có thể tái định vị (các file .o)
- Viết file linker script từ đầu và hiểu cách bố trí (placement) các section
- Liên kết (link) nhiều file .o bằng linker script và tạo file thực thi của ứng dụng
( .elf, .bin, .hex )
- Nạp (load) file thực thi cuối cùng lên thiết bị đích bằng OpenOCD và GDB client

# Cross-Compilation - Biên dịch chéo
- Biên dịch chéo là một quá trình mà trong đó bộ công cụ biên dịch chéo (cross-toolchain) chạy trên máy chủ (host machine) và tạo ra tệp thực thi để chạy trên một máy khác (target machine). 
- Ví dụ trong trường hợp này, máy chủ là máy tính cá nhân, còn máy mục tiêu là board mạch vi điều khiển STM32.
- Để thực hiện Cross-Compilation, cần có một tập hợp các file nhị phân được gọi là Toolchain hay cross-compilation toolchain, cho phép:
    - Biên dịch (compile) mã nguồn
    - Dịch hợp ngữ (assemble),
    - Liên kết (link) các file đối tượng để tạo file thực thi.
- Toolchain cũng bao gồm các công cụ nhị phân dùng để gỡ lỗi (debug) ứng dụng trên thiết bị đích (target).
- Ngoài ra, toolchain còn cung cấp các công cụ để phân tích file thực thi, bao gồm:
    - Dịch ngược mã máy (disassemble),
    - Chuyển đổi file thực thi sang các định dạng khác như hex, bin,
    - Trích xuất thông tin symbol và kích thước của tệp thực thi.
    - Cung cấp thư viện chuẩn của ngôn ngữ C