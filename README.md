Hands on Lab :-
Divide into 4 Part -
PART 1 : Package installation / Creating User / Service / Role / Endpoints for Manila.
PART 2 : Database Creation / Basic Manila configuration / Populating the database for Manila.
PART 3 : Configure Manila Share.
PART 4 : Creating manila-share and to work as a filesystem
Step 1 : Install required rpm
# yum install -y openstack-manila python-manila python-manilaclient openstack-manila-share python-manila
openstack-utils.noarch
Step 2 : Add a user for Manila
openstack user create --password manila --email manila@192.168.122.215 manila
~~~
[root@manila ~(keystone_admin)]# openstack user create --password manila --email manila@192.168.122.215 manila
+----------+----------------------------------+
| Field | Value |
+----------+----------------------------------+
| email | manila@192.168.122.215 |
| enabled | True |
| id | 0fe37c5cb2f24a72a7cc6f1b04ac7afb |
| name | manila |
| username | manila |
+----------+----------------------------------+
[root@manila ~(keystone_admin)]#
~~~
Step 3 : Add manila user to admin role
openstack role add --project services --user manila admin
~~~
[root@manila ~(keystone_admin)]# openstack role add --project services --user manila admin
+-----------+----------------------------------+
| Field | Value |
+-----------+----------------------------------+
| domain_id | None |
| id | beeb806cc1944e99a5c2fe02dd8107ad |
| name | admin |
+-----------+----------------------------------+-----------+----------------------------------+
[root@manila ~(keystone_admin)]#
~~~
Step 4 : Add service entry for manila for v1 and v2 both
openstack service create --name manila --description "OpenStack Shared Filesystems" share
~~~
[root@manila ~(keystone_admin)]# openstack service create --name manila --description "OpenStack Shared
Filesystems" share
+-------------+----------------------------------+
| Field | Value |
+-------------+----------------------------------+
| description | OpenStack Shared Filesystems |
| enabled | True |
| id | fc9db2e2fc4a4f9495373536c006adb4 |
| name | manila |
| type | share |
+-------------+----------------------------------+
[root@manila ~(keystone_admin)]#
~~~
for v2 :-
openstack service create --name manilav2 --description "OpenStack Shared Filesystems V2" sharev2
~~~
[root@manila ~(keystone_admin)]# openstack service create --name manilav2 --description "OpenStack Shared
Filesystems V2" sharev2
+-------------+----------------------------------+
| Field | Value |
+-------------+----------------------------------+
| description | OpenStack Shared Filesystems V2 |
| enabled | True |
| id | a41b13f6e7dc4ddbb9e93702e5707797 |
| name | manilav2 |
| type | sharev2 |
+-------------+----------------------------------+
[root@manila ~(keystone_admin)]#
~~~
Step 5 : Add endpoint for manila (public/internal & admin) for both v1 and v2
openstack endpoint create --publicurl 'http://192.168.122.215:8786/v1/%(tenant_id)s' \
--internalurl 'http://192.168.122.215:8786/v1/%(tenant_id)s' \
--adminurl 'http://192.168.122.215:8786/v1/%(tenant_id)s' \
--region RegionOne \
share
~~~
[root@manila ~(keystone_admin)]# openstack endpoint create --publicurl
'http://192.168.122.215:8786/v1/%(tenant_id)s' \
> --internalurl 'http://192.168.122.215:8786/v1/%(tenant_id)s' \
> --adminurl 'http://192.168.122.215:8786/v1/%(tenant_id)s' \
> --region RegionOne \
> share
+--------------+----------------------------------------------+
| Field | Value |
+--------------+----------------------------------------------+
| adminurl | http://192.168.122.215:8786/v1/%(tenant_id)s |
| id | 1442373bab3e4fd5b1745f2c4e9ea681 |
| internalurl | http://192.168.122.215:8786/v1/%(tenant_id)s |
| publicurl | http://192.168.122.215:8786/v1/%(tenant_id)s |
| region | RegionOne |
| service_id | fc9db2e2fc4a4f9495373536c006adb4 |
| service_name | manila |
| service_type | share |
+--------------+----------------------------------------------+
[root@manila ~(keystone_admin)]#
~~~
openstack endpoint create --publicurl 'http://192.168.122.215:8786/v2/%(tenant_id)s' \
--internalurl 'http://192.168.122.215:8786/v2/%(tenant_id)s' \
--adminurl 'http://192.168.122.215:8786/v2/%(tenant_id)s' \
--region RegionOne \
sharev2
~~~
[root@manila ~(keystone_admin)]# openstack endpoint create --publicurl
'http://192.168.122.215:8786/v2/%(tenant_id)s' \
> --internalurl 'http://192.168.122.215:8786/v2/%(tenant_id)s' \
> --adminurl 'http://192.168.122.215:8786/v2/%(tenant_id)s' \
> --region RegionOne \
> sharev2
+--------------+----------------------------------------------+
| Field | Value |
+--------------+----------------------------------------------+
| adminurl | http://192.168.122.215:8786/v2/%(tenant_id)s |
| id | a868b380b7d74a838d4def9d0e3071e6 |
| internalurl | http://192.168.122.215:8786/v2/%(tenant_id)s |
| publicurl | http://192.168.122.215:8786/v2/%(tenant_id)s |
| region | RegionOne |
| service_id | a41b13f6e7dc4ddbb9e93702e5707797 |
| service_name | manilav2 |
| service_type | sharev2 |
+--------------+----------------------------------------------+
[root@manila ~(keystone_admin)]#
~~~
Step 6 : Verify the endpoints :-
~~~
[root@manila ~(keystone_admin)]# openstack endpoint list
+----------------------------------+-----------+--------------+--------------+
| ID | Region | Service Name | Service Type |
+----------------------------------+-----------+--------------+--------------+
| aade22815d7d49ca8c3bbba117cdba89 | RegionOne | swift | object-store |
| bc25f2f45fea4d3ebccb2712940971f2 | RegionOne | aodh | alarming |
| 1442373bab3e4fd5b1745f2c4e9ea681 | RegionOne | manila | share |
| 3145fe3c67d84dbfb502689d5ebc6a08 | RegionOne | cinderv3 | volumev3 |
| 74fd9879d7b04b0abacfb1831cafcdaf | RegionOne | gnocchi | metric |
| 8adb5a00c281408a9971cd0ab2d1e536 | RegionOne | keystone | identity |
| 6186ac272f7643da9ab13e34406bf1bf | RegionOne | glance | image |
| f84a8b064a4f4adf92ec1a38ba682fd0 | RegionOne | ceilometer | metering |
| 27fae5b7bc2f4dfcb085d3f858282eb5 | RegionOne | cinderv2 | volumev2 |
| a868b380b7d74a838d4def9d0e3071e6 | RegionOne | manilav2 | sharev2 |
| 4b3f590d55a24788ab85c50be369bd88 | RegionOne | nova | compute |
| c02b5cc27e434e20919f30803bed1b80 | RegionOne | cinder | volume |
| 1f19bd4a7fb84b9d98c703e74771d925 | RegionOne | neutron | network |
+----------------------------------+-----------+--------------+--------------+
[root@manila ~(keystone_admin)]#
~~~
~~~
[root@manila ~(keystone_admin)]# openstack catalog list
| | | |
| manila | share | RegionOne |
| | | publicURL: http://192.168.122.215:8786/v1/6e43691bcb57453fbf4a1c1004531d0b |
| | | internalURL: http://192.168.122.215:8786/v1/6e43691bcb57453fbf4a1c1004531d0b |
| | | adminURL: http://192.168.122.215:8786/v1/6e43691bcb57453fbf4a1c1004531d0b |
| | | |
| | | |
| manilav2 | sharev2 | RegionOne |
| | | publicURL: http://192.168.122.215:8786/v2/6e43691bcb57453fbf4a1c1004531d0b |
| | | internalURL: http://192.168.122.215:8786/v2/6e43691bcb57453fbf4a1c1004531d0b |
| | | adminURL: http://192.168.122.215:8786/v2/6e43691bcb57453fbf4a1c1004531d0b |
| | | |
~~~
PART 2 : Database Creation / Basic Manila configuration / Populating the database for Manila.
Step 1 : Database creation and granting permission
mysql
CREATE DATABASE manila;
GRANT ALL PRIVILEGES ON manila.* TO 'manila'@'192.168.122.215' IDENTIFIED BY 'manila';
GRANT ALL PRIVILEGES ON manila.* TO 'manila'@'%' IDENTIFIED BY 'manila';
EXIT;
~~~
[root@manila ~(keystone_admin)]# mysql
Welcome to the MariaDB monitor. Commands end with ; or \g.
Your MariaDB connection id is 39
Server version: 5.5.42-MariaDB-wsrep MariaDB Server, wsrep_25.11.r4026
Copyright (c) 2000, 2016, Oracle, MariaDB Corporation Ab and others.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
MariaDB [(none)]> CREATE DATABASE manila;
Query OK, 1 row affected (0.00 sec)
MariaDB [(none)]> GRANT ALL PRIVILEGES ON manila.* TO 'manila'@'192.168.122.215' IDENTIFIED BY
'manila';
Query OK, 0 rows affected (0.00 sec)
MariaDB [(none)]> GRANT ALL PRIVILEGES ON manila.* TO 'manila'@'%' IDENTIFIED BY 'manila';
Query OK, 0 rows affected (0.01 sec)
MariaDB [(none)]> EXIT;
Bye
[root@manila ~(keystone_admin)]#
~~~
Step 2 : Basic Manila Configuration
~~~
[root@manila ~(keystone_admin)]# cd /etc/manila/
[root@manila manila(keystone_admin)]# ls -lhrt
total 116K
-rw-r-----. 1 root manila 989 Oct 6 2016 rootwrap.conf
-rw-r-----. 1 root manila 5.4K Oct 6 2016 policy.json
-rw-r-----. 1 root manila 1.9K Oct 6 2016 api-paste.ini
-rw-r-----. 1 root manila 100K Feb 13 23:04 manila.conf
[root@manila manila(keystone_admin)]#
[root@manila manila(keystone_admin)]# cp manila.conf{,.bak}
[root@manila manila(keystone_admin)]#
[root@manila manila(keystone_admin)]# ls -lhrt
total 216K
-rw-r-----. 1 root manila 989 Oct 6 2016 rootwrap.conf
-rw-r-----. 1 root manila 5.4K Oct 6 2016 policy.json
-rw-r-----. 1 root manila 1.9K Oct 6 2016 api-paste.ini
-rw-r-----. 1 root manila 100K Feb 13 23:04 manila.conf
-rw-r-----. 1 root root 100K Jun 8 07:11 manila.conf.bak
[root@manila manila(keystone_admin)]#
~~~
Step 3 : Minimum Manila configuration to up and running.
~~~
[root@manila manila(keystone_admin)]# cat manila.conf
[DEFAULT]
api_paste_config = /etc/manila/api-paste.ini
state_path = /var/lib/manila
storage_availability_zone = nova
rootwrap_config = /etc/manila/rootwrap.conf
auth_strategy = keystone
network_api_class = manila.network.neutron.neutron_network_plugin.NeutronNetworkPlugin
osapi_share_listen = 0.0.0.0
debug = False
log_dir = /var/log/manila
rpc_backend = rabbit
control_exchange = openstack
default_share_type = default_share_type
[database]
connection = mysql+pymysql://manila:manila@192.168.122.215/manila
[keystone_authtoken]
auth_uri = http://192.168.122.215:5000/v2.0
auth_type = password
password=manila
username=manila
auth_url=http://192.168.122.215:35357
project_name=services
[oslo_concurrency]
lock_path = /tmp/manila/manila_locks
[oslo_messaging_amqp]
[oslo_messaging_notifications]
driver =messaging
[oslo_messaging_rabbit]
rabbit_host = 192.168.122.215
rabbit_port = 5672
rabbit_use_ssl = False
rabbit_userid = guest
rabbit_password = guest
[root@manila manila(keystone_admin)]#
~~~
Step 4 : Populate the database and start the manila-api and scheduler service except share service.
chmod 640 /etc/manila/manila.conf
chgrp manila /etc/manila/manila.conf
~~~
[root@manila manila(keystone_admin)]# chmod 640 /etc/manila/manila.conf
[root@manila manila(keystone_admin)]# chgrp manila /etc/manila/manila.conf
[root@manila manila(keystone_admin)]#
~~~
su -s /bin/bash manila -c "manila-manage db sync"
~~~
[root@manila manila(keystone_admin)]# su -s /bin/bash manila -c "manila-manage db sync"
2017-06-08 07:20:15.709 8341 INFO alembic.runtime.migration [-] Context impl MySQLImpl.
2017-06-08 07:20:15.709 8341 INFO alembic.runtime.migration [-] Will assume non-transactional DDL.
2017-06-08 07:20:15.720 8341 INFO alembic.runtime.migration [-] Running upgrade -> 162a3e673105, manila_init
2017-06-08 07:20:15.947 8341 INFO alembic.runtime.migration [-] Running upgrade 162a3e673105 -> 211836bf835c,
add access level
2017-06-08 07:20:15.954 8341 INFO alembic.runtime.migration [-] Running upgrade 211836bf835c -> 38e632621e5a,
change volume_type to share_type
2017-06-08 07:20:15.954 8341 INFO 38e632621e5a_change_volume_type_to_share_type_py [-] Renaming column
name shares.volume_type_id to shares.share_type.id
2017-06-08 07:20:15.959 8341 INFO 38e632621e5a_change_volume_type_to_share_type_py [-] Renaming
volume_types table to share_types
2017-06-08 07:20:15.976 8341 INFO 38e632621e5a_change_volume_type_to_share_type_py [-] Creating
share_type_extra_specs table
2017-06-08 07:20:15.981 8341 INFO 38e632621e5a_change_volume_type_to_share_type_py [-] Migrating
volume_type_extra_specs to share_type_extra_specs
2017-06-08 07:20:15.985 8341 INFO 38e632621e5a_change_volume_type_to_share_type_py [-] Dropping
volume_type_extra_specs table
2017-06-08 07:20:15.988 8341 INFO alembic.runtime.migration [-] Running upgrade 38e632621e5a -> 17115072e1c3,
add_nova_net_id_column_to_share_networks
2017-06-08 07:20:15.994 8341 INFO alembic.runtime.migration [-] Running upgrade 17115072e1c3 -> 4ee2cf4be19a,
Remove share_snapshots.export_location
2017-06-08 07:20:16.000 8341 INFO alembic.runtime.migration [-] Running upgrade 4ee2cf4be19a -> 59eb64046740,
Add required extra spec
/usr/lib64/python2.7/site-packages/sqlalchemy/sql/default_comparator.py:153: SAWarning: The IN-predicate on
"share_types.id" was invoked with an empty sequence. This results in a contradiction, which nonetheless can be
expensive to evaluate. Consider alternative strategies for improved performance.
'strategies for improved performance.' % expr)
2017-06-08 07:20:16.016 8341 INFO alembic.runtime.migration [-] Running upgrade 59eb64046740 -> ef0c02b4366,
Add_share_type_projects
2017-06-08 07:20:16.029 8341 INFO alembic.runtime.migration [-] Running upgrade ef0c02b4366 -> 30cb96d995fa,
add public column for share
2017-06-08 07:20:16.037 8341 INFO alembic.runtime.migration [-] Running upgrade 30cb96d995fa -> 56cdbe267881,
Add share_export_locations table
2017-06-08 07:20:16.049 8341 INFO alembic.runtime.migration [-] Running upgrade 56cdbe267881 -> 3a482171410f,
add_driver_private_data_table
2017-06-08 07:20:16.054 8341 INFO alembic.runtime.migration [-] Running upgrade 3a482171410f -> 533646c7af38,
Remove unused attr status
2017-06-08 07:20:16.063 8341 INFO alembic.runtime.migration [-] Running upgrade 533646c7af38 -> 3db9992c30f3,
Transform statuses to lowercase
2017-06-08 07:20:16.092 8341 INFO alembic.runtime.migration [-] Running upgrade 3db9992c30f3 -> 5077ffcc5f1c,
add_share_instances
2017-06-08 07:20:16.225 8341 INFO alembic.runtime.migration [-] Running upgrade 5077ffcc5f1c -> 579c267fbb4d,
add_share_instances_access_map
2017-06-08 07:20:16.254 8341 INFO alembic.runtime.migration [-] Running upgrade 579c267fbb4d -> 1f0bd302c1a6,
add_availability_zones_table
2017-06-08 07:20:16.301 8341 INFO alembic.runtime.migration [-] Running upgrade 1f0bd302c1a6 -> 55761e5f59c5,
Add 'snapshot_support' extra spec to share types
2017-06-08 07:20:16.315 8341 INFO alembic.runtime.migration [-] Running upgrade 55761e5f59c5 -> 3651e16d7c43,
Create Consistency Groups Tables and Columns
2017-06-08 07:20:16.352 8341 INFO alembic.runtime.migration [-] Running upgrade 3651e16d7c43 ->
323840a08dc4, Add shares.task_state
2017-06-08 07:20:16.361 8341 INFO alembic.runtime.migration [-] Running upgrade 323840a08dc4 -> dda6de06349,
Add DB support for share instance export locations metadata.
2017-06-08 07:20:16.397 8341 INFO alembic.runtime.migration [-] Running upgrade dda6de06349 -> 344c1ac4747f,
Remove access rules status and add access_rule_status to share_instance
model
2017-06-08 07:20:16.444 8341 INFO alembic.runtime.migration [-] Running upgrade 344c1ac4747f -> 293fac1130ca,
Add replication attributes to Share and ShareInstance models.
2017-06-08 07:20:16.455 8341 INFO alembic.runtime.migration [-] Running upgrade 293fac1130ca -> 5155c7077f99,
Add more network info attributes to 'network_allocations' table.
2017-06-08 07:20:16.480 8341 INFO alembic.runtime.migration [-] Running upgrade 5155c7077f99 -> eb6d5544cbbd,
add provider_location to share_snapshot_instances
2017-06-08 07:20:16.487 8341 INFO alembic.runtime.migration [-] Running upgrade eb6d5544cbbd -> 221a83cfd85b,
change_user_id_length
2017-06-08 07:20:16.487 8341 INFO 221a83cfd85b_change_user_project_id_length_py [-] Changing user_id length
for share_networks
2017-06-08 07:20:16.492 8341 INFO 221a83cfd85b_change_user_project_id_length_py [-] Changing project_id length
for share_networks
2017-06-08 07:20:16.497 8341 INFO 221a83cfd85b_change_user_project_id_length_py [-] Changing project_id length
for security_services
2017-06-08 07:20:16.503 8341 INFO alembic.runtime.migration [-] Running upgrade 221a83cfd85b -> fdfb668d19e1,
add_gateway_to_network_allocations_table
2017-06-08 07:20:16.513 8341 INFO alembic.runtime.migration [-] Running upgrade fdfb668d19e1 -> e8ea58723178,
Remove host from driver private data
2017-06-08 07:20:16.520 8341 INFO alembic.runtime.migration [-] Running upgrade e8ea58723178 -> 493eaffd79e1,
add_mtu_network_allocations
2017-06-08 07:20:16.530 8341 INFO alembic.runtime.migration [-] Running upgrade 493eaffd79e1 -> 63809d875e32,
add_access_key
2017-06-08 07:20:16.537 8341 INFO alembic.runtime.migration [-] Running upgrade 63809d875e32 -> 48a7beae3117,
move_share_type_id_to_instances
[root@manila manila(keystone_admin)]#
~~~
systemctl start openstack-manila-api openstack-manila-scheduler
systemctl enable openstack-manila-api openstack-manila-scheduler
~~~
[root@manila manila(keystone_admin)]# systemctl start openstack-manila-api openstack-manila-scheduler
[root@manila manila(keystone_admin)]# systemctl enable openstack-manila-api openstack-manila-scheduler
Created symlink from /etc/systemd/system/multi-user.target.wants/openstack-manila-api.service to
/usr/lib/systemd/system/openstack-manila-api.service.
Created symlink from /etc/systemd/system/multi-user.target.wants/openstack-manila-scheduler.service to
/usr/lib/systemd/system/openstack-manila-scheduler.service.
[root@manila manila(keystone_admin)]#
[root@manila manila(keystone_admin)]# systemctl status openstack-manila-api openstack-manila-scheduler
● openstack-manila-api.service - OpenStack Manila API Server
Loaded: loaded (/usr/lib/systemd/system/openstack-manila-api.service; enabled; vendor preset: disabled)
Active: active (running) since Thu 2017-06-08 07:20:45 IST; 10s ago
Main PID: 8406 (manila-api)
CGroup: /system.slice/openstack-manila-api.service
├─8406 /usr/bin/python2 /usr/bin/manila-api --config-file /usr/share/manila/manila-dist.conf --config-file
/etc/manila/manila.conf --logfile /var/log/manila/api.log
└─8448 /usr/bin/python2 /usr/bin/manila-api --config-file /usr/share/manila/manila-dist.conf --config-file
/etc/manila/manila.conf --logfile /var/log/manila/api.log
Jun 08 07:20:46 manila.example.com manila-api[8406]: 2017-06-08 07:20:46.862 8406 INFO manila.api.extensions [-]
Initializing extension manager.
Jun 08 07:20:46 manila.example.com manila-api[8406]: 2017-06-08 07:20:46.933 8406 INFO manila.api.extensions [-]
Initializing extension manager.
Jun 08 07:20:47 manila.example.com manila-api[8406]: 2017-06-08 07:20:46.996 8406 INFO manila.api.extensions [-]
Initializing extension manager.
Jun 08 07:20:47 manila.example.com manila-api[8406]: 2017-06-08 07:20:47.002 8406 INFO manila.wsgi [-]
osapi_share listening on 0.0.0.0:8786
Jun 08 07:20:47 manila.example.com manila-api[8406]: 2017-06-08 07:20:47.002 8406 INFO oslo_service.service [-]
Starting 1 workers
Jun 08 07:20:47 manila.example.com manila-api[8406]: 2017-06-08 07:20:47.036 8406 WARNING oslo_config.cfg [-]
Option "rabbit_host" from group "oslo_messaging_rabbit" is deprecated for removal. I...n the future.
Jun 08 07:20:47 manila.example.com manila-api[8406]: 2017-06-08 07:20:47.038 8406 WARNING oslo_config.cfg [-]
Option "rabbit_port" from group "oslo_messaging_rabbit" is deprecated for removal. I...n the future.
Jun 08 07:20:47 manila.example.com manila-api[8406]: 2017-06-08 07:20:47.038 8406 WARNING oslo_config.cfg [-]
Option "rabbit_password" from group "oslo_messaging_rabbit" is deprecated for removal...n the future.
Jun 08 07:20:47 manila.example.com manila-api[8406]: 2017-06-08 07:20:47.039 8406 WARNING oslo_config.cfg [-]
Option "rabbit_userid" from group "oslo_messaging_rabbit" is deprecated for removal. ...n the future.
Jun 08 07:20:47 manila.example.com manila-api[8406]: 2017-06-08 07:20:47.047 8448 INFO eventlet.wsgi.server [-]
(8448) wsgi starting up on http://0.0.0.0:8786
● openstack-manila-scheduler.service - OpenStack Manila Scheduler
Loaded: loaded (/usr/lib/systemd/system/openstack-manila-scheduler.service; enabled; vendor preset: disabled)
Active: active (running) since Thu 2017-06-08 07:20:45 IST; 10s ago
Main PID: 8407 (manila-schedule)
CGroup: /system.slice/openstack-manila-scheduler.service
└─8407 /usr/bin/python2 /usr/bin/manila-scheduler --config-file /usr/share/manila/manila-dist.conf --config-file
/etc/manila/manila.conf --logfile /var/log/manila/scheduler.log
Jun 08 07:20:45 manila.example.com systemd[1]: Started OpenStack Manila Scheduler.
Jun 08 07:20:45 manila.example.com systemd[1]: Starting OpenStack Manila Scheduler...
Jun 08 07:20:46 manila.example.com manila-scheduler[8407]: 2017-06-08 07:20:46.447 8407 WARNING
oslo_config.cfg [-] Option "rpc_backend" from group "DEFAULT" is deprecated for removal. Its value...n the future.
Jun 08 07:20:46 manila.example.com manila-scheduler[8407]: 2017-06-08 07:20:46.741 8407 WARNING
oslo_config.cfg [-] Option "rabbit_host" from group "oslo_messaging_rabbit" is deprecated for remov...n the future.
Jun 08 07:20:46 manila.example.com manila-scheduler[8407]: 2017-06-08 07:20:46.742 8407 WARNING
oslo_config.cfg [-] Option "rabbit_port" from group "oslo_messaging_rabbit" is deprecated for remov...n the future.
Jun 08 07:20:46 manila.example.com manila-scheduler[8407]: 2017-06-08 07:20:46.742 8407 WARNING
oslo_config.cfg [-] Option "rabbit_password" from group "oslo_messaging_rabbit" is deprecated for r...n the future.
Jun 08 07:20:46 manila.example.com manila-scheduler[8407]: 2017-06-08 07:20:46.760 8407 WARNING
oslo_config.cfg [-] Option "rabbit_userid" from group "oslo_messaging_rabbit" is deprecated for rem...n the future.
Jun 08 07:20:46 manila.example.com manila-scheduler[8407]: 2017-06-08 07:20:46.761 8407 INFO manila.service [-]
Starting manila-scheduler node (version 3.0.0)
Hint: Some lines were ellipsized, use -l to show in full.
[root@manila manila(keystone_admin)]#
~~~
Step 6 : Check manila service status
~~~
[root@manila manila(keystone_admin)]# manila service-list
+----+------------------+--------------------+------+---------+-------+----------------------------+
| Id | Binary | Host | Zone | Status | State | Updated_at |
+----+------------------+--------------------+------+---------+-------+----------------------------+
| 1 | manila-scheduler | manila.example.com | nova | enabled | up | 2017-06-08T01:52:27.000000 |
+----+------------------+--------------------+------+---------+-------+----------------------------+
[root@manila manila(keystone_admin)]#
~~~
PART 3 : Configure Manila Share.
Step 1 : Add share protocol in DEFAULT section of manila.conf
~~~
enabled_share_protocols = NFS,CIFS
~~~
Step 2 : Configure local block device as shared storage on Storage Node and use it from Instances. Therefore, it
needs there is a free block device on Storage Node
[root@manila ~(keystone_admin)]# yum -y install nfs-utils nfs4-acl-tools
[root@manila ~]# pvcreate /dev/sdb1
Physical volume "/dev/sdb1" successfully created.
[root@manila ~]#
[root@manila ~]#
[root@manila ~]# vgcreate manila-volumes /dev/sdb1
Volume group "manila-volumes" successfully created
[root@manila ~]#
Step 3 : Edit manila.conf for backend configuration.
a) Add below in default section
enabled_share_backends = lvm
b) add below to the end.
~~~
[lvm]
share_backend_name = LVM
share_driver = manila.share.drivers.lvm.LVMShareDriver
driver_handles_share_servers = False
lvm_share_volume_group = manila-volumes
lvm_share_export_ip = 192.168.122.215
~~~
Step 4 : Start manila-share and nfs-server service
systemctl start openstack-manila-share nfs-server
systemctl enable openstack-manila-share nfs-server
PART 4 : Creating manil-share and to work as a filesystem
Step 1 : Create default share type.
manila type-create default_share_type False
[root@manila manila(keystone_admin)]# manila type-create default_share_type False
+----------------------+--------------------------------------+
| Property | Value |
+----------------------+--------------------------------------+
| required_extra_specs | driver_handles_share_servers : False |
| Name | default_share_type |
| Visibility | public |
| is_default | - |
| ID | 34c84315-060d-4a85-9320-0c3e637255f6 |
| optional_extra_specs | snapshot_support : True |
+----------------------+--------------------------------------+
[root@manila manila(keystone_admin)]#
Step 2 : Check Manil type whether created or not
manila type-list
[root@manila manila(keystone_admin)]# manila type-list
+--------------------------------------+--------------------+------------+------------+--------------------------------------+-------------
-----------+
| ID | Name | visibility | is_default | required_extra_specs | optional_extra_specs
| |
|
+--------------------------------------+--------------------+------------+------------+--------------------------------------+-------------
-----------+
| 34c84315-060d-4a85-9320-0c3e637255f6 | default_share_type | public | YES | driver_handles_share_servers :
False | snapshot_support : True |
+--------------------------------------+--------------------+------------+------------+--------------------------------------+-------------
-----------+
[root@manila manila(keystone_admin)]#
Step 3 : Create NFS Share
manila create NFS 1 --name share01
[root@manila manila(keystone_admin)]# manila create NFS 1 --name share01
+-----------------------------+--------------------------------------+
| Property | Value |
+-----------------------------+--------------------------------------+
| status | creating |
| share_type_name | default_share_type |
| description | None |
| availability_zone | None |
| share_network_id | None |
| share_server_id | None |
| host | |
| access_rules_status | active |
| snapshot_id | None |
| is_public | False |
| task_state | None |
| snapshot_support | True |
| id | 6c41dc1b-cf5f-4d3e-ae7b-808e4488bb5d |
| size | 1 |
| user_id | e56c83f745ed46bbb073e704b5705fc9 |
| name | share01 |
| share_type | 34c84315-060d-4a85-9320-0c3e637255f6 |
| has_replicas | False |
| replication_type | None |
| created_at | 2017-06-08T20:52:51.000000 |
| share_proto | NFS |
| consistency_group_id | None |
| source_cgsnapshot_member_id | None |
| project_id | 6e43691bcb57453fbf4a1c1004531d0b |
| metadata | {} |
+-----------------------------+--------------------------------------+
[root@manila manila(keystone_admin)]#
Step 4 : Few minutes later, the check the Status, should be turn to available
[root@manila manila(keystone_admin)]# manila list
+--------------------------------------+---------+------+-------------+-----------+-----------+--------------------+---------------------
------------------+-------------------+
| ID | Name | Size | Share Proto | Status | Is Public | Share Type Name | Host
| Availability Zone |
+--------------------------------------+---------+------+-------------+-----------+-----------+--------------------+---------------------
------------------+-------------------+
| 6c41dc1b-cf5f-4d3e-ae7b-808e4488bb5d | share01 | 1 | NFS | available | False | default_share_type |
manila.example.com@lvm#lvm-single-pool | nova |
+--------------------------------------+---------+------+-------------+-----------+-----------+--------------------+---------------------
------------------+-------------------+
[root@manila manila(keystone_admin)]#
Wow......If the status showing available means all set we are at good side.
Step 5 : Now we will use Manila Shared filesystem from Instances like follows.
[root@manila manila(keystone_admin)]# openstack server list
+--------------------------------------+----------------------------+---------+--------------------------------------+-------------+
| ID | Name | Status | Networks | Image Name |
+--------------------------------------+----------------------------+---------+--------------------------------------+-------------+
| 515e0c43-8db9-44c8-8dc6-ac6c1379bff1 | cirros_2017-06-07_07-28-48 | SHUTOFF | private=10.10.10.10,
192.168.122.104 | Cirros 0.3.4 |
+--------------------------------------+----------------------------+---------+--------------------------------------+-------------+
[root@manila manila(keystone_admin)]#
Step 6 : Create ACL for share. and list the access list
[root@manila manila(keystone_admin)]# manila access-allow share01 ip 192.168.122.0/24 --access-level rw
+--------------+--------------------------------------+
| Property | Value |
+--------------+--------------------------------------+
| access_key | None |
| share_id | 6c41dc1b-cf5f-4d3e-ae7b-808e4488bb5d |
| access_type | ip |
| access_to | 192.168.122.0/24 |
| access_level | rw |
| state | new |
| id | 1fe24d5a-2fab-43b4-ba83-75d820aec396 |
+--------------+--------------------------------------+
[root@manila manila(keystone_admin)]#
[root@manila manila(keystone_admin)]# manila access-list share01
+--------------------------------------+-------------+------------------+--------------+--------+------------+
| id | access_type | access_to | access_level | state | access_key |
+--------------------------------------+-------------+------------------+--------------+--------+------------+
| 1fe24d5a-2fab-43b4-ba83-75d820aec396 | ip | 192.168.122.0/24 | rw | active | None |
+--------------------------------------+-------------+------------------+--------------+--------+------------+
[root@manila manila(keystone_admin)]#
Step 7 : Confirm the access path, start the instance and mount the share on instance.
[root@manila manila(keystone_admin)]# manila show share01 | grep path | cut -d'|' -f3
path = 192.168.122.215:/var/lib/manila/mnt/share-490afa2a-8d9f-4b02-912d-7aadb9fcea88
[root@manila manila(keystone_admin)]#
[root@manila manila(keystone_admin)]#
[root@manila ~(keystone_admin)]# ssh 192.168.122.106
The authenticity of host '192.168.122.106 (192.168.122.106)' can't be established.
ECDSA key fingerprint is cc:44:2b:3a:e4:6b:9e:cf:32:51:29:7d:e7:45:78:4d.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.122.106' (ECDSA) to the list of known hosts.
root@192.168.122.106's password:
[root@host-10-10-10-6 ~]#
[root@host-10-10-10-6 ~]#
[root@host-10-10-10-6 ~]# mount -t nfs
192.168.122.215:/var/lib/manila/mnt/share-490afa2a-8d9f-4b02-912d-7aadb9fcea88 /mnt/
[root@host-10-10-10-6 ~]#
[root@host-10-10-10-6 ~]# df -h
Filesystem Size Used Avail Use% Mounted on
/dev/vda1 8.0G 821M 7.2G 11% /
devtmpfs 904M 0 904M 0% /dev
tmpfs 921M 0 921M 0% /dev/shm
tmpfs 921M 17M 905M 2% /run
tmpfs 921M 0 921M 0% /sys/fs/cgroup
192.168.122.215:/var/lib/manila/mnt/share-490afa2a-8d9f-4b02-912d-7aadb9fcea88 976M 2.0M 907M 1% /mnt
[root@host-10-10-10-6 ~]#


