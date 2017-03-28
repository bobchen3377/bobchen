---
layout: post
title: Java Files Merge
date: 2017-02-05
category: Java
---

### 序言

有时候在实际项目中，在处理较大的文件时，常常将文件拆分为多个子文件进行处理，最后再合并这些子文件。

Java中合并子文件可以利用BufferedStream和nio FileChannel进行读写。  

### 1. BufferedStream

BufferedStream的合并操作是一个循环读取子文件内容然后复制写入最终文件的过程，此过程会从文件系统中读取数据到内存中，之后再写入文件系统，比较低效。

Sample:
```
并多个文件
public static boolean mergeFiles(String[] fpaths, String resultPath) {
    if (fpaths == null || fpaths.length < 1 || TextUtils.isEmpty(resultPath)) {
        return false;
    }
    if (fpaths.length == 1) {
        return new File(fpaths[0]).renameTo(new File(resultPath));
    }

    File[] files = new File[fpaths.length];
    for (int i = 0; i < fpaths.length; i ++) {
        files[i] = new File(fpaths[i]);
        if (TextUtils.isEmpty(fpaths[i]) || !files[i].exists() || !files[i].isFile()) {
            return false;
        }
    }

    File resultFile = new File(resultPath);

    try {
        int bufSize = 1024;
        BufferedOutputStream outputStream = new BufferedOutputStream(new FileOutputStream(resultFile));
        byte[] buffer = new byte[bufSize];

        for (int i = 0; i < fpaths.length; i ++) {
            BufferedInputStream inputStream = new BufferedInputStream(new FileInputStream(files[i]));
            int readcount;
            while ((readcount = inputStream.read(buffer)) > 0) {
                outputStream.write(buffer, 0, readcount);
            }
            inputStream.close();
        }
        outputStream.close();
    } catch (FileNotFoundException e) {
        e.printStackTrace();
        return false;
    } catch (IOException e) {
        e.printStackTrace();
        return false;
    }

    for (int i = 0; i < fpaths.length; i ++) {
        files[i].delete();
    }

    return true;
}
```

---

### 2. nio FileChannel  
一种更高效的合并方式是利用Java nio库中FileChannel类的transferTo方法进行合并。此方法可以利用很多操作系统直接从文件缓存传输字节的能力来优化传输速度。

Sample:
```
public static boolean mergeFiles(String[] fpaths, String resultPath) {
    if (fpaths == null || fpaths.length < 1 || TextUtils.isEmpty(resultPath)) {
        return false;
    }
    if (fpaths.length == 1) {
        return new File(fpaths[0]).renameTo(new File(resultPath));
    }

    File[] files = new File[fpaths.length];
    for (int i = 0; i < fpaths.length; i ++) {
        files[i] = new File(fpaths[i]);
        if (TextUtils.isEmpty(fpaths[i]) || !files[i].exists() || !files[i].isFile()) {
            return false;
        }
    }

    File resultFile = new File(resultPath);

    try {
        FileChannel resultFileChannel = new FileOutputStream(resultFile, true).getChannel();
        for (int i = 0; i < fpaths.length; i ++) {
            FileChannel blk = new FileInputStream(files[i]).getChannel();
            resultFileChannel.transferFrom(blk, resultFileChannel.size(), blk.size());
            blk.close();
        }
        resultFileChannel.close();
    } catch (FileNotFoundException e) {
        e.printStackTrace();
        return false;
    } catch (IOException e) {
        e.printStackTrace();
        return false;
    }

    for (int i = 0; i < fpaths.length; i ++) {
        files[i].delete();
    }

    return true;
}
```