# gfs-react-redux-twoway-binding

数据双向绑定，在React Component中直接改变redux store中数据对象值，达到重新渲染页面的目的，减少编写更多的action；使用时请注意store数据对象必须是immutable类型且次组件是一个装饰类（decorator）

## 安装

```bash

	npm install gfs-react-redux-twoway-binding
```

## 依赖

* `immutable`

## 使用			

### 1. createReducer

创建一个redux store；

```js

	import { createReducer } from 'gfs-react-redux-twoway-binding';
    import Immutable from 'immutable';

    const initialState = Immutable.fromJS({
        name:'init'
    });

    export const test = createReducer('test',initialState, {
        ['QUERY']: (data, action) => {
            return data.merge(Immutable.fromJS(action.data));
        }
    });
```

### 2. setStore

组件与具体的store建立关系；

```js

	import {bindingMixin} from 'gfs-react-redux-twoway-binding';

	//用次组件装饰下面的类
    @bindingMixin
    export default class TestWebContainer extends Component {
        constructor(props) {
            super(props);

            //关联store
            this.setBinding('test');

        }
    }
```

### 3. binding

数据双向绑定，使用`this.binding`方法与具体字段绑定，表单元素发生改变时将直接会改变store中的值；

```js

	render() {
		return (
			<div>
				<input type="text"  valueLink={this.binding('name')  } />
				{this.props.test.get('name') }
			</div>
		);
	}
```

## Manual Change Functions

帮助用户快速根据reducer路径更改store值，避免写更多的action。

* 手动根据路径改变reducer的值:

```js

	this.manualChange('name', 'john');

	this.manualChange('name', function(age){
        return ++age;
    });
```

## Command

```
	#测试	
	npm run test	
	#打包	
	npm run build	
	#例子演示	
	npm start
```


