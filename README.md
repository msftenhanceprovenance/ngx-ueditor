# ngx-ueditor
Angular2.x for Baidu UEditor（[UMeditor](https://github.com/cipchk/ngx-umeditor)）

[![NPM version](https://img.shields.io/npm/v/ngx-ueditor.svg)](https://www.npmjs.com/package/ngx-ueditor)
[![Build Status](https://travis-ci.org/cipchk/ngx-ueditor.svg?branch=master)](https://travis-ci.org/cipchk/ngx-ueditor)


## Demo

[Live Demo](https://cipchk.github.io/ngx-ueditor/)

## 特性

+ 懒加载 ueditor.all.js 文件。
+ 支持ueditor事件监听与移除
+ 支持语言切换
+ 支持ueditor实例对象直接访问。

## 使用

### 1、安装

```
npm install ngx-ueditor --save
```

把 `UeditorModule` 模块导入到你项目中。

```typescript
import { UeditorModule } from 'ngx-ueditor';

@NgModule({
    imports: [BrowserModule, UeditorModule ],
    declarations: [AppComponent],
    bootstrap: [AppComponent]
})
export class AppModule { }
```

### 2、使用

```html
<ueditor [(ngModel)]="full_source" 
         [config]="{...}"
         [path]="'./assets/ueditor/'"
         [loadingTip]="'加载中……'"
         (onReady)=""
         (onDestroy)=""
         (onContentChange)=""></ueditor>
```

| 名称    | 类型           | 默认值  | 描述 |
| ------- | ------------- | ----- | ----- |
| config | Object |  | 前端配置项说明，[见官网](http://fex.baidu.com/ueditor/#start-config) |
| path | string | ./assets/ueditor/ | ueditor代码根目录路径，以 `/` 结尾。 |
| loadingTip | string | 加载中... | 初始化提示文本。 |
| onReady | Function |  | 编辑器准备就绪后会触发该事件 |
| onDestroy | Function |  | **编辑器组件销毁**后会触发该事件 |
| onContentChange | Function |  | 编辑器内容发生改变时会触发该事件 |

### 3、关于懒加载

懒加载在未到 `wdinow.UE` 时会启动，如果你在 `index.html` 已经使用 `<script src="ueditor.all.js"></script>` 加载过，懒加载流程将会失效。

**加载语言注意点**

懒加载会自动识别并引用，否则，需要自行在 `<head>` 加入语言版本脚本。

## 访问ueditor实例对象

首先，需要给组件定义一下模板变量：

```html
<ueditor [(ngModel)]="full_source" #full></ueditor>
```

使用 `@ViewChild` 访问组件，并使用 `this.full.Instance` 访问ueditor实例对象。

```typescript
export class DemoComponent {
    @ViewChild('full') full: UeditorComponent;
    constructor(private el: ElementRef) {}

    getAllHtml() {
        // 通过 `this.full.Instance` 访问ueditor实例对象
        alert(this.full.Instance.getAllHtml())
    }
}
```

## 事件

虽说上节也可以直接注册ueditor事件，但当组件被销毁时可能会引发内存泄露。所以**不建议直接在ueditor实例中这么做**。组件本身提供 `addListener` 和 `removeListener` 来帮你处理。

```typescript
// 事件监听
this.full.addListener('focus', () => {
    this.focus = `fire focus in ${new Date().getTime()}`;
});
// 事件移除
this.full.removeListener('focus');
```

## 表单非空校验

组件加入 `required` 当编辑器为空时会处于 `ng-invalid` 状态，具体体验见[Live Demo](https://cipchk.github.io/ngx-ueditor/)。

## 组件接口

```typescript
interface UeditorComponent {
    /**
     * 获取UE实例
     * 
     * @readonly
     */
    get Instance(): any;

        /**
     * 设置编辑器语言
     * 
     * @param {('zh-cn' | 'en')} lang 
     */
    setLanguage(lang: 'zh-cn' | 'en') {}

    /**
     * 添加编辑器事件
     */
    addListener(eventName: 'destroy' | 'reset' | 'focus' | 'langReady' | 'beforeExecCommand' | 'afterExecCommand' | 'firstBeforeExecCommand' | 'beforeGetContent' | 'afterGetContent' | 'getAllHtml' | 'beforeSetContent' | 'afterSetContent' | 'selectionchange' | 'beforeSelectionChange' | 'afterSelectionChange', 
                fn: Function): void {}

    /**
     * 移除编辑器事件
     * 
     * @param {('destroy' | 'reset' | 'focus' | 'langReady' | 'beforeExecCommand' | 'afterExecCommand' | 'firstBeforeExecCommand' | 'beforeGetContent' | 'afterGetContent' | 'getAllHtml' | 'beforeSetContent' | 'afterSetContent' | 'selectionchange' | 'beforeSelectionChange' | 'afterSelectionChange')} eventName 
     */
    removeListener(eventName: 'destroy' | 'reset' | 'focus' | 'langReady' | 'beforeExecCommand' | 'afterExecCommand' | 'firstBeforeExecCommand' | 'beforeGetContent' | 'afterGetContent' | 'getAllHtml' | 'beforeSetContent' | 'afterSetContent' | 'selectionchange' | 'beforeSelectionChange' | 'afterSelectionChange'): void {}
}
```

## Troubleshooting

Please follow this guidelines when reporting bugs and feature requests:

1. Use [GitHub Issues](https://github.com/cipchk/ngx-ueditor/issues) board to report bugs and feature requests (not our email address)
2. Please **always** write steps to reproduce the error. That way we can focus on fixing the bug, not scratching our heads trying to reproduce it.

Thanks for understanding!

### License

The MIT License (see the [LICENSE](https://github.com/cipchk/ngx-ueditor/blob/master/LICENSE) file for the full text)
