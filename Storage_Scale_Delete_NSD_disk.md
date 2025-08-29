# Storage Scale Delete NSD disk after mount filesystem 

## Step <br>
**1.Check**
On GPFS (Spectrum Node1) check current NSD <br>
```bash
mmlsnsd -X
df -h
mmlsdisk <filesystem>
```

**2.Delete disk** <br>
```bash
mmdeldisk <filesystem> <nsd_name>
```

**3.Restripe** <br>
```bash
mmrestripefs <filesystem> -b
```

**4.Check size** <br>
```bash
mmdf <filesystem> --block-size T
```
