# xml2jianpu

这个项目用来将musicxml文件转换为简谱（以三色绘恋为例）。暂不支持重复、跳跃类符号。  
由于在网上找不到三色绘恋的简谱，所以就自己做了一个简单版的。效果图如下。
![image](https://user-images.githubusercontent.com/120734145/208229550-b62d8c66-f729-4e9c-8f9a-1ac172a2b828.png)  
首先，从 https://musescore.org/zh-hans 下载musescore（一个制谱软件）。然后自己做一个五线谱，导出为不压缩的musicxml格式。  
![image](https://user-images.githubusercontent.com/120734145/208136371-0fdded23-b1de-4388-8392-9f64795eaab0.png)   
该项目需要用npm包管理工具，依赖项在package.json文件中已给出，执行命令 `npm install`  
依赖项添加完成后再执行`npm run serve`  
待项目构建完成后，在 http://localhost:8080 端口即可看到项目效果。

也可以在网页[https://nkufree.github.io/xml2jianpu/](https://nkufree.github.io/xml2jianpu/)在线查看效果。
