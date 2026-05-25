题目提示：`发现网页有个错别字？赶紧在生产环境vim改下，不好，死机了`

推测是 Vim 的缓存文件，之前做过但是忘了，搜索得知 Vim 的缓存文件以`.swp`、`.swo`、`.swn`等后缀结尾，一般来说网站的首页是 `index.html` 或者 `index.php`，逐个组合尝试访问发现访问 `/index.php.swp` 时直接下载文件，打开后获得 flag `ctfshow{4920333f-7ca8-43f4-99b2-cb63fe5266cf}`