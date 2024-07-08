## Đánh giá các phiên bản ceph tại Quincy và một số phiên bản cho rgw tại Reef

### Một số thay đổi đáng chú ý so với bản Pacific

**Thay đổi chung**

- BlueStore is Ceph’s default object store.

- device_health_metrics -> .mgr (pool name)

- Cephadm: osd_memory_target_autotune is enabled by default, which sets mgr/cephadm/autotune_memory_target_ratio to 0.7 of total RAM. This is unsuitable for hyperconverged infrastructures. For hyperconverged Ceph, please refer to the documentation or set mgr/cephadm/autotune_memory_target_ratio to 0.2. 


- MGR: The progress module disables the pg recovery event by default since the event is expensive and has interrupted other services when there are OSDs being marked in/out from the cluster. However, the user can still enable this event anytime. For more detail, see:

https://docs.ceph.com/en/quincy/mgr/progress/

Ability to zap osds as they are removed
**RADOS**

OSD: Support for on-wire compression for osd-osd communication, off by default.

For more details about compression modes, see: https://docs.ceph.com/en/quincy/rados/configuration/msgr2/#compression-modes

**RGW**

RGW now supports rate limiting by user and/or by bucket. With this feature it is possible to limit user and/or bucket, the total operations and/or bytes per minute can be delivered. This feature allows the admin to limit only READ operations and/or WRITE operations. The rate-limiting configuration could be applied on all users and all buckets by using global configuration.

S3 bucket notification events now contain an eTag key instead of etag, and eventName values no longer carry the s3: prefix, fixing deviations from the message format that is observed on AWS.

The behavior for Multipart Upload was modified so that only CompleteMultipartUpload notification is sent at the end of the multipart upload. The POST notification at the beginning of the upload and the PUT notifications that were sent on each part are no longer sent.

**RBD**

- rbd-nbd: rbd device attach and rbd device detach commands added, these allow for safe reattach after rbd-nbd daemon is restarted since Linux kernel 5.14.

- rbd-nbd: notrim map option added to support thick-provisioned images, similar to krbd.

- Large stabilization effort for client-side persistent caching on SSD devices, also available in 16.2.8. For details on usage, see:

https://docs.ceph.com/en/quincy/rbd/rbd-persistent-write-log-cache/

- Several bug fixes in diff calculation when using fast-diff image feature + whole object (inexact) mode. In some rare cases these long-standing issues could cause an incorrect rbd export. Also fixed in 15.2.16 and 16.2.8.

- Fix for a potential performance degradation when running Windows VMs on krbd. For details, see rxbounce map option description:

https://docs.ceph.com/en/quincy/man/8/rbd/#kernel-rbd-krbd-options





To Do:


- [x] Tóm tắt 1 bản cập nhập so với pacific theo những link ở bên trên

- [ ] Liệt kê từng bản recommend của ceph tại quincy, đối với rgw liệt kê ra có thể dùng được tại reef-đặc biệt support rgw nói chung và multi-site nói riêng

- [ ] Từng bản thì liệt kê ra những thay đổi theo tiêu chí trên, bug, feature và performance

- [ ] Liệt kê các production đã sử dụng ceph ver 17
https://youtu.be/S2rPA7qlSYY?t=271


### Tóm tắt một những thay đổi của quincy so với pacific

Một số thay đổi/cải tiến của quincy so với pacific như sau.

- Filestore has been deprecated in Quincy. BlueStore is Ceph’s default object store.

- pool `device_health_metrics` đổi tên thành pool `.mgr`

- Cephadm: `osd_memory_target_autotune` is enabled by default, which sets mgr/cephadm/autotune_memory_target_ratio to 0.7 of total RAM. 


- MGR: The progress module disables the pg recovery event by default since the event is expensive and has interrupted other services when there are OSDs being marked in/out from the cluster.


**RADOS**

OSD: Support for on-wire compression for osd-osd communication, off by default.

For more details about compression modes, see: https://docs.ceph.com/en/quincy/rados/configuration/msgr2/#compression-modes

**RGW**

RGW now supports rate limiting by user and/or by bucket. With this feature it is possible to limit user and/or bucket, the total operations and/or bytes per minute can be delivered. This feature allows the admin to limit only READ operations and/or WRITE operations. The rate-limiting configuration could be applied on all users and all buckets by using global configuration. -> có thể set limit cho user hoặc bucket về operation hoặc bandwidth

- đổi etag -> Etag, cần lưu ý trong s3 api tại cmp


**RBD**

- rbd-nbd: rbd device attach and rbd device detach commands added, these allow for safe reattach after rbd-nbd daemon is restarted since Linux kernel 5.14. -> có thể sử dụng lênh detach và attach 

- rbd-nbd: notrim map option added to support thick-provisioned images, similar to krbd.

- Large stabilization effort for client-side persistent caching on SSD devices, also available in 16.2.8. For details on usage, see:

https://docs.ceph.com/en/quincy/rbd/rbd-persistent-write-log-cache/

- Several bug fixes in diff calculation when using fast-diff image feature + whole object (inexact) mode. In some rare cases these long-standing issues could cause an incorrect rbd export. Also fixed in 15.2.16 and 16.2.8.

- Fix for a potential performance degradation when running Windows VMs on krbd. For details, see rxbounce map option description:

https://docs.ceph.com/en/quincy/man/8/rbd/#kernel-rbd-krbd-options


Sau khi đọc qua 1 lượt em có chọn được vài phiên bản mà ceph recommend cho user

- V17.2.7 QUINCY - jammy, focal https://lists.ceph.io/hyperkitty/list/ceph-users@ceph.io/message/NIALMLMGVQVZW5INEABS6W4LDIQYI3UC/

- V17.2.6 QUINCY - không có thay đổi nào đáng chú ý. -focal

- V17.2.5 QUINCY - không có thay đổi nào đáng chú ý.

- V17.2.4 QUINCY - không có thay đổi nào đáng chú ý.

check có thay đổi gì về nội dung API rgw không
https://github.com/ceph/ceph/pull/47234/files





