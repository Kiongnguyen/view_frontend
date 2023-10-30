# SETUP

> Cách setup một dự án thực tế dùng `react+ts`
## Contents
1. [Vite](#1)
2. [Git/Github](#2)
3. [Config](#3)
   1. [Prettier](#3.1)
   2. [ESlint](#3.2)
   3. [editorconfig](#3.3)
   4. [files_ignore](#3.4)
4. [Project file system](#4)
5. [Library vs install](#5)
6. [Theme](#6)
7. [Router](#7)
## Vite <a id="1"></a>
>Link youtube: [Vite](https://vitejs.dev/)  
> Trước khi cài `React` thì phải cài `Node.js` có thể cài trực tiếp node theo [link downloads](https://nodejs.org/en/download/current)  

> `Install`: 
* With `NPM`:
```bash 
npm create vite@latest
```
* With `Yarn`:
```bash 
yarn create vite
``` 
> vì trong project này dùng với ts nên chúng ta chọn mục `React` ==> `typescript + SWC`
```bash
    √ Project name: ... .
    √ Package name: ... cloneminimal
    √ Select a framework: » React
    √ Select a variant: » TypeScript + SWC
```
## Git/Github <a id="2"></a>
>Link youtube: [Git/Github](https://www.youtube.com/playlist?list=PLodO7Gi1F7R0t9SyEZF5mwfKevCULLjgG)

> Những kĩ năng cơ bản cần biết có thể xem trong file [Task1_Git/Github](https://github.com/Kiongnguyen/view_frontend.git)

> Chú ý cách tạo một `Commit` :  

   `Pattern`: 
   ```
   git commit -m "#IssueNumber feat/chore/config/fix (file? or folder?) mess"
   ```

        feat - tạo cái gì đó mới.
        chore - sửa cái gì đó.
        config - setting cái gì đó.
        fix - sửa lỗi nào rồi.
 

## Config <a id="3"></a>

### Prettier <a id="3.1"></a>
>  `Prettier` để định dạng mã nguồn của mình để đảm bảo mã nguồn được hiển thị thống nhất và dễ đọc.

> `Install`:
   ```bash
   npm i prettier
   ```

> Cài `Extension` tích hợp vào Visual Studio Code.

> Bạn có thể cấu hình Prettier để phù hợp với nhu cầu của mình. Để cấu hình Prettier, bạn có thể tạo tệp cấu hình `.prettierrc` trong thư mục gốc của dự án.

> Bạn có thể tìm hiểu thêm về các cài đặt của Prettier tại trang chủ của [link-Prettier](https://prettier.io/docs/en/configuration.html.)

```json
    {
  "printWidth": 100,//Độ rộng tối đa của một dòng mã, tính theo số ký tự
  "singleQuote": true,//Sử dụng dấu nháy đơn thay vì dấu nháy kép cho chuỗi
  "trailingComma": "es5",//Vị trí dấu phẩy cuối cùng trong một mảng hoặc đối tượng.
  "tabWidth": 2 //Độ rộng của một tab, tính theo số ký tự
    }//có thêm nhiều tùy chọn nhưng đây là vd cơ bản nhất

```
## ESlint<a id="3.2"></a>
>`ESLint` là một công cụ phân tích tĩnh mã nguồn JavaScript, giúp bạn phát hiện và sửa chữa các lỗi tiềm ẩn trong mã của mình.

> Nhưng với dự án dùng Typescript chúng ta sẽ cài đặt gói ESLint theo các hướng dẫn của `Airbnb`. Các quy tắc này được thiết kế để giúp bạn viết mã TypeScript tốt hơn. [Đọc thêm về Airbnb](https://www.npmjs.com/package/eslint-config-airbnb)
```bash
    npm i eslint-config-airbnb-typescript
```
>Để sử dụng gói `eslint-config-airbnb-typescript`, bạn cần thêm nó vào tệp cấu hình ESLint (`.eslintr`) của mình. Bạn có thể thêm nó theo cách sau:

```json
    {
  "root": true,//Điều này cho ESLint biết rằng đây là tệp cấu hình gốc và nó sẽ không tìm kiếm các tệp cấu hình khác.
  "env": {
    "browser": true,
    "es2021": true
  },//Điều này cho ESLint biết rằng mã của bạn sẽ được chạy trong trình duyệt và nó sẽ sử dụng cú pháp ES2021.
  "plugins": ["unused-imports", "@typescript-eslint", "prettier"],//giúp bạn phát hiện và loại bỏ các import không sử dụng, phân tích mã TypeScript của mình, định dạng mã của mình theo các quy tắc của Prettier.
  "extends": ["airbnb", "airbnb-typescript", "airbnb/hooks", "prettier"],// Bộ quy tắc  mã JavaScript, mã TypeScript, React Hooks, quy tắc của Prettier.
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaFeatures": {
      "jsx": true //có dùng file jsx
    },
    "ecmaVersion": "latest",
    "sourceType": "module",
    "project": "./tsconfig.json",
    "warnOnUnsupportedTypeScriptVersion": false//Cho ESLint biết rằng bạn không muốn nhận cảnh báo về các phiên bản TypeScript không được hỗ trợ.
  },
  "settings": {
    "import/resolver": {
      "typescript": {
        "alwaysTryTypes": true
      }
    }
  },
  "rules": {
    "no-alert": 0,
    "camelcase": 0,
    "no-console": 0,
    "no-unused-vars": 0,
    "no-param-reassign": 0,
    "no-underscore-dangle": 0,
    "no-restricted-exports": 0,
    "react/no-children-prop": 0,
    "react/react-in-jsx-scope": 0,
    "jsx-a11y/anchor-is-valid": 0,
    "react/no-array-index-key": 0,
    "no-promise-executor-return": 0,
    "react/require-default-props": 0,
    "react/jsx-props-no-spreading": 0,
    "import/prefer-default-export": 0,
    "react/function-component-definition": 0,
    "@typescript-eslint/naming-convention": 0,
    "jsx-a11y/control-has-associated-label": 0,
    "@typescript-eslint/no-use-before-define": 0,
    "react/jsx-no-useless-fragment": [
      1,
      {
        "allowExpressions": true
      }
    ],
    "prefer-destructuring": [
      1,
      {
        "object": true,
        "array": false
      }
    ],
    "react/no-unstable-nested-components": [
      1,
      {
        "allowAsProps": true
      }
    ],
    "@typescript-eslint/no-unused-vars": [
      1,
      {
        "args": "none"
      }
    ],
    "react/jsx-no-duplicate-props": [
      1,
      {
        "ignoreCase": false
      }
    ],
    "unused-imports/no-unused-imports": 0,
    "unused-imports/no-unused-vars": [
      0,
      {
        "vars": "all",
        "varsIgnorePattern": "^_",
        "args": "after-used",
        "argsIgnorePattern": "^_"
      }
    ]
  }
}

```
> Tệp cấu hình `ESLint` này cung cấp một bộ quy tắc cơ bản cho mã `JavaScript` và `TypeScript`. Bạn có thể tùy chỉnh tệp này để phù hợp với nhu cầu của mình. Bạn có thể bắt đầu với một bộ quy tắc cơ bản như bộ quy tắc được cung cấp trong tệp cấu hình này. Sau đó, bạn có thể thêm hoặc xóa các quy tắc khi cần thiết.

### Editorconfig <a id="3.3"></a>
> `EditorConfig` có thể được sử dụng để định dạng mã của bạn theo các quy tắc nhất định. Điều này có thể giúp bạn cải thiện tính nhất quán và dễ đọc của mã của mình. EditorConfig có thể được sử dụng để tự động định dạng mã của bạn khi bạn lưu file. Điều này có thể giúp bạn tiết kiệm thời gian và đảm bảo rằng mã của bạn luôn được định dạng theo các quy tắc đã đặt.

> Để cài đặt EditorConfig trong VS Code, bạn có thể thực hiện theo các bước sau:

    1. Mở VS Code.
    2. Nhấn Ctrl+Shift+P để mở Command Palette.
    3. Nhập "Install Extension" và nhấn Enter.
    4. Nhập "EditorConfig for Visual Studio Code" và nhấn Enter.
    5. Nhấn Enter để cài đặt extension.
> Sau khi `extension` được cài đặt, bạn có thể tạo một tệp `.editorconfig` trong thư mục gốc của dự án của mình. Tệp này sẽ chứa các quy tắc định dạng mà bạn muốn áp dụng cho mã của mình.
```
    root = true
    [*]
    charset = utf-8
    end_of_line = lf
    indent_style = space
    indent_size = 2
    trim_trailing_whitespace = false
    insert_final_newline = false

```
>Để sử dụng tệp `.editorconfig` này, bạn cần cài đặt `extension` EditorConfig cho VS Code. Sau khi extension được cài đặt, VS Code sẽ tự động áp dụng các quy tắc định dạng trong tệp này cho mã của bạn.

### Files_ignore <a id="3.4"></a>

> Một số file `ignore` gồm: `.gitignore`, `.eslintignore`, `.prettierignone`

>`.gitignore` ==> file để loại bỏ những files trong thư mục sẽ không áp dụng đẩy lên git

```
     //Các dòng này loại bỏ tất cả các file nhật ký và file gỡ lỗi khỏi kho lưu trữ Git của bạn.
    # Logs
    logs
    *.log
    npm-debug.log*
    yarn-debug.log*
    yarn-error.log*
    pnpm-debug.log*
    lerna-debug.log*

   //Các dòng này loại bỏ thư mục node_modules và thư mục dist khỏi kho lưu trữ Git của bạn. Thư mục node_modules chứa các phụ thuộc của dự án của bạn, và thư mục dist chứa các file được biên dịch. Các file này có thể được tạo lại khi cần thiết, vì vậy không cần theo dõi chúng trong kho lưu trữ Git.
    
    node_modules
    dist
    dist-ssr
    *.local

    //Các dòng này loại bỏ tất cả các file và thư mục được sử dụng bởi các trình soạn thảo văn bản và IDE khỏi kho lưu trữ Git của bạn. Các file và thư mục này có thể gây ra xung đột khi nhiều người cộng tác trên cùng một dự án.

    # Editor directories and files
    .vscode/*
    !.vscode/extensions.json
    .idea
    .DS_Store
    *.suo
    *.ntvs*
    *.njsproj
    *.sln
    *.sw?
    
    //Các dòng này loại bỏ tất cả các file môi trường khỏi kho lưu trữ Git của bạn. Các file này thường chứa thông tin nhạy cảm, chẳng hạn như mật khẩu và khóa API. Không nên theo dõi các file này trong kho lưu trữ Git của bạn.

    # environment variables
    .env
    .env.local
    .env.development.local
    .env.test.local
    .env.production.local 
```
>`.eslintignore` ==> loại bỏ các file và thư mục khỏi quá trình phân tích mã của ESLint.
```
// .eslintignore

build/* Thư mục này thường chứa các file được biên dịch
dist/* Thư mục này thường chứa các file được biên dịch và phân phối
public/* Thư mục này thường chứa các file tĩnh, chẳng hạn như hình ảnh
**/out/*
**/node_modules/* Thư mục này chứa các phụ thuộc của dự án

**/.next/*
next.config.js

vite.config.js sử dụng để cấu hình Vite
vite.config.ts

src/reportWebVitals.js
src/service-worker.js
src/serviceWorkerRegistration.js
src/setupTests.js

src/reportWebVitals.ts
src/service-worker.ts
src/serviceWorkerRegistration.ts
src/setupTests.ts
README.md  chứa thông tin về dự án

```
> `.prettierignone` ==> loại bỏ các file không cần phải theo dõi mã bởi `prettier`
```
    build/*
    dist/*
    public/*
    **/out/*
    **/.next/*
    **/node_modules/*

    package-lock.json
    yarn.lock
    README.md

```
## Project file system <a id="4"></a>
> **Cấu trúc dự án:**
1. `node_modules`
   > file chứa các thư viện của node, react, thư viện thêm bên ngoài   
2. `public`
   > Chứa các thành phần mà khi buil ra sử dụng người dùng có thể thấy trực tiếp được các file ảnh, logo, icon,...
3. `src`
   1. `components` //Thư mục này chứa các thành phần React. Các thành phần là các khối xây dựng cơ bản của ứng dụng React.
   2. `hooks` // Thư mục này chứa các hook React. Các hook là các hàm nhỏ có thể được sử dụng để tái sử dụng mã trong các thành phần React.
   3. `layouts` //Thư mục này chứa các bố cục React. Các bố cục là các thành phần React được sử dụng để xác định bố cục chung của ứng dụng.
   4. `locales` //Thư mục này chứa các tệp ngôn ngữ cho ứng dụng. Các tệp ngôn ngữ được sử dụng để dịch ứng dụng sang các ngôn ngữ khác nhau.
   5. `pages` //Thư mục này chứa các trang React. Các trang là các thành phần React được sử dụng để hiển thị nội dung của ứng dụng.
   6. `routes` //Thư mục này chứa các tuyến đường React. Các tuyến đường xác định cách ứng dụng điều hướng đến các trang khác nhau.
   7. `sections` //Thư mục này chứa các phần React. Các phần là các thành phần React được sử dụng để hiển thị các phần nội dung cụ thể của ứng dụng.
   8. `theme` // Thư mục này chứa chủ đề React. Chủ đề xác định giao diện  chung của ứng dụng.
   9. `App.tsx` //File này là thành phần gốc của ứng dụng React. Thành phần gốc là thành phần chính của ứng dụng và chịu trách nhiệm khởi tạo ứng dụng.
   10. `main.tsx` // File này là file khởi động của ứng dụng. File này chịu trách nhiệm tạo và khởi chạy ứng dụng.
   
4. `file config`(git, prettier, ESlint, editor, vite.config, tsconfig)
   > cái này đã được giải thích trong phần config 
5. `index.html`
   > file hiển thì ra ngoài dao diện cho người dùng 
6. `package.json`
   > file chứa mọi thông tin về project, các thư viện sử dụng cũng như sever 
7. `README.md`
   > tài liệu giới dự án thiệu đính kèm 

## Library <a id="5"></a>
> Khi clone lại một dự án nào ấy thì việc đầu tiên để ý đến file `package.json` có chứa các thư viện cần dùng và version


> Chú ý về Sematic versioning
```
    Ví dụ: phiên bản 1.2.3 của phần mềm có nghĩa là:

    * Phiên bản chính là 1, cho biết đây là phiên bản lớn thứ hai của phần mềm.
    * Phiên bản phụ là 2, cho biết có thêm hai tính năng hoặc cải tiến mới so với phiên bản 1.0.
    * Phiên bản vá là 3, cho biết có sửa ba lỗi hoặc cải thiện hiệu suất so với phiên bản 1.2.2.
```
> Trong file `package.json` có 2 trường hợp có `^` và `~` trước version:  
  
 * `~` khi cập nhập lại chỉ cập nhập version đến số cuối cùng 
  
* `^` khi cập nhập lại chỉ cập nhập version đến số thứ 2

```json
    "dependencies": {
    "@babel/core": "^7.23.2",
    "@babel/plugin-syntax-flow": "~7.22.5",
    "@babel/plugin-transform-react-jsx": "^7.22.15"}
```

>`Install`:

> With `NPM`:
```bash
    npm install <tên thư viện > @ <version >
```
> With `Yarn`:
```bash
    yarn add <tên thư viện > @ <version >
```
> có thể cài nhiều thư viện 1 lúc bằng cách viết liền nhau

> Có thể copy danh sách các thư viện vào trong file `package.json` và chạy lênh `npm i` để cài tất cả các thư viện cần dùng, là 1 cách khá đảm bảo về mọi vấn đề :
```json
    "dependencies": {
    "@babel/core": "^7.23.2",
    "@babel/plugin-syntax-flow": "^7.22.5",
    "@babel/plugin-transform-react-jsx": "^7.22.15",
    "@emotion/cache": "^11.11.0",
    "@emotion/react": "^11.11.1",
    "@emotion/styled": "^11.11.0",
    "@mui/base": "^5.0.0-beta.13",
    "@mui/lab": "^5.0.0-alpha.142",
    "@mui/material": "^5.14.7",
    "@mui/system": "^5.14.7",
    "@mui/x-data-grid": "^6.12.0",
    "@mui/x-date-pickers": "^6.12.0",
    "@types/lodash.merge": "^4.6.7",
    "@types/node": "^20.8.6",
    "@vitejs/plugin-react": "^4.1.0",
    "eslint-config-airbnb-typescript": "^17.1.0",
    "framer-motion": "^10.16.2",
    "prettier": "^3.0.3",
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-helmet-async": "^1.3.0",
    "react-router": "^6.15.0",
    "react-router-dom": "^6.15.0",
    "styled-components": "^6.1.0"
  },
  "devDependencies": {
    "@iconify/react": "^4.1.1",
    "@types/react": "^18.2.21",
    "@types/react-dom": "^18.2.7",
    "@typescript-eslint/eslint-plugin": "^6.0.0",
    "@typescript-eslint/parser": "^6.0.0",
    "@vitejs/plugin-react-swc": "^3.3.2",
    "eslint": "^8.45.0",
    "eslint-plugin-react-hooks": "^4.6.0",
    "eslint-plugin-react-refresh": "^0.4.3",
    "typescript": "^5.2.2",
    "vite": "^4.4.5"
  }
```

1. MUI
2. helmet
3. router
4. iconify
5. framer-motion

## Theme <a id="6"></a>
### typography.ts
> File này sẽ định dạng về cỡ chữ, loại font chữ, chiều cao chữ, và độ đậm của chữ, thay đổi cỡ chữ theo kích thước màn
```tsx
//khai báo interface TypographyVariants với thuộc tính fontWeightSemiBold để cho trình biên dịch biết rằng thuộc tính này tồn tại trong module @mui/material/styles.
  declare module '@mui/material/styles' {
  interface TypographyVariants {
    fontWeightSemiBold: React.CSSProperties['fontWeight'];
  }
}
// chuyển đổi đơn vị rem và em ra px
export function remToPx(value: string) {
  return Math.round(parseFloat(value) * 16);
}
export function pxToRem(value: number) {
  return `${value / 16}rem`;
}
// thay đổi kích thước chữ theo reposive
interface ScreenType {
  sm: number;
  md: number;
  lg: number;
}
export function responsiveFontSizes(params: ScreenType) {
  const { sm, md, lg } = params;
  return {
    '@media (min-width:600px)': {
      fontSize: pxToRem(sm),
    },
    '@media (min-width:900px)': {
      fontSize: pxToRem(md),
    },
    '@media (min-width:1200px)': {
      fontSize: pxToRem(lg),
    },
  };
}
//chon font chữ 
const primaryFont = 'Public Sans, sans-serif';
export const typography = {
  fontFamily: primaryFont,
  fontWeightRegular: 400,
  fontWeightMedium: 500,
  fontWeightSemiBold: 600,
  fontWeightBold: 700,
  h1: {
    fontWeight: 800,
    lineHeight: 80 / 64,
    fontSize: pxToRem(40),
    ...responsiveFontSizes({ sm: 52, md: 58, lg: 64 }),
  },
  h2: {
    fontWeight: 800,
    lineHeight: 64 / 48,
    fontSize: pxToRem(32),
    ...responsiveFontSizes({ sm: 40, md: 44, lg: 48 }),
  },
  h3: {
    fontWeight: 700,
    lineHeight: 1.5,
    fontSize: pxToRem(24),
    ...responsiveFontSizes({ sm: 26, md: 30, lg: 32 }),
  },
  h4: {
    fontWeight: 700,
    lineHeight: 1.5,
    fontSize: pxToRem(20),
    ...responsiveFontSizes({ sm: 20, md: 24, lg: 24 }),
  },
  h5: {
    fontWeight: 700,
    lineHeight: 1.5,
    fontSize: pxToRem(18),
    ...responsiveFontSizes({ sm: 19, md: 20, lg: 20 }),
  },
  h6: {
    fontWeight: 700,
    lineHeight: 28 / 18,
    fontSize: pxToRem(17),
    ...responsiveFontSizes({ sm: 18, md: 18, lg: 18 }),
  },
  subtitle1: {
    fontWeight: 600,
    lineHeight: 1.5,
    fontSize: pxToRem(16),
  },
  subtitle2: {
    fontWeight: 600,
    lineHeight: 22 / 14,
    fontSize: pxToRem(14),
  },
  body1: {
    lineHeight: 1.5,
    fontSize: pxToRem(16),
  },
  body2: {
    lineHeight: 22 / 14,
    fontSize: pxToRem(14),
  },
  caption: {
    lineHeight: 1.5,
    fontSize: pxToRem(12),
  },
  overline: {
    fontWeight: 700,
    lineHeight: 1.5,
    fontSize: pxToRem(12),
    textTransform: 'uppercase',
  },
  button: {
    fontWeight: 700,
    lineHeight: 24 / 14,
    fontSize: pxToRem(14),
    textTransform: 'unset',
  },
};

```

### palette.ts
> File này sẽ định dạng về màu sắc chung 
```tsx
    import { alpha } from '@mui/material/styles';
// dữ liệu về bảng màu
export type ColorSchema = 'primary' | 'secondary' | 'info' | 'success' | 'warning' | 'error';
const GREY = {
  0: '#FFFFFF',
  100: '#F9FAFB',
  200: '#F4F6F8',
  300: '#DFE3E8',
  400: '#C4CDD5',
  500: '#919EAB',
  600: '#637381',
  700: '#454F5B',
  800: '#212B36',
  900: '#161C24',
};
const PRIMARY = {
  lighter: '#C8FAD6',
  light: '#5BE49B',
  main: '#00A76F',
  dark: '#007867',
  darker: '#004B50',
  contrastText: '#FFFFFF',
};

const SECONDARY = {
  lighter: '#EFD6FF',
  light: '#C684FF',
  main: '#8E33FF',
  dark: '#5119B7',
  darker: '#27097A',
  contrastText: '#FFFFFF',
};

const INFO = {
  lighter: '#CAFDF5',
  light: '#61F3F3',
  main: '#00B8D9',
  dark: '#006C9C',
  darker: '#003768',
  contrastText: '#FFFFFF',
};

const SUCCESS = {
  lighter: '#D3FCD2',
  light: '#77ED8B',
  main: '#22C55E',
  dark: '#118D57',
  darker: '#065E49',
  contrastText: '#ffffff',
};

const WARNING = {
  lighter: '#FFF5CC',
  light: '#FFD666',
  main: '#FFAB00',
  dark: '#B76E00',
  darker: '#7A4100',
  contrastText: GREY[800],
};

const ERROR = {
  lighter: '#FFE9D5',
  light: '#FFAC82',
  main: '#FF5630',
  dark: '#B71D18',
  darker: '#7A0916',
  contrastText: '#FFFFFF',
};

const COMMON = {
  common: {
    black: '#000000',
    white: '#FFFFFF',
  },
  primary: PRIMARY,
  secondary: SECONDARY,
  info: INFO,
  success: SUCCESS,
  warning: WARNING,
  error: ERROR,
  grey: GREY,
  divider: alpha(GREY[500], 0.2),
  action: {
    hover: alpha(GREY[500], 0.08),
    selected: alpha(GREY[500], 0.16),
    disabled: alpha(GREY[500], 0.8),
    disabledBackground: alpha(GREY[500], 0.24),
    focus: alpha(GREY[500], 0.24),
    hoverOpacity: 0.08,
    disabledOpacity: 0.48,
  },
};
// bảng màu 2 chế độ light vs dark
export function palette(mode: 'light' | 'dark') {
  const light = {
    ...COMMON,
    mode: 'light',
    text: {
      primary: GREY[800],
      secondary: GREY[600],
      disabled: GREY[500],
    },
    background: {
      paper: '#FFFFFF',
      default: '#FFFFFF',
      neutral: GREY[200],
    },
    action: {
      ...COMMON.action,
      active: GREY[600],
    },
  };
  const dark = {
    ...COMMON,
    mode: 'dark',
    text: {
      primary: '#FFFFFF',
      secondary: GREY[500],
      disabled: GREY[600],
    },
    background: {
      paper: GREY[800],
      default: GREY[900],
      neutral: alpha(GREY[500], 0.12),
    },
    action: {
      ...COMMON.action,
      active: GREY[500],
    },
  };
  return mode === 'light' ? light : dark;
}

```

### typography.ts
> File này sẽ định dạng về cỡ chữ, loại font chữ, chiều cao chữ, và độ đậm của chữ, thay đổi cỡ chữ theo kích thước màn
```tsx
  import { Shadows, alpha } from '@mui/material/styles';
import { palette as themePalette } from './palette';
const palette = themePalette('light');
//chọn màu cho shawdow
const LIGHT_MODE = palette.grey[500];

const DARK_MODE = palette.common.black;

function createShadow(color: string): Shadows {
  const transparent1 = alpha(color, 0.2);
  const transparent2 = alpha(color, 0.14);
  const transparent3 = alpha(color, 0.12);
  //Hàm alpha nhận hai tham số: tham số thứ nhất là màu sắc và tham số thứ hai là độ mờ đục. Độ mờ đục có giá trị từ 0 đến 1, trong đó 0 là hoàn toàn trong suốt và 1 là hoàn toàn đục.
  return [
    'none',
    `0px 2px 1px -1px ${transparent1},0px 1px 1px 0px ${transparent2},0px 1px 3px 0px ${transparent3}`,
    `0px 3px 1px -2px ${transparent1},0px 2px 2px 0px ${transparent2},0px 1px 5px 0px ${transparent3}`,
    `0px 3px 3px -2px ${transparent1},0px 3px 4px 0px ${transparent2},0px 1px 8px 0px ${transparent3}`,
    `0px 2px 4px -1px ${transparent1},0px 4px 5px 0px ${transparent2},0px 1px 10px 0px ${transparent3}`,
    `0px 3px 5px -1px ${transparent1},0px 5px 8px 0px ${transparent2},0px 1px 14px 0px ${transparent3}`,
    `0px 3px 5px -1px ${transparent1},0px 6px 10px 0px ${transparent2},0px 1px 18px 0px ${transparent3}`,
    `0px 4px 5px -2px ${transparent1},0px 7px 10px 1px ${transparent2},0px 2px 16px 1px ${transparent3}`,
    `0px 5px 5px -3px ${transparent1},0px 8px 10px 1px ${transparent2},0px 3px 14px 2px ${transparent3}`,
    `0px 5px 6px -3px ${transparent1},0px 9px 12px 1px ${transparent2},0px 3px 16px 2px ${transparent3}`,
    `0px 6px 6px -3px ${transparent1},0px 10px 14px 1px ${transparent2},0px 4px 18px 3px ${transparent3}`,
    `0px 6px 7px -4px ${transparent1},0px 11px 15px 1px ${transparent2},0px 4px 20px 3px ${transparent3}`,
    `0px 7px 8px -4px ${transparent1},0px 12px 17px 2px ${transparent2},0px 5px 22px 4px ${transparent3}`,
    `0px 7px 8px -4px ${transparent1},0px 13px 19px 2px ${transparent2},0px 5px 24px 4px ${transparent3}`,
    `0px 7px 9px -4px ${transparent1},0px 14px 21px 2px ${transparent2},0px 5px 26px 4px ${transparent3}`,
    `0px 8px 9px -5px ${transparent1},0px 15px 22px 2px ${transparent2},0px 6px 28px 5px ${transparent3}`,
    `0px 8px 10px -5px ${transparent1},0px 16px 24px 2px ${transparent2},0px 6px 30px 5px ${transparent3}`,
    `0px 8px 11px -5px ${transparent1},0px 17px 26px 2px ${transparent2},0px 6px 32px 5px ${transparent3}`,
    `0px 9px 11px -5px ${transparent1},0px 18px 28px 2px ${transparent2},0px 7px 34px 6px ${transparent3}`,
    `0px 9px 12px -6px ${transparent1},0px 19px 29px 2px ${transparent2},0px 7px 36px 6px ${transparent3}`,
    `0px 10px 13px -6px ${transparent1},0px 20px 31px 3px ${transparent2},0px 8px 38px 7px ${transparent3}`,
    `0px 10px 13px -6px ${transparent1},0px 21px 33px 3px ${transparent2},0px 8px 40px 7px ${transparent3}`,
    `0px 10px 14px -6px ${transparent1},0px 22px 35px 3px ${transparent2},0px 8px 42px 7px ${transparent3}`,
    `0px 11px 14px -7px ${transparent1},0px 23px 36px 3px ${transparent2},0px 9px 44px 8px ${transparent3}`,
    `0px 11px 15px -7px ${transparent1},0px 24px 38px 3px ${transparent2},0px 9px 46px 8px ${transparent3}`,
  ];
}

export function shadows(mode: 'light' | 'dark') {
  return mode === 'light' ? createShadow(LIGHT_MODE) : createShadow(DARK_MODE);
}

```
```
Các giá trị bóng đổ còn lại được định dạng như sau:

* offset-x offset-y blur-radius spread-radius color
* offset-x: Khoảng cách theo trục X của bóng đổ, tính theo pixel.
* offset-y: Khoảng cách theo trục Y của bóng đổ, tính theo pixel.
* blur-radius: Độ mờ của bóng đổ, tính theo pixel.
* spread-radius: Kích thước của bóng đổ, tính theo pixel.
* color: Màu sắc của bóng đổ.
```
### index.ts
> File này sẽ định dạng theme với các tùy chọn đã làm bên trên
```tsx
import { createTheme, ThemeProvider as MuiThemeProvider, ThemeOptions } from '@mui/material/styles';
import { useMemo } from 'react';
import { palette } from './palette';
import { shadows } from './shadows';
import { typography } from './typography';
import merge from 'lodash.merge';
import CssBaseline from '@mui/material/CssBaseline';

type Props = {
  children: React.ReactNode;
};

export default function ThemeProvider({ children }: Props) {
  //Biến baseOption được sử dụng để lưu trữ các tùy chọn theme cơ bản. Các tùy chọn theme này bao gồm màu sắc, bóng đổ, kiểu chữ và hình dạng của theme.
  const baseOption = useMemo(
    () => ({
      palette: palette('light'),
      shadows: shadows('light'),
      typography,
    }),
    []
  );
//Hàm merge() từ thư viện Lodash được sử dụng để kết hợp các đối tượng. Trong trường hợp này, nó được sử dụng để kết hợp các tùy chọn theme cơ bản với các tùy chọn theme tùy chỉnh của bạn.
  const memoizedValue = useMemo(() => merge(baseOption), [baseOption]);
  //Hàm createTheme() từ thư viện Material UI được sử dụng để tạo một theme. Theme là một đối tượng chứa các thuộc tính như màu sắc, bóng đổ, kiểu chữ, v.v.
  const theme = createTheme(memoizedValue as ThemeOptions);
  const themeWithLocale = useMemo(() => createTheme(theme), [theme]);
  return (
    //Component ThemeProvider từ thư viện Material UI được sử dụng để áp dụng theme cho tất cả các component con của nó.
    <MuiThemeProvider theme={themeWithLocale}>
    //Component CssBaseline từ thư viện Material UI được sử dụng để đặt một số kiểu CSS cơ bản cho ứng dụng của bạn, chẳng hạn như đặt lại kiểu chữ và kích thước phông chữ.
      <CssBaseline />
      {children}
    </MuiThemeProvider>
  );
}
```
## Router <a id="7"></a>

1. *main.tsx*
   > thành phần chính đẩy ra file html cua react
   ```tsx
   import React, { Suspense } from 'react';
    import ReactDOM from 'react-dom/client';
    import App from './App';
    import { BrowserRouter } from 'react-router-dom';
    import { HelmetProvider } from 'react-helmet-async';

    ReactDOM.createRoot(document.getElementById('root')!).render(
      <React.StrictMode>//thẻ kiểm tra của react
        <HelmetProvider>//viết titile cho từng trang
          <BrowserRouter>//dùng để sử dụng router 
            <Suspense>//tạo trang loading
              <App />//mọi dữ liệu từ app chảy qua đây
            </Suspense>
          </BrowserRouter>
        </HelmetProvider>
      </React.StrictMode>
    );
   ```
2. *app.tsx*
   > Thành phần thu mọi dữ liệu hiển thị lại 
   ```tsx
    import Router from 'src/routes/sections';
    import ThemeProvider from './theme';
    function App() {
      return (
        <ThemeProvider> //theme mình làm từ trước bao tất cả ở đây
         
            <Router />
          
        </ThemeProvider>
      );
    }

    export default App;
   ```
3. *sections/index.tsx*
   > từ đây sẽ tạo các đường dẫn 
   ```tsx
    import { useRoutes } from 'react-router-dom';
    import MainLayout from 'src/layouts/main/layout';
    import HomePage from 'src/pages/home';
    import { mainRoutes } from './main';
    export default function SimpleLayout() {
      return useRoutes([
        {
          path: '/',
          element: (
            <MainLayout>
              <HomePage />
            </MainLayout>
          ),
        },
        ...mainRoutes,
      ]);
    }
   ```

```
Hàm useRoutes() từ React Router DOM được sử dụng để định nghĩa các tuyến đường của ứng dụng. Mỗi tuyến đường là một đối tượng có các thuộc tính sau:

path: Đường dẫn của tuyến đường.
element: Component được hiển thị khi người dùng truy cập tuyến đường này.
Component MainLayout là một layout chung của ứng dụng. Layout này có thể bao gồm thanh điều hướng, thanh công cụ và các thành phần khác.

Component HomePage là component được hiển thị khi người dùng truy cập tuyến đường gốc (/).

Biến mainRoutes lưu trữ một mảng các tuyến đường của ứng dụng. Mỗi tuyến đường trong mảng này đều là một đối tượng có các thuộc tính path và element.

Hàm useRoutes() được sử dụng để hiển thị component phù hợp với tuyến đường hiện tại. Ví dụ, nếu người dùng truy cập tuyến đường gốc (/), thì component HomePage sẽ được hiển thị. Nếu người dùng truy cập tuyến đường /about, thì component AboutPage sẽ được hiển thị.
```
4. *pages/home.tsx*
   >thiệt lập trang home
   ```tsx
   import { Helmet } from 'react-helmet-async';
    import { HomeView } from 'src/sections/home/view';

    export default function HomePage() {
      return (
        <>
          <Helmet>
            <title>Home</title>
          </Helmet>
          <HomeView />
        </>
      );
    }
   ```
5. *sections/home/view.tsx*
   > cái này sẽ hiển thị trong trang home
   ```tsx
      export default function HomeView() {
       return <>Home</>;
      }
   ```