# Khởi tạo dự án

> mkdir nodejs-typescript
> cd nodejs-typescript

Tiếp theo, setup dự án với package.json và thêm các dependencies cần thiết.

# Tạo dự án Node.js

Sử dụng -y khi chạy lệnh npm init khi tạo file package.json để không cần nhập các thông tin về project. Có thể vào file package.json để chỉnh sửa sau.

> npm init -y

# Cài đặt TypeScript vào Dev Dependency

Để sử dụng Typescript, cần phải cài đặt nó trước.

> npm install typescript --save-dev

Sau khi cài typescript, có thể dùng TypeScript để biên dịch code bằng câu lệnh tsc (lưu ý là khi cài local nên muốn dùng tsc thì phải thông qua file package.json hoặc dùng npx tsc).

# Cài đặt kiểu dữ liệu TypeScript cho Node.js

Vì dùng TypeScript để code Node.js nên cần cài thêm kiểu dữ liệu cho Node.js.

> npm install @types/node --save-dev

# Cài đặt các package config cần thiết

Cần cài đặt các package config cần thiết để làm việc với TypeScript như ESLint, Prettier, ...

> npm install eslint prettier eslint-config-prettier eslint-plugin-prettier @typescript-eslint/eslint-plugin @typescript-eslint/parser ts-node tsc-alias tsconfig-paths rimraf nodemon --save-dev

- **eslint**: Linter (bộ kiểm tra lỗi) chính
- **prettier**: Code formatter chính
- **eslint-config-prettier**: Cấu hình ESLint để không bị xung đột với Prettier
- **eslint-plugin-prettier**: Dùng thêm một số rule prettier cho eslint
- **@typescript-eslint/eslint-plugin**: ESLint plugin cung cấp các rule cho Typescript
- **@typescript-eslint/parser**: Parser cho phép ESLint kiểm tra lỗi Typescript
- **ts-node**: Dùng để chạy TypeScript code trực tiếp mà không cần build
- **tsc-alias**: Xử lý alias khi build
- **tsconfig-paths**: Khi setting alias import trong dự án dùng ts-node thì chúng ta cần dùng tsconfig-paths để nó hiểu được paths và baseUrl trong file tsconfig.json
- **rimraf**: Dùng để xóa folder dist khi trước khi build
- **nodemon**: Dùng để tự động restart server khi có sự thay đổi trong code

# Cấu hình tsconfig.json

Tạo file tsconfig.json tại thư mục root, có thể tạo bằng lệnh touch tsconfig.json

Tiếp theo copy và paste cấu hình dưới đây vào file tsconfig.json

> [!NOTE]

{
"compilerOptions": {
  "module": "CommonJS", // Quy định output module được sử dụng
  "moduleResolution": "node", //
  "target": "ES2020", // Target ouput cho code
  "outDir": "dist", // Đường dẫn output cho thư mục build
  "esModuleInterop": true /* Emit additional JavaScript to ease support for importing CommonJS modules. This enables 'allowSyntheticDefaultImports' for type compatibility. */,
  "strict": true /* Enable all strict type-checking options. */,
 "skipLibCheck": true /* Skip type checking all .d.ts files. */,
  "baseUrl": ".", // Đường dẫn base cho các import
  "paths": {
    "~/*": ["src/*"] // Đường dẫn tương đối cho các import (alias)
  }
},
"ts-node": {
  "require": ["tsconfig-paths/register"]
},
  "files": ["src/type.d.ts"], // Các file dùng để defined global type cho dự án
  "include": ["src/**/*"] // Đường dẫn include cho các file cần build
}

# Cấu hình file config cho ESLint

## Tạo file .eslintrc tại thư mục root, copy và paste config dưới đây vào file .eslintrc của bạn

> [!NOTE]

{
  "root": true,
  "parser": "@typescript-eslint/parser",
  "plugins": ["@typescript-eslint", "prettier"],
  "extends": ["eslint:recommended", "plugin:@typescript-eslint/recommended", "eslint-config-prettier", "prettier"],
  "rules": {
    "@typescript-eslint/no-explicit-any": "off",
    "@typescript-eslint/no-unused-vars": "off",
    "prettier/prettier": [
      "warn",
      {
        "arrowParens": "always",
        "semi": false,
        "trailingComma": "none",
        "tabWidth": 2,
        "endOfLine": "auto",
        "useTabs": false,
        "singleQuote": true,
        "printWidth": 120,
        "jsxSingleQuote": true
      }
    ]
  }
}

## Tiếp theo tạo file .eslintignore để ignore các file không cần kiểm tra lỗi

> node_modules/
> dist/

# Cấu hình file config cho Prettier

## Tạo file .prettierrc trong thư trong thư mục root với nội dung dưới đây

> [!NOTE]

{
  "arrowParens": "always",
  "semi": false,
  "trailingComma": "none",
  "tabWidth": 2,
  "endOfLine": "auto",
  "useTabs": false,
  "singleQuote": true,
  "printWidth": 120,
  "jsxSingleQuote": true
}

## Tiếp theo Tạo file .prettierignore ở thư mục root

Mục đích là Prettier bỏ qua các file không cần thiết

> node_modules/
> dist/

# Config editor để chuẩn hóa cấu hình editor

Tạo file .editorconfig ở thư mục root
Mục đích là cấu hình các config đồng bộ các editor với nhau nếu dự án có nhiều người tham gia.

> [*]
> indent_size = 2
> indent_style = space

# Cấu hình file gitignore

Tạo file .gitignore ở thư mục root
Mục đích là cấu hình các file không cần đẩy lên git

> node_modules/
> dist/

# Cấu hình file nodemon.json

Tạo file nodemon.json ở thư mục root
Mục đích là cấu hình nodemon để tự động restart server khi có sự thay đổi trong code

> [!NOTE]

{
  "watch": ["src"],
  "ext": ".ts,.js",
  "ignore": [],
  "exec": "npx ts-node ./src/index.ts"
}

# Cấu hình file package.json

Mở file package.json lên, thêm đoạn script này vào

> [!NOTE]

 "scripts": {
    "dev": "npx nodemon",
    "build": "rimraf ./dist && tsc && tsc-alias",
    "start": "node dist/index.js",
    "lint": "eslint .",
    "lint:fix": "eslint . --fix",
    "prettier": "prettier --check .",
    "prettier:fix": "prettier --write ."
  }

# Tạo file type.d.ts

Tạo file type.d.ts trong thư mục src
Mục đích là để defined các global type cho dự án.
Mở file tsconfig.json lên sẽ thấy dòng add file này vào để cho typescript nhận diện

# Tạo file index.ts

Tạo file index.ts trong thư mục src

> const hello: string = 'Hello Word !'
> console.log(hello)

# Câu lệnh để chạy dự án

## Chạy dự án trong môi trường dev

> npm run dev

## Build dự án TypeScript sang JavaScript cho production

> npm run build

Sau khi build, có thể chạy dự án bằng câu lệnh 

> npm run start

## Kiểm tra lỗi ESLint / Prettier

### Câu lệnh giúp kiểm tra lỗi ESLint/Prettier trong dự án

> npm run lint
> npm run prettier

### Câu lệnh giúp ESLint/Prettier tự động fix lỗi 

> npm run lint:fix
> npm run prettier:fix