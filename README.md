# HELP(https://hexo.io/zh-cn/docs/)

## 启动服务器

hexo server

## 生成静态文件

hexo generate / hexo g

## 发布

hexo deploye / hexo d

## 上传搜索

    npm install --save hexo-algolia
    

    [1]站点配置： 
        algolia:
            applicationID: TMOKSN5WPG
            apiKey: 4a5991d5fa55b319b0fa5683e8f9391b
            indexName: Notes
    [2]环境变量设置
        export HEXO_ALGOLIA_INDEXING_KEY=Admin API Key
    [3]执行命令更新INDEX
        hexo algolia
        // --flush不是第一次提交设置为true
        // --layouts page/post
        hexo algolia --flush true --layouts post