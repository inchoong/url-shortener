# url-shortener
Minimal URL shortener that can be entirely hosted on GitHub pages.
<hr style="height:1px;border:none;border-top:1px dashed #0066CC;">
<li><h1>黑科技：使用GitHub搭建自己的短链接服务</h1>
</li>
<div>
<br>
</div>
<div>
<p style="margin: 0px 0px 1.71429rem; padding: 0px; border: 0px; vertical-align: baseline; line-height: 1.71429; color: rgb(68, 68, 68); font-family: &quot;Open Sans&quot;, Helvetica, Arial, sans-serif;">前两天偶然在GitHub发现一个挺有意思的项目，可以不依赖自己的服务器、数据库来构建一个短链接服务。</p>
<p style="margin: 0px 0px 1.71429rem; padding: 0px; border: 0px; vertical-align: baseline; line-height: 1.71429; color: rgb(68, 68, 68); font-family: &quot;Open Sans&quot;, Helvetica, Arial, sans-serif;">自己尝试了一下，还挺简单的。这里记录一下自己的构建流程，感兴趣的小伙伴可以自己尝试一下。</p>
<h2 id="prerequisites" style="margin: 1.71429rem 0px; padding: 0px; border: 0px; font-size: 1.28571rem; vertical-align: baseline; clear: both; line-height: 1.6; color: rgb(68, 68, 68); font-family: &quot;Open Sans&quot;, Helvetica, Arial, sans-serif;">Prerequisites</h2>
<ol style="margin: 0px 0px 1.71429rem; padding: 0px; border: 0px; vertical-align: baseline; list-style-position: outside; list-style-image: initial; line-height: 1.71429; color: rgb(68, 68, 68); font-family: &quot;Open Sans&quot;, Helvetica, Arial, sans-serif;">
<li style="margin: 0px 0px 0px 2.57143rem; padding: 0px; border: 0px; vertical-align: baseline;">新建两个GitHub仓库，一个用来做服务器存储源码、提供服务(url_shortener)，一个用来做数据库存储链接(url_shortener_db)</li>
<li style="margin: 0px 0px 0px 2.57143rem; padding: 0px; border: 0px; vertical-align: baseline;">注册一个域名（可选），如果没有的话，可以直接使用GitHub pages的域名(<code style="margin: 0px; padding: 0px; border: 0px; font-size: 0.857143rem; vertical-align: baseline; font-family: Consolas, Monaco, &quot;Lucida Console&quot;, monospace; line-height: 2;">username.github.io</code>)。不过我是用了自己注册的域名：t.choong.net</li>
</ol>
<h2 id="_1" style="margin: 1.71429rem 0px; padding: 0px; border: 0px; font-size: 1.28571rem; vertical-align: baseline; clear: both; line-height: 1.6; color: rgb(68, 68, 68); font-family: &quot;Open Sans&quot;, Helvetica, Arial, sans-serif;">获取及配置源码</h2>
<p style="margin: 0px 0px 1.71429rem; padding: 0px; border: 0px; vertical-align: baseline; line-height: 1.71429; color: rgb(68, 68, 68); font-family: &quot;Open Sans&quot;, Helvetica, Arial, sans-serif;">首先，你需要获取这个服务的源代码，你可以直接fork这个<a href="https://github.com/nelsontky/gh-pages-url-shortener" style="margin: 0px; padding: 0px; border: 0px; vertical-align: baseline; outline: none; color: rgb(33, 117, 155);">源码仓库</a>，当然也欢迎fork我的<a href="https://github.com/inchoong/url-shortener" style="margin: 0px; padding: 0px; border: 0px; vertical-align: baseline; outline: none; color: rgb(33, 117, 155);">代码仓库</a>。</p>
<p style="margin: 0px 0px 1.71429rem; padding: 0px; border: 0px; vertical-align: baseline; line-height: 1.71429; color: rgb(68, 68, 68); font-family: &quot;Open Sans&quot;, Helvetica, Arial, sans-serif;">然后，克隆自己的仓库到本地(当然，你也可以直接在GitHub网页上操作)，修改<code style="margin: 0px; padding: 0px; border: 0px; font-size: 0.857143rem; vertical-align: baseline; font-family: Consolas, Monaco, &quot;Lucida Console&quot;, monospace; line-height: 2;">404.html</code>文件的<code style="margin: 0px; padding: 0px; border: 0px; font-size: 0.857143rem; vertical-align: baseline; font-family: Consolas, Monaco, &quot;Lucida Console&quot;, monospace; line-height: 2;">GITHUB_ISSUES_LINK</code>字段，指向自己的<code style="margin: 0px; padding: 0px; border: 0px; font-size: 0.857143rem; vertical-align: baseline; font-family: Consolas, Monaco, &quot;Lucida Console&quot;, monospace; line-height: 2;">url_shortener_db</code>仓库，这个仓库的<code style="margin: 0px; padding: 0px; border: 0px; font-size: 0.857143rem; vertical-align: baseline; font-family: Consolas, Monaco, &quot;Lucida Console&quot;, monospace; line-height: 2;">issues</code>就是作为存储你的链接的数据库：</p>
<div class="codehilite" style="margin: 0px; padding: 0px; border: 0px; vertical-align: baseline; background-image: initial; background-position: initial; background-size: initial; background-repeat: initial; background-attachment: initial; background-origin: initial; background-clip: initial; font-family: &quot;Open Sans&quot;, Helvetica, Arial, sans-serif;">
<pre style="margin-top: 1.71429rem; margin-bottom: 1.71429rem; padding: 1.71429rem; border: 1px solid rgb(237, 237, 237); font-size: 0.857143rem; vertical-align: baseline; font-family: Consolas, Monaco, &quot;Lucida Console&quot;, monospace; line-height: 1.71429; overflow: auto;">
<span style="margin: 0px; padding: 0px; border: 0px; font-size: 12px; vertical-align: baseline; color: rgb(102, 102, 102); border-style: initial; border-color: initial; border-image: initial;">
</span>
<code style="margin: 0px; padding: 0px; border: 0px; font-size: 0.857143rem; vertical-align: baseline; font-family: Consolas, Monaco, &quot;Lucida Console&quot;, monospace; line-height: 2; display: block;">
<span style="color: rgb(102, 102, 102);">var GITHUB_ISSUES_LINK = "https://api.github.com/repos</span><span style="color: rgb(255, 0, 0);">/<span style="font-weight: bold;">username/repo-name</span>/</span><span style="color: rgb(102, 102, 102);">issues/";</span>
</code>
</pre>
</div>
<p style="margin: 0px 0px 1.71429rem; padding: 0px; border: 0px; vertical-align: baseline; line-height: 1.71429; color: rgb(68, 68, 68); font-family: &quot;Open Sans&quot;, Helvetica, Arial, sans-serif;">
<em style="margin: 0px; padding: 0px; border: 0px; vertical-align: baseline;">注意把上面的/<code style="margin: 0px; padding: 0px; border: 0px; font-size: 0.857143rem; vertical-align: baseline; font-family: Consolas, Monaco, &quot;Lucida Console&quot;, monospace; line-height: 2;">username</code>/<code style="margin: 0px; padding: 0px; border: 0px; font-size: 0.857143rem; vertical-align: baseline; font-family: Consolas, Monaco, &quot;Lucida Console&quot;, monospace; line-height: 2;">repo-name/</code>替换为自己的用户名及仓库名。还有，<strong style="margin: 0px; padding: 0px; border: 0px; vertical-align: baseline;">最后的<code style="margin: 0px; padding: 0px; border: 0px; font-size: 0.857143rem; vertical-align: baseline; font-family: Consolas, Monaco, &quot;Lucida Console&quot;, monospace; line-height: 2;">/</code>不要漏掉</strong>，这是获取链接的关键</em>
</p>
<h2 id="github-pages" style="margin: 1.71429rem 0px; padding: 0px; border: 0px; font-size: 1.28571rem; vertical-align: baseline; clear: both; line-height: 1.6; color: rgb(68, 68, 68); font-family: &quot;Open Sans&quot;, Helvetica, Arial, sans-serif;">配置GitHub Pages</h2>
<p style="margin: 0px 0px 1.71429rem; padding: 0px; border: 0px; vertical-align: baseline; line-height: 1.71429; color: rgb(68, 68, 68); font-family: &quot;Open Sans&quot;, Helvetica, Arial, sans-serif;">接下来就是配置Github Pages了，这部分也很简单。</p>
<p style="margin: 0px 0px 1.71429rem; padding: 0px; border: 0px; vertical-align: baseline; line-height: 1.71429; color: rgb(68, 68, 68); font-family: &quot;Open Sans&quot;, Helvetica, Arial, sans-serif;">点击仓库的<code style="margin: 0px; padding: 0px; border: 0px; font-size: 0.857143rem; vertical-align: baseline; font-family: Consolas, Monaco, &quot;Lucida Console&quot;, monospace; line-height: 2;">Settings</code>选项，找到其中的<code style="margin: 0px; padding: 0px; border: 0px; font-size: 0.857143rem; vertical-align: baseline; font-family: Consolas, Monaco, &quot;Lucida Console&quot;, monospace; line-height: 2;">GitHub Pages</code>部分，然后配置<code style="margin: 0px; padding: 0px; border: 0px; font-size: 0.857143rem; vertical-align: baseline; font-family: Consolas, Monaco, &quot;Lucida Console&quot;, monospace; line-height: 2;">Source</code>中的分支： 下面的部分根据自己需要选择：</p>
<img src="https://github.com/inchoong/url-shortener/blob/main/img/img-1.jpg?raw=true" style="width: 1182px; height: 676px;" id="img_insert_164981452323107406928859817099" modifysize="48%" diffpixels="7px" scalingmode="zoom">
<br>
<p style="margin: 0px 0px 1.71429rem; padding: 0px; border: 0px; vertical-align: baseline; line-height: 1.71429; color: rgb(68, 68, 68); font-family: &quot;Open Sans&quot;, Helvetica, Arial, sans-serif;">
<br>
</p>
<h3 id="_2" style="margin: 1.71429rem 0px; padding: 0px; border: 0px; font-size: 1.14286rem; vertical-align: baseline; clear: both; line-height: 1.84615; color: rgb(68, 68, 68); font-family: &quot;Open Sans&quot;, Helvetica, Arial, sans-serif;">如果你不使用自己的域名</h3>
<ol style="margin: 0px 0px 1.71429rem; padding: 0px; border: 0px; vertical-align: baseline; list-style-position: outside; list-style-image: initial; line-height: 1.71429; font-family: &quot;Open Sans&quot;, Helvetica, Arial, sans-serif;">
<li style="color: rgb(68, 68, 68); margin: 0px 0px 0px 2.57143rem; padding: 0px; border: 0px; vertical-align: baseline;">删除仓库中的<code style="margin: 0px; padding: 0px; border: 0px; font-size: 0.857143rem; vertical-align: baseline; font-family: Consolas, Monaco, &quot;Lucida Console&quot;, monospace; line-height: 2;">CNAME</code>文件</li>
<li style="margin: 0px 0px 0px 2.57143rem; padding: 0px; border: 0px; vertical-align: baseline;">
<span style="color: rgb(68, 68, 68);">把</span>
<code style="margin: 0px; padding: 0px; border: 0px; font-size: 0.857143rem; vertical-align: baseline; font-family: Consolas, Monaco, &quot;Lucida Console&quot;, monospace; line-height: 2; color: rgb(255, 0, 0); font-weight: bold;">404.html</code>
<span style="color: rgb(68, 68, 68);">文件中的</span>
<code style="color: rgb(68, 68, 68); margin: 0px; padding: 0px; border: 0px; font-size: 0.857143rem; vertical-align: baseline; font-family: Consolas, Monaco, &quot;Lucida Console&quot;, monospace; line-height: 2;">var PATH_SEGMENTS_TO_SKIP = 0;</code>
<span style="color: rgb(68, 68, 68);">改为</span>
<code style="color: rgb(68, 68, 68); margin: 0px; padding: 0px; border: 0px; font-size: 0.857143rem; vertical-align: baseline; font-family: Consolas, Monaco, &quot;Lucida Console&quot;, monospace; line-height: 2;">var PATH_SEGMENTS_TO_SKIP = 1;</code>
</li>
<li style="color: rgb(68, 68, 68); margin: 0px 0px 0px 2.57143rem; padding: 0px; border: 0px; vertical-align: baseline;">到这里就结束了，你可以直接跳到<span style="border-style: initial; border-color: initial; border-image: initial; outline-color: initial; outline-width: initial; color: rgb(33, 117, 155);">下一部分.</span>
</li>
</ol>
<h3 id="_3" style="margin: 1.71429rem 0px; padding: 0px; border: 0px; font-size: 1.14286rem; vertical-align: baseline; clear: both; line-height: 1.84615; color: rgb(68, 68, 68); font-family: &quot;Open Sans&quot;, Helvetica, Arial, sans-serif;">如果你用了自己的域名</h3>
<ol style="margin: 0px 0px 1.71429rem; padding: 0px; border: 0px; vertical-align: baseline; list-style-position: outside; list-style-image: initial; line-height: 1.71429; font-family: &quot;Open Sans&quot;, Helvetica, Arial, sans-serif;">
<li style="margin: 0px 0px 0px 2.57143rem; padding: 0px; border: 0px; vertical-align: baseline;">
<span style="color: rgb(68, 68, 68);">到你的域名服务商，添加一个CNAME解析，域名指向</span>
<code style="margin: 0px; padding: 0px; border: 0px; font-size: 0.857143rem; vertical-align: baseline; font-family: Consolas, Monaco, &quot;Lucida Console&quot;, monospace; line-height: 2;">
<span style="color: rgb(255, 0, 0); font-weight: bold;">username</span>
<span style="color: rgb(68, 68, 68);">.github.io</span>
</code>
<span style="color: rgb(68, 68, 68);">，注意替换</span>
<code style="margin: 0px; padding: 0px; border: 0px; font-size: 0.857143rem; vertical-align: baseline; font-family: Consolas, Monaco, &quot;Lucida Console&quot;, monospace; line-height: 2; font-weight: bold; color: rgb(255, 0, 0);">username</code>
<span style="color: rgb(68, 68, 68);">为自己的GitHub用户名</span>
</li>
<li style="color: rgb(68, 68, 68); margin: 0px 0px 0px 2.57143rem; padding: 0px; border: 0px; vertical-align: baseline;">等待几分钟（域名解析生效需要时间，所以最好提前做），再进入到github pages部分，键入你自己的域名，最好勾选__Enforce HTTPS__，然后点击<code style="margin: 0px; padding: 0px; border: 0px; font-size: 0.857143rem; vertical-align: baseline; font-family: Consolas, Monaco, &quot;Lucida Console&quot;, monospace; line-height: 2;">Save</code>，刷新页面，会显示下面的内容:&nbsp;<br>
<img src="https://github.com/inchoong/url-shortener/blob/main/img/img-2.jpg?raw=true" style="width: 888px; height: 516px;" id="img_insert_164981481355605010207404603817" modifysize="36%" diffpixels="3px" scalingmode="zoom">
<br>
<br>
</li>
<li style="color: rgb(68, 68, 68); margin: 0px 0px 0px 2.57143rem; padding: 0px; border: 0px; vertical-align: baseline;">把<code style="margin: 0px; padding: 0px; border: 0px; font-size: 0.857143rem; vertical-align: baseline; font-family: Consolas, Monaco, &quot;Lucida Console&quot;, monospace; line-height: 2;">CNAME</code>文件中的域名更改为你配置好的域名</li>
<li style="color: rgb(68, 68, 68); margin: 0px 0px 0px 2.57143rem; padding: 0px; border: 0px; vertical-align: baseline;">到此，就配置完成了，接下来可以测试一下</li>
</ol>
<h2 id="_4" style="margin: 1.71429rem 0px; padding: 0px; border: 0px; font-size: 1.28571rem; vertical-align: baseline; clear: both; line-height: 1.6; color: rgb(68, 68, 68); font-family: &quot;Open Sans&quot;, Helvetica, Arial, sans-serif;">测试效果</h2>
<ol style="margin: 0px 0px 1.71429rem; padding: 0px; border: 0px; vertical-align: baseline; list-style-position: outside; list-style-image: initial; line-height: 1.71429; color: rgb(68, 68, 68); font-family: &quot;Open Sans&quot;, Helvetica, Arial, sans-serif;">
<li style="margin: 0px 0px 0px 2.57143rem; padding: 0px; border: 0px; vertical-align: baseline;">
<p style="margin: 0px 0px 1.71429rem; padding: 0px; border: 0px; vertical-align: baseline; line-height: 1.71429;">在你开始作为数据库的GitHub仓库创建一个issue，标题就是你需要缩短的链接，其他什么都不需要做，直接Submit就行了。</p>
</li>
<li style="margin: 0px 0px 0px 2.57143rem; padding: 0px; border: 0px; vertical-align: baseline;">
<p style="margin: 0px 0px 1.71429rem; padding: 0px; border: 0px; vertical-align: baseline; line-height: 1.71429;">等待一会儿，然后在浏览器查询链接<code style="margin: 0px; padding: 0px; border: 0px; font-size: 0.857143rem; vertical-align: baseline; font-family: Consolas, Monaco, &quot;Lucida Console&quot;, monospace; line-height: 2;">https://your-domain/issue-no;</code>。例如：<a href="https://t.choong.net/2" style="margin: 0px; padding: 0px; border: 0px; font-size: 0.857143rem; vertical-align: baseline; font-family: Consolas, Monaco, &quot;Lucida Console&quot;, monospace; line-height: 2;">https://t.choong.net/2</a>，点击这个链接会跳转到#<a href="https://to-do.microsoft.com/tasks/myday" title="微软待办">微软待办</a>。其中的<code style="margin: 0px; padding: 0px; border: 0px; font-size: 0.857143rem; vertical-align: baseline; font-family: Consolas, Monaco, &quot;Lucida Console&quot;, monospace; line-height: 2;">t.choong.net</code>是我的域名，<code style="margin: 0px; padding: 0px; border: 0px; font-size: 0.857143rem; vertical-align: baseline; font-family: Consolas, Monaco, &quot;Lucida Console&quot;, monospace; line-height: 2;">1</code>代表issue的楼层数。</p>
</li>
</ol>
<h2 id="_5" style="margin: 1.71429rem 0px; padding: 0px; border: 0px; font-size: 1.28571rem; vertical-align: baseline; clear: both; line-height: 1.6; color: rgb(68, 68, 68); font-family: &quot;Open Sans&quot;, Helvetica, Arial, sans-serif;">总结</h2>
<p style="margin: 0px 0px 1.71429rem; padding: 0px; border: 0px; vertical-align: baseline; line-height: 1.71429; color: rgb(68, 68, 68); font-family: &quot;Open Sans&quot;, Helvetica, Arial, sans-serif;">如果大家觉得麻烦，也可以使用网友搭建的服务，你可以点击<a href="https://github.com/baddate/url_shortener_db/issues/new" style="margin: 0px; padding: 0px; border: 0px; vertical-align: baseline; outline: none; color: rgb(33, 117, 155);">这里</a>提供链接。</p>
<p style="margin: 0px 0px 1.71429rem; padding: 0px; border: 0px; vertical-align: baseline; line-height: 1.71429; color: rgb(68, 68, 68); font-family: &quot;Open Sans&quot;, Helvetica, Arial, sans-serif;">有人在<a href="https://github.com/nelsontky/gh-pages-url-shortener/issues/5#issuecomment-728040879" style="margin: 0px; padding: 0px; border: 0px; vertical-align: baseline; outline: none; color: rgb(33, 117, 155);">这里</a>给了一个比较好的解释，大家可以参考一下。</p>
<h2 id="reference" style="margin: 1.71429rem 0px; padding: 0px; border: 0px; font-size: 1.28571rem; vertical-align: baseline; clear: both; line-height: 1.6; color: rgb(68, 68, 68); font-family: &quot;Open Sans&quot;, Helvetica, Arial, sans-serif;">REFERENCE</h2>
<p style="margin: 0px 0px 1.71429rem; padding: 0px; border: 0px; vertical-align: baseline; line-height: 1.71429; color: rgb(68, 68, 68); font-family: &quot;Open Sans&quot;, Helvetica, Arial, sans-serif;">
<a href="https://github.com/nelsontky/gh-pages-url-shortener" style="margin: 0px; padding: 0px; border: 0px; vertical-align: baseline; outline: none; color: rgb(33, 117, 155);">https://github.com/nelsontky/gh-pages-url-shortener</a>
</p>
<p style="margin: 0px 0px 1.71429rem; padding: 0px; border: 0px; vertical-align: baseline; line-height: 1.71429; color: rgb(68, 68, 68); font-family: &quot;Open Sans&quot;, Helvetica, Arial, sans-serif;">
<a href="https://blog.tldr.plus/post/2021/6/20/12.html">黑科技：使用GitHub搭建自己的短链接服务 | 折木的方寸之地 (tldr.plus)</a>
</p>
</div>
