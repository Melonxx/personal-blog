看过`typescript`官网的5分钟上手之后，觉得语法上像极了JS，相比JS，TS更注重于 '`type`' （这样一看JS真的是垃圾，很垃圾），于是动动手写了一个计算器[(jsbin在线)](http://js.jirengu.com/bunos/3/edit)体验了一下。
##### [github](https://github.com/Melonxx/firstTS-calculate)
[预览](https://melonxx.github.io/firstTS-calculate/index.html)
#### typescript：
```
class Calculate {
    private n1: string = '';
    private n2: string = '';
    private option: string = '';
    private result: string = '';
    private keys: Array<Array<string>> = [
      ['Clear', '÷'],
      ['7', '8', '9', '×'],
      ['4', '5', '6', '-'],
      ['1', '2', '3', '+'],
      ['0', '.', '=']
    ];
    public bottomBar: HTMLDivElement;
    public top: HTMLDivElement;
    public wrap: HTMLDivElement;
    constructor(){
      this.createTopScreen();
      this.createBottomBar();
      this.createWrap();
      this.createAllButton();
      this.bindEvents();
    }
    createSingleButton(text: string, wrap: HTMLElement, className: string): HTMLButtonElement {
      let button: HTMLButtonElement = document.createElement('button');
      button.textContent = text;
      button.classList.add('button');
      button.classList.add(className?className:'');
      wrap.appendChild(button);
      return button;
    }
    createTopScreen(): void {
      let top:HTMLDivElement = document.createElement('div');
      top.classList.add('top-screen');
      top.textContent = '0';
      this.top = top;
    }
    createBottomBar(): void {
      let bottom:HTMLDivElement = document.createElement('div');
      bottom.classList.add('bottom-bar');
      this.bottomBar = bottom;
    }
    createWrap(): void {
      let wrap:HTMLDivElement = document.createElement('div');
      document.body.appendChild(wrap);
      wrap.classList.add('calc-wrap');
      wrap.appendChild(this.top);
      wrap.appendChild(this.bottomBar);
      this.wrap = wrap;
    }
    createAllButton(): void {
      this.keys.map((v1:Array<string>): void => {
        let div:HTMLDivElement = document.createElement('div');
        div.classList.add('button-bar');
        v1.map((v2: string): void => {
          this.createSingleButton(v2, div, `button-${v2}`);
        })
        this.bottomBar.appendChild(div);
      })
    }
    checkNumber(num: string, text: string): void{
      if(text === '.' && this[num].indexOf('.') >= 0){
        this.top.textContent = this[num];
        return;
      }
      this[num] += text;
      this.top.textContent = this[num];
    }
    updateNumber(text: string): void {
      if(this.option){
        this.checkNumber('n2', text);
      } else {
        this.checkNumber('n1', text);
      }
    }
    updateOption(text: string):void {
      if(this.n1 === ''){
        this.n1 = this.result;
      }
      this.option = text;
    }
    updateResult(): void {
      if(this.n1 === '' && this.n2 === '' && this.option === ''){
        this.top.textContent = this.result;
        return;
      }
      let result: number;
      if(this.option === '+') result = Number(this.n1) + Number(this.n2);
      if(this.option === '-') result = Number(this.n1) - Number(this.n2);
      if(this.option === '×') result = Number(this.n1) * Number(this.n2);
      if(this.option === '÷') result = Number(this.n1) / Number(this.n2);
      this.top.textContent = this.result = result.toString();
      if(result.toString().length > 9){
        this.top.textContent = this.result = result.toPrecision(9).toString().replace(/0+$/,'');
      }
      if(this.n2 === '0') this.top.textContent = '不是数字';
      this.n1 = '';
      this.n2 = '';
      this.option = '';
      
    }
    updateNumberAndOption(text: string): void{
      if('0123456789.'.indexOf(text) >= 0){
        this.updateNumber(text);
      } else if('+-×÷'.indexOf(text) >= 0){
        this.updateOption(text)
      } else if('='.indexOf(text) >= 0){
        this.updateResult();
      } else if('Clear' === text){
        this.n1 = '';
        this.n2 = '';
        this.option = '';
        this.top.textContent = this.result = '0';
      }
      console.log(this.n1, this.option, this.n2, this.result);
    }
    bindEvents(): void {
      this.wrap.addEventListener('click', (event): void => {
        if(event.target instanceof HTMLButtonElement){
          let button: HTMLButtonElement = event.target;
          let text: string = button.textContent;
          this.updateNumberAndOption(text);
        }
      })
    }
  }
  
  new Calculate()
```
#### 编译过后的JS：
```
var Calculate = /** @class */ (function () {
        function Calculate() {
            this.n1 = '';
            this.n2 = '';
            this.option = '';
            this.result = '';
            this.keys = [
                ['Clear', '÷'],
                ['7', '8', '9', '×'],
                ['4', '5', '6', '-'],
                ['1', '2', '3', '+'],
                ['0', '.', '=']
            ];
            this.createTopScreen();
            this.createBottomBar();
            this.createWrap();
            this.createAllButton();
            this.bindEvents();
        }
        Calculate.prototype.createSingleButton = function (text, wrap, className) {
            var button = document.createElement('button');
            button.textContent = text;
            button.classList.add('button');
            button.classList.add(className ? className : '');
            wrap.appendChild(button);
            return button;
        };
        Calculate.prototype.createTopScreen = function () {
            var top = document.createElement('div');
            top.classList.add('top-screen');
            top.textContent = '0';
            this.top = top;
        };
        Calculate.prototype.createBottomBar = function () {
            var bottom = document.createElement('div');
            bottom.classList.add('bottom-bar');
            this.bottomBar = bottom;
        };
        Calculate.prototype.createWrap = function () {
            var wrap = document.createElement('div');
            document.body.appendChild(wrap);
            wrap.classList.add('calc-wrap');
            wrap.appendChild(this.top);
            wrap.appendChild(this.bottomBar);
            this.wrap = wrap;
        };
        Calculate.prototype.createAllButton = function () {
            var _this = this;
            this.keys.map(function (v1) {
                var div = document.createElement('div');
                div.classList.add('button-bar');
                v1.map(function (v2) {
                    _this.createSingleButton(v2, div, "button-" + v2);
                });
                _this.bottomBar.appendChild(div);
            });
        };
        Calculate.prototype.checkNumber = function (num, text) {
            if (text === '.' && this[num].indexOf('.') >= 0) {
                this.top.textContent = this[num];
                return;
            }
            this[num] += text;
            this.top.textContent = this[num];
        };
        Calculate.prototype.updateNumber = function (text) {
            if (this.option) {
                this.checkNumber('n2', text);
            }
            else {
                this.checkNumber('n1', text);
            }
        };
        Calculate.prototype.updateOption = function (text) {
            if (this.n1 === '') {
                this.n1 = this.result;
            }
            this.option = text;
        };
        Calculate.prototype.updateResult = function () {
            if (this.n1 === '' && this.n2 === '' && this.option === '') {
                this.top.textContent = this.result;
                return;
            }
            var result;
            if (this.option === '+')
                result = Number(this.n1) + Number(this.n2);
            if (this.option === '-')
                result = Number(this.n1) - Number(this.n2);
            if (this.option === '×')
                result = Number(this.n1) * Number(this.n2);
            if (this.option === '÷')
                result = Number(this.n1) / Number(this.n2);
            this.top.textContent = this.result = result.toString();
            if (result.toString().length > 9) {
                this.top.textContent = this.result = result.toPrecision(9).toString().replace(/0+$/, '');
            }
            if (this.n2 === '0')
                this.top.textContent = '不是数字';
            this.n1 = '';
            this.n2 = '';
            this.option = '';
        };
        Calculate.prototype.updateNumberAndOption = function (text) {
            if ('0123456789.'.indexOf(text) >= 0) {
                this.updateNumber(text);
            }
            else if ('+-×÷'.indexOf(text) >= 0) {
                this.updateOption(text);
            }
            else if ('='.indexOf(text) >= 0) {
                this.updateResult();
            }
            else if ('Clear' === text) {
                this.n1 = '';
                this.n2 = '';
                this.option = '';
                this.top.textContent = this.result = '0';
            }
            console.log(this.n1, this.option, this.n2, this.result);
        };
        Calculate.prototype.bindEvents = function () {
            var _this = this;
            this.wrap.addEventListener('click', function (event) {
                if (event.target instanceof HTMLButtonElement) {
                    var button = event.target;
                    var text = button.textContent;
                    _this.updateNumberAndOption(text);
                }
            });
        };
        return Calculate;
    }());
    new Calculate();
```
#### html：
```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>calc</title>
  <style>
    :root{
      --em3: 3em;
    }
    .calc-wrap {
      display: inline-block;
    }
    .top-screen {
      height: 6rem;
      line-height: 6rem;
      background: rgb(42,40,42);
      text-align: right;
      color: #fff;
      font-size: 4.4em;
      padding-right: 1rem;
      border-top: 1.2rem solid rgb(42,40,42);
    }
    .button {
      height: var(--em3);
      width: var(--em3);
    }
    .button.button-Clear {
      width: 9em;
    }
    .button.button-0 {
      width: 6em;
    }
    .button-bar .button {
      background: rgb(226,224,226);
      background: linear-gradient(rgb(245,240,245), rgb(226,224,226));
      border-color: rgb(226,224,226);
      font-weight: bold;
      font-size: 40px;
    }
    .button-bar .button:last-child {
      background: rgb(255,151,58);
      background: linear-gradient(rgb(247,167,96), rgb(255,151,58));
      border-color: rgb(255,151,58);
      border-image: rgb(255,151,58);
      color: #fff;
    }
  </style>
</head>
<body>
  <script src="1.js"></script>
</body>
</html>
```