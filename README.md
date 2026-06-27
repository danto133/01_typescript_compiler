# Lộ trình Chi tiết Level 1: Làm chủ TypeScript Compiler (tsc)

## 🎯 Mục tiêu của Level 1

- **Hiểu rõ file `tsconfig.json` hoạt động ra sao**: Tự viết cấu hình từ đầu để hiểu từng thuộc tính, không dùng các mẫu generate tự động hoặc Boilerplate có sẵn.
- **Phân biệt được các hệ thống module**: Phân biệt bản chất của **CommonJS (CJS)** và **ES Modules (ESM)** ở tầng kết quả đầu ra sau khi biên dịch.
- **Làm chủ cấu trúc thư mục kinh điển**:
  - `src/`: Chứa mã nguồn TypeScript gốc (`.ts`).
  - `dist/`: Chứa mã nguồn JavaScript sau khi đã được biên dịch (`.js`).

---

## 📁 Cấu trúc Thư mục Dự án

```text
01_typescript_compiler/
├── src/
│   ├── math.ts         # Khai báo hàm sum bằng arrow function và export
│   └── index.ts        # Import hàm sum từ math.ts và thực thi console.log
├── dist/               # Thư mục chứa code JS sau khi build (tsc)
├── tsconfig.json       # File cấu hình TypeScript Compiler
├── package.json        # Chứa lệnh scripts và dependencies
└── README.md           # Hướng dẫn học tập (Tài liệu này)
```

---

## ⚙️ Cấu hình Hiện tại (`tsconfig.json`)

Trong dự án này, `tsconfig.json` được cấu hình cơ bản như sau:

```json
{
    "compilerOptions": {
        "target": "es5", // Dịch code về chuẩn ES5 (để tương thích trình duyệt cũ)
        "module": "commonjs", // Sử dụng hệ thống module CommonJS (require/exports) của Node.js
        "moduleResolution": "node", // Cách tsc tìm kiếm file import
        "outDir": "./dist", // Thư mục lưu mã nguồn JS đầu ra
        "rootDir": "./src", // Thư mục chứa mã nguồn TS gốc
        "strict": true, // Bật chế độ kiểm tra kiểu dữ liệu nghiêm ngặt
        "esModuleInterop": true, // Hỗ trợ import các module CommonJS tốt hơn
        "ignoreDeprecations": "6.0" // Bỏ qua cảnh báo deprecation cho target ES5 trong các phiên bản TS mới
    },
    "include": [
        "src/**/*"
    ]
}
```

---

## 🚀 Chạy Biên dịch (Build)

Để chạy biên dịch từ TypeScript sang JavaScript, hãy chạy lệnh dưới đây trong Terminal:

```bash
npm run build
```

Sau khi chạy xong, thư mục `dist/` sẽ xuất hiện chứa 2 file là `index.js` và `math.js`.

---

## 🔍 Phân tích Kết quả Biên dịch Mặc định

Khi mở các file trong thư mục `dist/`, bạn sẽ thấy TypeScript đã chuyển đổi mã nguồn hiện đại về mã nguồn cũ tương thích:

### 1. Phép chuyển đổi Module (CommonJS)
Cú pháp `export const sum` trong `src/math.ts` đã được dịch thành:
```javascript
exports.sum = ...
```
Và cú pháp `import { sum } from "./math"` ở `src/index.ts` đã chuyển thành:
```javascript
var math_1 = require("./math");
```

### 2. Phép chuyển đổi Target (ES5)
Arrow Function `() => {}` cực kỳ hiện đại trong `src/math.ts`:
```typescript
export const sum = (a: number, b: number): number => a + b;
```
đã bị biến đổi thành hàm JavaScript truyền thống trong `dist/math.js`:
```javascript
var sum = function (a, b) { return a + b; };
```
Điều này đảm bảo code có thể chạy được trên các môi trường JavaScript rất cũ (như Internet Explorer).

---

## 🎯 Bài tập Thử thách để "Tốt nghiệp" Level 1

Để thực sự nắm chắc kiến thức và tự rút ra bài học sâu sắc, bạn hãy tự tay thực hiện 2 thử thách cấu hình dưới đây:

### 🔥 Thử thách 1: Thay đổi Target (Phiên bản JavaScript đầu ra)
1. Mở file `tsconfig.json`.
2. Sửa dòng cấu hình `"target": "es5"` thành `"target": "ES2022"`.
3. Chạy lại lệnh build trong Terminal:
   ```bash
   npm run build
   ```
4. **Quan sát & Nhận xét**: Mở file `dist/math.js` xem Arrow Function (`=>`) có còn bị biến thành `function` thông thường hay không?
   > [!TIP]<br>
   > Bạn sẽ thấy các cú pháp hiện đại như arrow function được giữ nguyên vì ES2022 đã hỗ trợ native arrow functions!

### 🔥 Thử thách 2: Thay đổi Hệ thống Module (CommonJS vs ESM)
1. Mở file `tsconfig.json`.
2. Sửa dòng cấu hình `"module": "commonjs"` thành `"module": "ESNext"`.
3. Chạy lại lệnh build trong Terminal:
   ```bash
   npm run build
   ```
4. **Quan sát & Nhận xét**: Các từ khóa `require` và `exports` biến mất đúng không? Thay vào đó là từ khóa gì?
   > [!NOTE]<br>
   > Đây chính là cách trực quan nhất để bạn phân biệt sự khác nhau giữa **CommonJS (CJS)** và **ES Modules (ESM)** bằng mắt thường. ESM sử dụng cú pháp chuẩn hóa `import / export` nguyên bản thay vì giả lập bằng `require / exports` của Node.js.

---

Chúc bạn hoàn thành xuất sắc thử thách tốt nghiệp Level 1 và tự tin bước sang các level tiếp theo! 🚀
