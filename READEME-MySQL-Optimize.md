## MySQL Performance improvement
```bash
#################################################################################### 
#           Custom changes for Performance improvement
####################################################################################
[mysqld]
innodb_log_buffer_size=8M
innodb_flush_log_at_trx_commit=1
innodb_lock_wait_timeout=50
innodb_max_dirty_pages_pct=0

innodb_file_per_table = 1
bulk_insert_buffer_size=512M
innodb_buffer_pool_instances=8
innodb_buffer_pool_size=2G

join_buffer_size=8M
key_buffer_size=512M
myisam_sort_buffer_size=512M

read_buffer_size=2M
read_rnd_buffer_size=16M
sort_buffer_size=8M

innodb_log_file_size=1G
innodb_log_files_in_group=3
;innodb_max_dirty_pages_pct=90
;innodb_thread_concurrency=64
innodb_read_io_threads=8
innodb_write_io_threads=8

max_allowed_packet=1G
max_connections=1000
max_heap_table_size=64M
max_sp_recursion_depth=1
thread_stack=512K
tmp_table_size=64M
transaction_isolation=READ-COMMITTED

# Set up timezone for database.
default_time_zone='+1:00'

;sql_mode="STRICT_TRANS_TABLES,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION"
character_set_server=utf8
collation_server=utf8_general_ci

[mysqldump]
max_allowed_packet = 1G

#################################################################################### 
#			enable logging by default to help find problems
####################################################################################
general_log = 1
general_log_file = "/var/mysql/logs/mysql_query.log"
slow_query_log=ON
slow_query_log_file="/var/mysql/logs/mysql_slow_query.log"
log-slow-queries

innodb_monitor_enable=all
performance_schema=ON
log_output=file
slow_query_log=ON
long_query_time=2
log_slow_admin_statements=ON
log_slow_slave_statements=ON											

```
