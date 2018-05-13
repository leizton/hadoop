#
mvn clean compile -Dskiptest=true

# start job
job.waitForCompletion()
  submit()
    connect()  //连接JobTracker, 创建cluster
      cluster = Cluster.new  //create client:ClientProtocol
    submitter = JobSubmitter.new(cluster.fileSystem, cluster.client)
      submitter.submitClient = cluster.client
    submitter.submitJobInternal(job, cluster)
      maps:int = writeSplits()  // mapperNum
        input = ReflectionUtils.newInstance(job.getInputFormatClass, job.conf)
        splits = input.getSplits()
        => splits.size()
      writeConf()  //把job.conf写到fileSystem的新文件里, 变成全局配置
      submitClient.submitJob()