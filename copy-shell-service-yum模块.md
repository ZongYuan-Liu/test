# 模块

### yum模块

* name：要操作的软件包名字，可以是一个url或者本地rpm包路径
* update_cache：更新软件包缓存
* state：具体操作，
	* state=installed
	* state=removed

### copy模块

* src：文件源路径，文件夹后面加/表示的是选择文件夹内容。不加/表示文件夹本身
* content ：文本内容（和src二选一否则报错）
* dest:目标路径
* backup：是否备份 yes或者no
* owner：设置文件属主（不能指定不存在的用户）
* group：设置文件数组（不能指定不存在的组）
* mode：设置文件权限，和chmod使用方法类似，
* directory_mode：递归设置目录权限，默认为系统权限
* force：如果目标主机包含该文件，内容不同，设置为yes表示强制覆盖，设置no表示当目标位置不存在该文件时才复制。默认为yes

### service模块

* enabled：是否开机自启
* name：操作的服务名
* state：具体操作
	* started
	* stoped
	* restarted

### shell

* 直接指定shell命令

### fetch

从被控主机拉去文件
* src ：源文件路径
* dest：目标路径

### file

更改被控主机文件
* path ：操作的文件
* src：创建链接文件时指定的源文件
* state：创建文件的类型
	* touch （表示创建文件）
	* directory （表示创建目录）
	* link （表示创建软链接）
	* hard （表示创建硬链接）
	* absent（表示删除文件）
* owner：修改属主
* group：修改数组
* mode：修改权限
