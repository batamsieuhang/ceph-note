## Config Multi-site

### Install cephadm
https://docs.ceph.com/en/quincy/install/get-packages/
https://download.ceph.com/debian-16.2.5/dists/focal/

### Deloy Ceph Cluster

Install ceph single cluster

cephadm --image quay.io/ceph/ceph:v16.2.5 bootstrap --mon-ip 10.1.0.46 --single-host-defaults --skip-monitoring-stack --skip-dashboard

### Edit ceph crush map

ceph osd getcrushmap -o compile

crushtool -d compile -o de_com

crushtool -c de_com -o compile

ceph osd setcrushmap -i compile
"5d2255aa-72e1-4fle-af99-85683f36cb34.1157817345.6",


# Ceph multi-site


**Reef**

radosgw-admin realm create --rgw-realm=realm-vn --default


create master zonegroup
```cmd
radosgw-admin zonegroup create --rgw-zonegroup={name} --endpoints={url} [--rgw-realm={realm-name}|--realm-id={realm-id}] --master --default

radosgw-admin zonegroup create --rgw-zonegroup=hn --endpoints=http://10.0.0.3:8888 --rgw-realm=realm-vn --master --default
```

create master zone

radosgw-admin zone create --rgw-zonegroup=hn \
                            --rgw-zone=hn-01 \
                            --master --default \
                            --endpoints=http://10.0.0.3:8888


Create system user

```cmd
radosgw-admin user create --uid="admin" --display-name="Admin Object Storage" --system


{
    "user_id": "admin",
    "display_name": "Admin Object Storage",
    "email": "",
    "suspended": 0,
    "max_buckets": 1000,
    "subusers": [],
    "keys": [
        {
            "user": "admin",
            "access_key": "PM59S6AZFHE3KJ3BQFY2",
            "secret_key": "sbdExwqSX5HDCl6teWj2gm3pTBDANIkl6lXw69ww"
        }
    ],
    "swift_keys": [],
    "caps": [],
    "op_mask": "read, write, delete",
    "system": true,
    "default_placement": "",
    "default_storage_class": "",
    "placement_tags": [],
    "bucket_quota": {
        "enabled": false,
        "check_on_raw": false,
        "max_size": -1,
        "max_size_kb": 0,
        "max_objects": -1
    },
    "user_quota": {
        "enabled": false,
        "check_on_raw": false,
        "max_size": -1,
        "max_size_kb": 0,
        "max_objects": -1
    },
    "temp_url_keys": [],
    "type": "rgw",
    "mfa_ids": []
}

```

create rgw endpoint
```cmd
ceph orch apply rgw *<name>* [--realm=*<realm-name>*] [--zone=*<zone-name>*] --placement="*<num-daemons>* [*<host1>* ...]"


ceph orch apply rgw rgw.01 --realm=realm-01 --zone=hn-01 --placement="1 ceph-node2-ver18" --port=8888
```

commit period multi-site

```cmd
radosgw-admin period update --commit

{
    "id": "f0d5f06a-0fd9-4189-8093-ca465f612bc2",
    "epoch": 1,
    "predecessor_uuid": "d0920136-0b65-4c13-ae1b-c9fa69eb881b",
    "sync_status": [],
    "period_map": {
        "id": "f0d5f06a-0fd9-4189-8093-ca465f612bc2",
        "zonegroups": [
            {
                "id": "a428eaf3-0c17-4051-bb22-04ef77fd1f27",
                "name": "hn",
                "api_name": "hn",
                "is_master": true,
                "endpoints": [
                    "http://10.0.0.3:8888"
                ],
                "hostnames": [],
                "hostnames_s3website": [],
                "master_zone": "1461bca1-31ad-4a5b-84cf-4d0722823999",
                "zones": [
                    {
                        "id": "1461bca1-31ad-4a5b-84cf-4d0722823999",
                        "name": "hn-01",
                        "endpoints": [
                            "http://10.0.0.3:8888"
                        ],
                        "log_meta": false,
                        "log_data": false,
                        "bucket_index_max_shards": 11,
                        "read_only": false,
                        "tier_type": "",
                        "sync_from_all": true,
                        "sync_from": [],
                        "redirect_zone": "",
                        "supported_features": [
                            "compress-encrypted",
                            "resharding"
                        ]
                    }
                ],
                "placement_targets": [
                    {
                        "name": "default-placement",
                        "tags": [],
                        "storage_classes": [
                            "STANDARD"
                        ]
                    }
                ],
                "default_placement": "default-placement",
                "realm_id": "eee9e882-1abe-4883-bd2a-4d6c958b9991",
                "sync_policy": {
                    "groups": []
                },
                "enabled_features": [
                    "resharding"
                ]
            }
        ],
        "short_zone_ids": [
            {
                "key": "1461bca1-31ad-4a5b-84cf-4d0722823999",
                "val": 3556075099
            }
        ]
    },
    "master_zonegroup": "a428eaf3-0c17-4051-bb22-04ef77fd1f27",
    "master_zone": "1461bca1-31ad-4a5b-84cf-4d0722823999",
    "period_config": {
        "bucket_quota": {
            "enabled": false,
            "check_on_raw": false,
            "max_size": -1,
            "max_size_kb": 0,
            "max_objects": -1
        },
        "user_quota": {
            "enabled": false,
            "check_on_raw": false,
            "max_size": -1,
            "max_size_kb": 0,
            "max_objects": -1
        },
        "user_ratelimit": {
            "max_read_ops": 0,
            "max_write_ops": 0,
            "max_read_bytes": 0,
            "max_write_bytes": 0,
            "enabled": false
        },
        "bucket_ratelimit": {
            "max_read_ops": 0,
            "max_write_ops": 0,
            "max_read_bytes": 0,
            "max_write_bytes": 0,
            "enabled": false
        },
        "anonymous_ratelimit": {
            "max_read_ops": 0,
            "max_write_ops": 0,
            "max_read_bytes": 0,
            "max_write_bytes": 0,
            "enabled": false
        }
    },
    "realm_id": "eee9e882-1abe-4883-bd2a-4d6c958b9991",
    "realm_name": "realm-vn",
    "realm_epoch": 2
}
```

**Configure secondary zone**

pull realm
```cmd
radosgw-admin realm pull --url=http://10.0.0.3:8888
--access-key=PM59S6AZFHE3KJ3BQFY2 --secret=sbdExwqSX5HDCl6teWj2gm3pTBDANIkl6lXw69ww
```

make realm default
```cmd
radosgw-admin realm default --rgw-realm=realm-vn
```

create secondary zone
```cmd
radosgw-admin zone create --rgw-zonegroup=hn --rgw-zone=hn-02 \
                             --access-key=PM59S6AZFHE3KJ3BQFY2 --secret=sbdExwqSX5HDCl6teWj2gm3pTBDANIkl6lXw69ww \
                             --endpoints=http://10.0.0.2:8888
```

create rgw in zone 2
```cmd
ceph orch apply rgw rgw.02 --realm=realm-vn --zone=hn-02 --placement="1 ceph-node1-ver16" --port=8888
```


bucket:
- được tạo ở cả 2 zone, đồng thời sync metadata
- vẫn có thể tạo lại bucket 
- tắt sync của object -> chỉ có bucket nào được upload mới có object
- Việc tạo bucket chỉ có thể tạo được ở master zone, user chắc cũng thế


