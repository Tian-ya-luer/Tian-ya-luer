postcss 的优点:

1. 模块化架构: 使库非常小并且响应迅速;
2. 同时实现预处理和后处理;
3. 无缝支持所有任务处理程序, 如 gulp, grunt, broccoli;
4. 不需要编译依赖, 由javascript编写;
5. 可以创建任何需要的插件或修改插件;

缺点:

1. postcss需要语法正确的css, 否则容易编译失败;

## 1. 引入postcss

### 1. autoprefixer

安装 gulp (https://gulpjs.com/docs/en/getting-started/quick-start)

```bash
npm install --global gulp-cli
npm install --save-dev gulp
gulp --version
```

安装 gulp-postcss (https://www.npmjs.com/package/gulp-postcss)

```bash
npm install --save-dev postcss gulp-postcss
```

安装插件 autoprefixer

```bash
npm i -D autoprefixer
```

编写代码
gulpfile.js
```javascript
const { series, src, dest } = require("gulp");
const postcss = require("gulp-postcss");
const autoprefixer = require("autoprefixer");

function styles() {
  return src("src/*.css")
    .pipe(postcss([autoprefixer]))
    .pipe(dest("dest/"));
}

exports.default = series(styles);
```

创建文件夹 dest, src
src\example.css
```css
.text {
  background: linear-gradient(60deg, #cc4a60, #9658d8, #cc4a60, #9658d8, #cc4a60);
  color: transparent;
  font-size: 28px;
  background-clip: text;
}
```

运行 gulp
```bash
gulp
```

gulp 执行代码生成了一个文件 dest\example.css
```css
.text {
  background: linear-gradient(60deg, #cc4a60, #9658d8, #cc4a60, #9658d8, #cc4a60);
  color: transparent;
  font-size: 28px;
  -webkit-background-clip: text;
          background-clip: text;
}
```

可以看到插件 autoprefixer 为我们添加了前缀 -webkit-background-clip 属性

### 2. gulp-sourcemaps

安装 gulp-sourcemaps

```bash
npm i -D gulp-sourcemaps
```

修改代码
```javascript
const { series, src, dest } = require("gulp");
const postcss = require("gulp-postcss");
const autoprefixer = require("autoprefixer");
const sourcemaps = require("gulp-sourcemaps");

function styles() {
  return src("src/*.css")
    .pipe(sourcemaps.init())
    .pipe(postcss([autoprefixer]))
    .pipe(sourcemaps.write("./maps"))
    .pipe(dest("dest/"));
}

exports.default = series(styles);
```

执行代码可以看到生成了 dest\maps\example.css.map 文件

### 3. minified

安装 cssnano , gulp-rename

```bash
npm i -D cssnano
npm i -D gulp-rename
```

修改代码

```javascript
const { series, src, dest } = require("gulp");
const postcss = require("gulp-postcss");
const autoprefixer = require("autoprefixer");
const sourcemaps = require("gulp-sourcemaps");
const cssnano = require("cssnano");
const rename = require("gulp-rename");

function styles() {
  return src("src/*.css")
    .pipe(sourcemaps.init())
    .pipe(postcss([autoprefixer]))
    .pipe(sourcemaps.write("./maps"))
    .pipe(dest("dest/"));
}

function minified() {
  return src("dest/example.css")
    .pipe(postcss([cssnano]))
    .pipe(rename("example.min.css"))
    .pipe(dest("dest/"));
}

exports.default = series(styles, minified);
```

执行代码可以看到生成了 dest\example.min.css 文件

### 4. watch

```javascript

const { series, src, dest, watch } = require("gulp");
const postcss = require("gulp-postcss");
const autoprefixer = require("autoprefixer");
const sourcemaps = require("gulp-sourcemaps");
const cssnano = require("cssnano");
const rename = require("gulp-rename");

function styles() {
  return src("src/*.css")
    .pipe(sourcemaps.init())
    .pipe(postcss([autoprefixer]))
    .pipe(sourcemaps.write("./maps"))
    .pipe(dest("dest/"));
}

function minified() {
  return src("dest/example.css")
    .pipe(postcss([cssnano]))
    .pipe(rename("example.min.css"))
    .pipe(dest("dest/"));
}

function watchFile() {
  console.log("watch src/*.css change running task");
  watch("src/*.css", series(styles, minified));
}

exports.default = watchFile;

```

### 5. styleLint

安装 stylelint

```bash
npm init stylelint
```
