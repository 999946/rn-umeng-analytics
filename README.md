# rn-umeng-analytics
##安装
```
npm install rn-umeng-analytics
rnpm link rn-umeng-analytics
```

## 简介
其实这个库代码都是友盟官方提供的，此库只是方便安装而已。
官方SDK下载地址：http://dev.umeng.com/analytics/h5/sdk-download

官方集成文档地址：http://dev.umeng.com/analytics/h5/react-native%E9%9B%86%E6%88%90%E6%96%87%E6%A1%A3

## 使用
```
import UMNative from 'react-native-umeng-analytics';

UMNative.onPageBegin("pageA");
UMNative.onPageEnd("pageA");
```