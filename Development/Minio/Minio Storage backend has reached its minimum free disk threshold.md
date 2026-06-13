I am using MinIO as the storage backend for Loki, but after a few days, the Loki writer shows an error: "Storage backend has reached its minimum free disk threshold." However, when I check the Persistent Volume Claim (PVC) for the MinIO storage, it only shows 60% usage. I'm not sure why Loki is unable to write logs to MinIO.

# File Type

By using `df -Th` we can see the file type, it’s `ext4`
**ext4** has a feature called `dir_index` enabled by default, which is quite susceptible to hash-collisions
We can run `tune2fs -l /dev/vdc` to get more detail about the disk.
# Check Whether we have many files

Even if your disk shows ample available space, excessive file counts can still trigger issues. While ext4 supports a vast number of files per filesystem (each consuming an inode), inherent inode limits exist. Verify this by checking inode usage: Just use `df -i`:
# Do a file system check

Some people on the internet have reported experiencing a similar issue after a system crash. Coincidentally, my system crashed just before this problem occurred (see the comments above about offlineimap). They suggest that running a file system check resolved the issue, so it's worth doing that. If you're unable to unmount the filesystem—such as when it's mounted at the root directory—it's advisable to perform the check from a live system.

# Do you have `dir_index` enabled?

So this is the punch line: ext4 has the possibility to hash the filenames of its contents. This enhances performance, but has a “small” problem: ext4 does not grow its hashtable, when it starts to fill up. Instead it returns -ENOSPC or “no space left on device”.

ext4 uses `half_md4` as a default hashing-mechanism. If I interpret my google-results correctly, this uses the md4-hash algorithm, but strips it to 32 bits. This is a classical example of the [birthday-paradox](https://en.wikipedia.org/wiki/Birthday_problem): A 32 bit hash means, that there are 4294967296 different hash values available, so if we are fair and assume a uniform distribution of hash values, that makes it highly unlikely to hit one specific hash. But the probability of hitting two identical hashes, given enough filenames, is much much higher. Using the [formula](https://en.wikipedia.org/wiki/Birthday_problem#Cast_as_a_collision_problem) from Wikipedia we get (with about 50K files) a probability of about 25% that a newly added file has the same hash. This is a huge probability of failure. If on the other hand we take a 64bit hash-function the probability becomes much smaller, about 0.00000000007%.

So if you have a lot of files in the same directory, you probably want to switch off `dir_index`, or at least change to a different hash-algorithm. You can check if you have `dir_index` enabled and change the hash, like this:

```
root@server ~$ sudo tune2fs -l /dev/vdc | grep -o dir_index  
dir_index
```

# Change the hash-algo to a bigger one  

```
root@server ~$ sudo tune2fs -E "hash_alg=tea" /dev/vdc  
```

# Disable it completely  

```
root@server ~$ sudo tune2fs -O "^dir_index"
```

Note however, that `dir_index` and `half_md4` where choices made for performance reasons. So you might experience a performance-hit after this.

After trying it out, I realized, that the problem actually also persists with the tea-hash. I then had a look at [the ext4-documentation](https://ext4.wiki.kernel.org/index.php/Ext4_Disk_Layout#Hash_Tree_Directories) about the topic and it seems, that the hash is only stored as 32 bits, so it actually does not matter what hash we choose, regarding this particular problem. So if `half_md4` is chosen [because of its better performance and collision-resistance](http://git.whamcloud.com/?p=tools%2Fe2fsprogs.git%3Ba%3Dcommitdiff_plain%3Bh%3Dd1070d91b4de8438dc78c034283baaa19b31d25e) it actually makes sense to leave it as the default. You can by the way easily test and reproduce the issue by using the following on an ext4 file system:

```
for a in `seq 100000`  
do  
        file=`head -c 51 /dev/urandom | base64 | tr '/' '_'`  
        touch $file  
done
```

Curiously, this only gives me about 160 collisions on 100K files (instead of about 10K collisions on 60K files), which would suggest, that my original sample (meaning my mailbox) exhibits some properties that make collisions more likely both on `half_md4` _and_ `tea`.

# Manually Remove Files

download mc cli
```
 wget https://dl.min.io/client/mc/release/linux-amd64/mc  
/ # chmod +x mc
```

Configure alias to access the MinIO server endpoint. Credentials of a user with admin access to the MinIO object storage service is required.

```
mc alias set grafana_loki_minio http://minio.monitor:9000
```

List the alias configured and confirm that the grafana-loki-minio storage is configured correctly:

```
./mc alias list
```

Before deleting objects is always a good idea to perform a dry-run test to confirm that desired objects will be deleted:

```
./mc rm --dry-run --recursive --force --older-than 7d0h0s grafana_loki_minio/loki/fake
```

Objects deletion (in this case objects older than 7days will be deleted):

```
./mc rm --recursive --force --older-than 7d0h0s grafana_loki_minio/loki/fake
```
