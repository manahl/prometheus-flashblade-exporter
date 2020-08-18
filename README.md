# Pure FlashBlade Exporter for Prometheus

Export Prometheus scrapable metrics from a Pure Storage FlashBlade. 

The exporter minimally requires FlashBlade API version 1.2 (and version 1.3 for S3 metrics).

## Building

```
make build
```

Or simply:

```
make
```

## Usage

The exporter requires the name of the FlashBlade as a command-line argument:

`flashblade_exporter <flashblade>`

Pass in the token for API access as the environment variable `PUREFB_API`. (An API token can be generated for
the FlashBlade by using the `pureadmin` command after SSHing to the device as 'pureuser'.)

The exporter accepts the following command line flags:

| Flag                       | Description                                                                                                           | Default |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------- | ------- |
| --port                     | Port on which the exporter will bind to in order to serve up the metrics                                              | 9130    |
| --insecure                 | Disable SSL verification                                                                                              | false   |
| --filesystem-metrics       | Enable per-filesystem performance and user/group metrics (requires FlashBlade API version 1.8)                        | false   |
| --filesystem-filter-regexp | Regexp of filesystems to gather metrics for (combine with --filesystem-metrics) (requires FlashBlade API version 1.8) | `.*`    |


## Metrics

* Filesystem usage (unique, virtual, snapshot and total)
* S3 bucket usage (unique, virtual, snapshot, total and number of objects)
* Bandwidth, IOPS and latency for both read and write
* Number of alerts of each severity
* FlashBlade total capacity and usage
* Usage statistics for each user and group per filesystem (with `--filesystem-metrics`)
* Filesystem performance (with `--filesystem-metrics`)
* S3 and HTTP specific performance metrics
* NFS specific performance metrics (requires FlashBlade API version 1.9)

```
# HELP flashblade_alert_alert_total Number of open alerts
# TYPE flashblade_alert_alert_total gauge
# HELP flashblade_blade_num_healthy_blades Number of blades in healthy status
# TYPE flashblade_blade_num_healthy_blades gauge
# HELP flashblade_blade_num_unhealthy_blades Number of blades in a non-healthy status
# TYPE flashblade_blade_num_unhealthy_blades gauge
# HELP flashblade_collector_build_info A metric with a constant '1' value labeled by version, revision, branch, and goversion from which flashblade_collector was built.
# TYPE flashblade_collector_build_info gauge
# HELP flashblade_fs_data_reduction Reduction of data
# TYPE flashblade_fs_data_reduction gauge
# HELP flashblade_fs_snapshot_usage_bytes Physical usage by snapshots, non-unique
# TYPE flashblade_fs_snapshot_usage_bytes gauge
# HELP flashblade_fs_total_usage_bytes Total physical usage (including snapshots) in bytes
# TYPE flashblade_fs_total_usage_bytes gauge
# HELP flashblade_fs_unique_usage_bytes Physical usage in bytes
# TYPE flashblade_fs_unique_usage_bytes gauge
# HELP flashblade_fs_virtual_usage_bytes Usage in bytes
# TYPE flashblade_fs_virtual_usage_bytes gauge
# HELP flashblade_perf_bytes_per_op Average operation size (read bytes+write bytes/(read ops+write ops))
# TYPE flashblade_perf_bytes_per_op gauge
# HELP flashblade_perf_bytes_per_read Average read size in bytes per read operation
# TYPE flashblade_perf_bytes_per_read gauge
# HELP flashblade_perf_bytes_per_write Average write size in bytes per write operation
# TYPE flashblade_perf_bytes_per_write gauge
# HELP flashblade_perf_http_others_per_sec Other operations processed per second
# TYPE flashblade_perf_http_others_per_sec gauge
# HELP flashblade_perf_http_read_dirs_per_sec Read directories requests processed per second
# TYPE flashblade_perf_http_read_dirs_per_sec gauge
# HELP flashblade_perf_http_read_files_per_sec Read files requests processed per second
# TYPE flashblade_perf_http_read_files_per_sec gauge
# HELP flashblade_perf_http_usec_per_other_op Average time, measured in microseconds, that the array takes to process other operations
# TYPE flashblade_perf_http_usec_per_other_op gauge
# HELP flashblade_perf_http_usec_per_read_dir_op Average time, measured in microseconds, that the array takes to process a read directory request
# TYPE flashblade_perf_http_usec_per_read_dir_op gauge
# HELP flashblade_perf_http_usec_per_read_file_op Average time, measured in microseconds, that the array takes to process a read file request
# TYPE flashblade_perf_http_usec_per_read_file_op gauge
# HELP flashblade_perf_http_usec_per_write_dir_op Average time, measured in microseconds, that the array takes to process a write directory request
# TYPE flashblade_perf_http_usec_per_write_dir_op gauge
# HELP flashblade_perf_http_usec_per_write_file_op Average time, measured in microseconds, that the array takes to process a write file request
# TYPE flashblade_perf_http_usec_per_write_file_op gauge
# HELP flashblade_perf_http_write_dirs_per_sec Write directories requests processed per second
# TYPE flashblade_perf_http_write_dirs_per_sec gauge
# HELP flashblade_perf_http_write_files_per_sec Write files requests processed per second
# TYPE flashblade_perf_http_write_files_per_sec gauge
# HELP flashblade_perf_input_per_sec Bytes written per second
# TYPE flashblade_perf_input_per_sec gauge
# HELP flashblade_perf_nfs_accesses_per_sec ACCESS requests processed per second
# TYPE flashblade_perf_nfs_accesses_per_sec gauge
# HELP flashblade_perf_nfs_aggregate_file_metadata_creates_per_sec Sum of file-level or directory-level create-like metadata requests per second. Includes CREATE, LINK, MKDIR, and SYMLINK
# TYPE flashblade_perf_nfs_aggregate_file_metadata_creates_per_sec gauge
# HELP flashblade_perf_nfs_aggregate_file_metadata_modifies_per_sec Sum of file-level or directory-level modify-like and delete-like metadata requests per second. Includes REMOVE, RENAME, RMDIR, and SETATTR
# TYPE flashblade_perf_nfs_aggregate_file_metadata_modifies_per_sec gauge
# HELP flashblade_perf_nfs_aggregate_file_metadata_reads_per_sec Sum of file-level or directory-level read-like metadata requests per second. Includes GETATTR, LOOKUP, PATHCONF, READDIR, READDIRPLUS, and READLINK
# TYPE flashblade_perf_nfs_aggregate_file_metadata_reads_per_sec gauge
# HELP flashblade_perf_nfs_aggregate_share_metadata_reads_per_sec Sum of share-level read-like metadata requests per second. Includes ACCESS, FSINFO, and FSSTAT
# TYPE flashblade_perf_nfs_aggregate_share_metadata_reads_per_sec gauge
# HELP flashblade_perf_nfs_aggregate_usec_per_file_metadata_create_op Average time, measured in microseconds, it takes the array to process a file-level or directory-level create-like metadata request. Includes CREATE, LINK, MKDIR, and SYMLINK
# TYPE flashblade_perf_nfs_aggregate_usec_per_file_metadata_create_op gauge
# HELP flashblade_perf_nfs_aggregate_usec_per_file_metadata_modify_op Average time, measured in microseconds, it takes the array to process a file-level or directory-level modify-like or delete-like metadata request. Includes REMOVE, RENAME, RMDIR, and SETATTR
# TYPE flashblade_perf_nfs_aggregate_usec_per_file_metadata_modify_op gauge
# HELP flashblade_perf_nfs_aggregate_usec_per_file_metadata_read_op Average time, measured in microseconds, it takes the array to process a file-level or directory-level read-like metadata request. Includes GETATTR, LOOKUP, PATHCONF, READDIR, READDIRPLUS, and READLINK
# TYPE flashblade_perf_nfs_aggregate_usec_per_file_metadata_read_op gauge
# HELP flashblade_perf_nfs_aggregate_usec_per_share_metadata_read_op Average time, measured in microseconds, it takes the array to process a share-level read-like metadata request. Includes ACCESS, FSINFO, and FSSTAT
# TYPE flashblade_perf_nfs_aggregate_usec_per_share_metadata_read_op gauge
# HELP flashblade_perf_nfs_creates_per_sec CREATE requests processed per second
# TYPE flashblade_perf_nfs_creates_per_sec gauge
# HELP flashblade_perf_nfs_fsinfos_per_sec FSINFO requests processed per second
# TYPE flashblade_perf_nfs_fsinfos_per_sec gauge
# HELP flashblade_perf_nfs_fsstats_per_sec FSSTAT requests processed per second
# TYPE flashblade_perf_nfs_fsstats_per_sec gauge
# HELP flashblade_perf_nfs_getattrs_per_sec GETATTR requests processed per second
# TYPE flashblade_perf_nfs_getattrs_per_sec gauge
# HELP flashblade_perf_nfs_links_per_sec LINK requests processed per second
# TYPE flashblade_perf_nfs_links_per_sec gauge
# HELP flashblade_perf_nfs_lookups_per_sec LOOKUP requests processed per second
# TYPE flashblade_perf_nfs_lookups_per_sec gauge
# HELP flashblade_perf_nfs_mkdirs_per_sec MKDIR requests processed per second
# TYPE flashblade_perf_nfs_mkdirs_per_sec gauge
# HELP flashblade_perf_nfs_others_per_sec All other requests processed per second
# TYPE flashblade_perf_nfs_others_per_sec gauge
# HELP flashblade_perf_nfs_pathconfs_per_sec PATHCONF requests processed per second
# TYPE flashblade_perf_nfs_pathconfs_per_sec gauge
# HELP flashblade_perf_nfs_readdirpluses_per_sec READDIRPLUS requests processed per second
# TYPE flashblade_perf_nfs_readdirpluses_per_sec gauge
# HELP flashblade_perf_nfs_readdirs_per_sec READDIR requests processed per second
# TYPE flashblade_perf_nfs_readdirs_per_sec gauge
# HELP flashblade_perf_nfs_readlinks_per_sec READLINK requests processed per second
# TYPE flashblade_perf_nfs_readlinks_per_sec gauge
# HELP flashblade_perf_nfs_reads_per_sec READ requests processed per second
# TYPE flashblade_perf_nfs_reads_per_sec gauge
# HELP flashblade_perf_nfs_removes_per_sec REMOVE requests processed per second
# TYPE flashblade_perf_nfs_removes_per_sec gauge
# HELP flashblade_perf_nfs_renames_per_sec RENAME requests processed per second
# TYPE flashblade_perf_nfs_renames_per_sec gauge
# HELP flashblade_perf_nfs_rmdirs_per_sec RMDIR requests processed per second
# TYPE flashblade_perf_nfs_rmdirs_per_sec gauge
# HELP flashblade_perf_nfs_setattrs_per_sec SETATTR requests processed per second
# TYPE flashblade_perf_nfs_setattrs_per_sec gauge
# HELP flashblade_perf_nfs_symlinks_per_sec SYMLINK requests processed per second
# TYPE flashblade_perf_nfs_symlinks_per_sec gauge
# HELP flashblade_perf_nfs_usec_per_access_op Average time, measured in microseconds, it takes the array to process an ACCESS request
# TYPE flashblade_perf_nfs_usec_per_access_op gauge
# HELP flashblade_perf_nfs_usec_per_create_op Average time, measured in microseconds, it takes the array to process a CREATE request
# TYPE flashblade_perf_nfs_usec_per_create_op gauge
# HELP flashblade_perf_nfs_usec_per_fsinfo_op Average time, measured in microseconds, it takes the array to process an FSINFO request
# TYPE flashblade_perf_nfs_usec_per_fsinfo_op gauge
# HELP flashblade_perf_nfs_usec_per_fsstat_op Average time, measured in microseconds, it takes the array to process an FSSTAT request
# TYPE flashblade_perf_nfs_usec_per_fsstat_op gauge
# HELP flashblade_perf_nfs_usec_per_getattr_op Average time, measured in microseconds, it takes the array to process a GETATTR request
# TYPE flashblade_perf_nfs_usec_per_getattr_op gauge
# HELP flashblade_perf_nfs_usec_per_link_op Average time, measured in microseconds, it takes the array to process a LINK request
# TYPE flashblade_perf_nfs_usec_per_link_op gauge
# HELP flashblade_perf_nfs_usec_per_lookup_op Average time, measured in microseconds, it takes the array to process a LOOKUP request
# TYPE flashblade_perf_nfs_usec_per_lookup_op gauge
# HELP flashblade_perf_nfs_usec_per_mkdir_op Average time, measured in microseconds, it takes the array to process a MKDIR request
# TYPE flashblade_perf_nfs_usec_per_mkdir_op gauge
# HELP flashblade_perf_nfs_usec_per_other_op Average time, measured in microseconds, it takes the array to process all other requests
# TYPE flashblade_perf_nfs_usec_per_other_op gauge
# HELP flashblade_perf_nfs_usec_per_pathconf_op Average time, measured in microseconds, it takes the array to process a PATHCONF request
# TYPE flashblade_perf_nfs_usec_per_pathconf_op gauge
# HELP flashblade_perf_nfs_usec_per_read_op Average time, measured in microseconds, it takes the array to process a READ request
# TYPE flashblade_perf_nfs_usec_per_read_op gauge
# HELP flashblade_perf_nfs_usec_per_readdir_op Average time, measured in microseconds, it takes the array to process a READDIR request
# TYPE flashblade_perf_nfs_usec_per_readdir_op gauge
# HELP flashblade_perf_nfs_usec_per_readdir_plus_op Average time, measured in microseconds, it takes the array to process a READDIRPLUS request
# TYPE flashblade_perf_nfs_usec_per_readdir_plus_op gauge
# HELP flashblade_perf_nfs_usec_per_readlink_op Average time, measured in microseconds, it takes the array to process a READLINK request
# TYPE flashblade_perf_nfs_usec_per_readlink_op gauge
# HELP flashblade_perf_nfs_usec_per_remove_op Average time, measured in microseconds, it takes the array to process a REMOVE request
# TYPE flashblade_perf_nfs_usec_per_remove_op gauge
# HELP flashblade_perf_nfs_usec_per_rename_op Average time, measured in microseconds, it takes the array to process a RENAME request
# TYPE flashblade_perf_nfs_usec_per_rename_op gauge
# HELP flashblade_perf_nfs_usec_per_rmdir_op Average time, measured in microseconds, it takes the array to process an RMDIR request
# TYPE flashblade_perf_nfs_usec_per_rmdir_op gauge
# HELP flashblade_perf_nfs_usec_per_setattr_op Average time, measured in microseconds, it takes the array to process a SETATTR request
# TYPE flashblade_perf_nfs_usec_per_setattr_op gauge
# HELP flashblade_perf_nfs_usec_per_symlink_op Average time, measured in microseconds, it takes the array to process a SYMLINK request
# TYPE flashblade_perf_nfs_usec_per_symlink_op gauge
# HELP flashblade_perf_nfs_usec_per_write_op Average time, measured in microseconds, it takes the array to process a WRITE request
# TYPE flashblade_perf_nfs_usec_per_write_op gauge
# HELP flashblade_perf_nfs_writes_per_sec WRITE requests processed per second
# TYPE flashblade_perf_nfs_writes_per_sec gauge
# HELP flashblade_perf_others_per_sec Other operations processed per second
# TYPE flashblade_perf_others_per_sec gauge
# HELP flashblade_perf_output_per_sec Bytes read per second
# TYPE flashblade_perf_output_per_sec gauge
# HELP flashblade_perf_reads_per_sec Read requests processed per second
# TYPE flashblade_perf_reads_per_sec gauge
# HELP flashblade_perf_s3_others_per_sec Other operations processed per second
# TYPE flashblade_perf_s3_others_per_sec gauge
# HELP flashblade_perf_s3_read_buckets_per_sec Read bucket requests processed per second
# TYPE flashblade_perf_s3_read_buckets_per_sec gauge
# HELP flashblade_perf_s3_read_objects_per_sec Read object requests processed per second
# TYPE flashblade_perf_s3_read_objects_per_sec gauge
# HELP flashblade_perf_s3_usec_per_other_op Average time, measured in microseconds, that the array takes to process other operations
# TYPE flashblade_perf_s3_usec_per_other_op gauge
# HELP flashblade_perf_s3_usec_per_read_bucket_op Average time, measured in microseconds, that the array takes to process a read bucket request
# TYPE flashblade_perf_s3_usec_per_read_bucket_op gauge
# HELP flashblade_perf_s3_usec_per_read_object_op Average time, measured in microseconds, that the array takes to process a read object request
# TYPE flashblade_perf_s3_usec_per_read_object_op gauge
# HELP flashblade_perf_s3_usec_per_write_bucket_op Average time, measured in microseconds, that the array takes to process a write bucket request
# TYPE flashblade_perf_s3_usec_per_write_bucket_op gauge
# HELP flashblade_perf_s3_usec_per_write_object_op Average time, measured in microseconds, that the array takes to process a write object request
# TYPE flashblade_perf_s3_usec_per_write_object_op gauge
# HELP flashblade_perf_s3_write_buckets_per_sec Write bucket requests processed per second
# TYPE flashblade_perf_s3_write_buckets_per_sec gauge
# HELP flashblade_perf_s3_write_objects_per_sec Write object requests processed per second
# TYPE flashblade_perf_s3_write_objects_per_sec gauge
# HELP flashblade_perf_usec_per_other_op Average time, measured in microseconds, that the array takes to process other operations
# TYPE flashblade_perf_usec_per_other_op gauge
# HELP flashblade_perf_usec_per_read_op Average time, measured in microseconds, that the array takes to process a read request
# TYPE flashblade_perf_usec_per_read_op gauge
# HELP flashblade_perf_usec_per_write_op Average time, measured in microseconds, that the array takes to process a write request
# TYPE flashblade_perf_usec_per_write_op gauge
# HELP flashblade_perf_writes_per_sec Write requests processed per second
# TYPE flashblade_perf_writes_per_sec gauge
# HELP flashblade_s3_data_reduction Reduction of data
# TYPE flashblade_s3_data_reduction gauge
# HELP flashblade_s3_object_count The number of object within the bucket.
# TYPE flashblade_s3_object_count gauge
# HELP flashblade_s3_snapshot_usage_bytes Physical usage by snapshots, non-unique
# TYPE flashblade_s3_snapshot_usage_bytes gauge
# HELP flashblade_s3_total_usage_bytes Total physical usage (including snapshots) in bytes
# TYPE flashblade_s3_total_usage_bytes gauge
# HELP flashblade_s3_unique_usage_bytes Physical usage in bytes
# TYPE flashblade_s3_unique_usage_bytes gauge
# HELP flashblade_s3_virtual_usage_bytes Usage in bytes
# TYPE flashblade_s3_virtual_usage_bytes gauge
# HELP flashblade_space_capacity_bytes Usable capacity in bytes
# TYPE flashblade_space_capacity_bytes gauge
# HELP flashblade_space_data_reduction Reduction of data
# TYPE flashblade_space_data_reduction gauge
# HELP flashblade_space_snapshot_usage_bytes Physical usage by snapshots, non-unique
# TYPE flashblade_space_snapshot_usage_bytes gauge
# HELP flashblade_space_total_usage_bytes Total physical usage (including snapshots) in bytes
# TYPE flashblade_space_total_usage_bytes gauge
# HELP flashblade_space_unique_usage_bytes Physical usage in bytes
# TYPE flashblade_space_unique_usage_bytes gauge
# HELP flashblade_space_virtual_usage_bytes Usage in bytes
# TYPE flashblade_space_virtual_usage_bytes gauge
```

Additionally, with `--filesystem-metrics`:

```
# HELP flashblade_usagequota_memory_quota_bytes Quota of a user/group on a filesystem in bytes
# TYPE flashblade_usagequota_memory_quota_bytes gauge
# HELP flashblade_usagequota_memory_usage_bytes Usage of a user/group on a filesystem in bytes
# TYPE flashblade_usagequota_memory_usage_bytes gauge
# HELP flashblade_fs_performance_bytes_per_op Average operation size (read bytes+write bytes/(read ops+write ops))
# TYPE flashblade_fs_performance_bytes_per_op gauge
# HELP flashblade_fs_performance_bytes_per_read Average read size in bytes per read operation
# TYPE flashblade_fs_performance_bytes_per_read gauge
# HELP flashblade_fs_performance_bytes_per_write Average write size in bytes per write operation
# TYPE flashblade_fs_performance_bytes_per_write gauge
# HELP flashblade_fs_performance_non_read_write_operations_per_second All operations except read processed per second
# TYPE flashblade_fs_performance_non_read_write_operations_per_second gauge
# HELP flashblade_fs_performance_read_bytes_per_second Read byte requests processed per second
# TYPE flashblade_fs_performance_read_bytes_per_second gauge
# HELP flashblade_fs_performance_reads_per_second Read requests processed per second
# TYPE flashblade_fs_performance_reads_per_second gauge
# HELP flashblade_fs_performance_sec_per_non_read_write_op Average time, measured in seconds, that the array takes to process other operations
# TYPE flashblade_fs_performance_sec_per_non_read_write_op gauge
# HELP flashblade_fs_performance_sec_per_read_op Average time, measured in seconds, that the array takes to process a read request
# TYPE flashblade_fs_performance_sec_per_read_op gauge
# HELP flashblade_fs_performance_sec_per_write_op Average time, measured in seconds, that the array takes to process a write request
# TYPE flashblade_fs_performance_sec_per_write_op gauge
# HELP flashblade_fs_performance_write_bytes_per_second Write byte requests processed per second
# TYPE flashblade_fs_performance_write_bytes_per_second gauge
# HELP flashblade_fs_performance_writes_per_second Write requests processed per second
# TYPE flashblade_fs_performance_writes_per_second gauge
```

## Authors
Prometheus FlashBlade Exporter has been under development since 2019 and welcomes contributions.

* [Michael Captain](https://github.com/macaptain), Man Group
* [Advait Bhatwdekar](https://github.com/You-NeverKnow), Hudson River Trading LLC
* [Jeff Patti](https://github.com/jepatti), Man Group
* [Mike Frost](https://github.com/mifrost), Man Group
