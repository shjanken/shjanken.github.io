`openstack` 的控制节点
====
`keystone` 的配置安装 -----


##### 安装 

1. 安装`keystone` 需要安装 `mysql` 。如果没有安装 `mysql` 则安装 `keystone` 之后，启动时会提示。
2. `keystone` 的三个组件分别是：`openstack-utils`, `openstack-keystone`, `python-keystoneclient` 

##### 配置

1. 创建 `keystone` 数据库， 其默认会创建一个 `keystone` 用户访问此同名数据库， 密码可以用 `--pass` 来指定。

```sh
# openstack-db --init --service keystone --pass keystone
```

2. 配置 `keystone` 的主配置文件， 使其使用 `mysql` 作为数据库存储池， 并配置使用正确的参数。

```sh
# vim /etc/keystone/keystone.conf
```

    在配置文件中的 [sql] 段中，确保与`mysql` 相关内容如下， 注意其中密码为 `keystone` 用户访问 `mysql` 服务器所使用的密码

```
connection = mysql://keystone:keystone@localhost/keyston
```

3. 配置 `keystone` 的管理 `token` 

```sh
# export SERVICE_TOKEN=`openssl rand -hex 10`
# export SERVICE_ENDPOINT=http://172.168.200.6:35357/v2.0
# $SERVICE_TOKEN > ~/ks_admin_token //将生成的随即数保存到文件中备用
```

    之后使用
        
        openstack-config --set /etc/keystone/keystone.conf DEFAULT admin_token 'YOUR TOKEN ADMIN'
    
    来修改配置文件中的相关配置，（也可以直接修改配置文件的 `admin_token = number` 使其神效）

4. 启动服务
        
        service openstack-keyston start

5. 初始化 `keystone` 数据库

        keystone-manager db_sync

6. 设定 `keystone` 为 `API endpoint` ，

`API endpoint`： `openstack` 中的每一个服务都需要 `keystone` 作为一个端点，将自己的信息保存在`keystone`中。比如:如果管理器需要访问一个节点，则必须要通过`keystone`来获取节点的地址位置。

        # keystone service-create --name=keystone --type=indentity --description="Keystone Identiy Service"
        # keystone servoce-list //查看当前keystone的服务
        # keystone endpoint-create \
          --service_id service id \
          --publicurl 'http://172.16.200.6:5000/v2.0' \ //给公网使用
          --adminurl 'http://172.16.200.6:35357/v2.0' \ //给管理员使用
          --internalurl 'http://172.16.200.6:5000/v2.0' \ //给内部使用


7. 创建 `user` ， `role` 和 `tenant`

`tenant` 是 `openstack` 的 `keystone` 中的一个重要术语。他相当于一个特定项目（project）或一个特定组织（origaniztion)等。他实现了资源或identity 对象的隔离，用户（user）在认证通过后就可以访问资源，其通常会直接关联至 `tenant` ，因此看起来就像用户是包含于 `tenant` 中的。
