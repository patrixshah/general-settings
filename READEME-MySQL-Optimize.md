#################################################################################### 
#           Custom changes for Performance improvement
####################################################################################
[mysqld]
innodb_file_per_table = 1
bulk_insert_buffer_size=512M
innodb_buffer_pool_instances=8
innodb_buffer_pool_size=2G
;innodb_file_format=Barracuda
;innodb_file_format_max=Barracuda

join_buffer_size=8M
key_buffer_size=512M
myisam_sort_buffer_size=512M

read_buffer_size=2M
read_rnd_buffer_size=16M
sort_buffer_size=8M

innodb_log_file_size=5M
innodb_log_files_in_group=2
;innodb_read_io_threads=8
;innodb_write_io_threads=8

max_allowed_packet=1G
max_connections=1000
max_heap_table_size=64M
max_sp_recursion_depth=1
thread_stack=512K
tmp_table_size=64M
transaction_isolation=READ-COMMITTED

;sql_mode="STRICT_TRANS_TABLES,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION"
character_set_server=utf8
collation_server=utf8_general_ci

[mysqldump]
max_allowed_packet = 1G
