# FAQ

## Contents
- [如何修改Git过去提交的message？](#reset_commited_message)
- [Windows7如何在CopSSH中新建Git远程仓库？](#create_git_repository_on_copssh)
- [Git乱码解决方案？](#git_messy_code_problem)
- [Git如何为远程仓库(origin)设置push默认分值(branch)？](#set_default_push_stream_in_git)
- [Hibernate中如何避免@OneToMany生成连接表](#hibernate_avoid_create_join_table_4_one_to_many)
- [MySQL Connection Error: Host  is not allowed to connect to this MySQL server](#mysql_access_permission)
- [Gradle web project convert to Eclipse web project](#build_gradle_project_4_eclipse)
- [Vim基本配置](#basic_vim_settings)
- [JPA中设置@ElementCollection元素为有序数组](#jpa_element_collection_as_ordered_array)
- [如何根据域名设置多个SSH-Key？](#config_ssh_key_4_multi_hosts)
- [Jetty中如何设置URIEncoding？](#set_jetty_uri_encoding)
	

### <a name="reset_commited_message"/> Q：如何修改Git过去提交的message？

1. 运行 **git rebase -i ( message hash | HEAD~n)**，选择要修改的message行，将pick改为edit，保存退出；
2. 运行 **git commit -amend -m "new message"**，将新的message提交；
3. 运行 **git rebase --continue**提交修改；
4. 如果已提交到远程仓库，需要运行 **git push <repo> <branch> --force**更新远程git仓库的message。

####References:
https://help.github.com/articles/changing-a-commit-message/


### <a name="create_git_repository_on_copssh"/> Q：Windows7如何在CopSSH中新建Git远程仓库？

1. 进入CopSSH/home/**{user}**目录下，运行 **git init --bare {repo};**；
2. 右键点击“**{repo}**”目录，选择属性->安全->编辑->添加，添加**{user}**；
3. 设置**{user}**权限为：**修改**，**读取和执行**；

> 如果在安全选项卡未为**{repo}**添加**{user}**或设置权限，在提交源码到远程仓库时将显示如下错误信息：
> ![insufficient permission](etc/insufficient-permission.jpg)

#### References:
https://github.com/msysgit/msysgit/wiki/Setting-up-a-Git-server-on-Windows-using-Git-for-Windows-and-CopSSH


### <a name="git_messy_code_proble"/> Q：Git乱码解决方案？

运行git-bash命令，依次输入如下命令：
```bash
git config --global core.quotepath false          # 显示 status 编码
git config --global gui.encoding utf-8            # 图形界面编码
git config --global i18n.commit.encoding utf-8    # 提交信息编码
git config --global i18n.logoutputencoding (utf-8|gbk)  # 输出 log 编码
export LESSCHARSET=utf-8
```

#### References:
http://howiefh.github.io/2014/10/11/git-encoding/


### <a name="set_default_push_stream_in_git"/> Q：Git如何为远程仓库(origin)设置push默认分值(branch)？

运行git-bash命令，依次输入如下命令：
```bash
git push --set-upstream {repo} {branch}
```


### <a name="hibernate_avoid_create_join_table_4_one_to_many"/> Q：Hibernate中如何避免@OneToMany生成连接表

将@OneToMany中的mappedBy设置为多的一端，如下例所示：

```java
@ManyToOne
private Node parent;

@OneToMany(mappedBy = "parent")
private Set<Node> children = new HashSet<>();
```

### <a name="mysql_access_permission"/> Q：MySQL Connection Error: Host  is not allowed to connect to this MySQL server

运行mysql命令行程序，输入以下sql语句，更新用户访问权限

```sql
  use mysql;
  update user set host = '%' where user = 'root';
  flush privileges;
```

### <a name="build_gradle_project_4_eclipse"/> Q：Gradle web project convert to Eclipse web project

在build.gradle中添加如下配置信息

```groovy
apply plugin: 'eclipse'

eclipse {
    classpath {
        downloadSources = false
    }
}

import org.gradle.plugins.ide.eclipse.model.Facet
apply plugin: 'eclipse-wtp'
eclipse {
    wtp {
        facet {
            facet name: 'jst.web', type: Facet.FacetType.fixed
            facet name: 'wst.jsdt.web', type: Facet.FacetType.fixed
            facet name: 'jst.java', type: Facet.FacetType.fixed
            facet name: 'jst.web', version: '3.0'
            facet name: 'jst.java', version: '1.7'
            facet name: 'wst.jsdt.web', version: '1.0'
        }
    }
}
```
#### References:
http://jameskaron.iteye.com/blog/2250079


### <a name="basic_vim_settings"/> Q: Vim基本配置

```bash
colors zellner
syntax on
:set number
set ts=2
set sw=2
set fileencodings=ucs-bom,utf-8,chinese
set fileencoding=utf-8
```


### <a name="jpa_element_collection_as_ordered_array"/> Q: JPA中设置@ElementCollection元素为有序数组

```java
@ElementCollection(fetch = FetchType.LAZY)
@OrderColumn(name = "nth")
private List<Attribute> attributes;
```


### <a name="config_ssh_key_4_multi_hosts"/> Q: 如何根据域名设置多个SSH-Key？

创建或修改 ` ~/.ssh/config ` 文件，添加如下配置

```yaml
Host me.github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/me_rsa

Host work.github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/work_rsa
```

#### References:

http://stackoverflow.com/questions/3225862/multiple-github-accounts-ssh-config

### <a name="set_jetty_uri_encoding"/> Q: Jetty中如何设置URIEncoding？
```java
System.setProperty("org.mortbay.util.URI.charset", "UTF-8");
```
