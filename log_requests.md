# Log requests

## Log in the table

It's a good habit to log requests during integration of two IT-systems. We can do it with a MySQL table:

```sql
CREATE TABLE `request_logs` (
	`id` INT(11) NOT NULL AUTO_INCREMENT,
	`request_id` VARCHAR(255) NOT NULL COLLATE 'utf8_general_ci',
	`url` VARCHAR(1500) NOT NULL COLLATE 'utf8_general_ci',
	`request_body` TEXT(65535) NOT NULL COLLATE 'utf8_general_ci',
	`response_code` SMALLINT(5) UNSIGNED NOT NULL,
	`response_body` TEXT(65535) NOT NULL COLLATE 'utf8_general_ci',
	`request_duration` DECIMAL(12,3) UNSIGNED NULL DEFAULT '0.000',
	`create_time` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
	PRIMARY KEY (`id`) USING BTREE,
	INDEX `index_request_logs_request_id` (`request_id`) USING BTREE,
	INDEX `index_request_logs_create_time` (`create_time`) USING BTREE
)
COLLATE='utf8_general_ci'
ENGINE=InnoDB
```
