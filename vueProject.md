# 项目设计与原理分析

## css模块化设计

设计方法    
1.先整体后部分再颗粒化    
  布局，页面，功能，业务   

设计原则    
1.可复用能继承要完整   
2.周期性迭代   
  reset.css  layout.css,  elsement.css    
  
## js模块化设计

设计原则    
1.高内聚低耦合    
2.周期性迭代     

设计方法    
1.先整体后部分再颗粒化    
2.尽可能的抽象    

## 自适应
1.基本概念    
  css像素，设备像素，逻辑像素，设备像素比   
  viewport    
  rem   
2.工作原理    
  利用viewport和设备像素比调整基准像素    
  利用px2rem自动转换css单位   

## SPA设计

1.设计意义    
  前后端分离   
  减轻服务器压力   
  增强用户体验    
  Prerender预渲染优化SEO   
2.工作原理    
  History API   
  hash    
![images](https://github.com/yusisi/codeStudy/blob/master/images/12.png)
