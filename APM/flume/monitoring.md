# Monitoring

官方文档：[https://flume.apache.org/releases/content/1.9.0/FlumeUserGuide.html#monitoring](https://flume.apache.org/releases/content/1.9.0/FlumeUserGuide.html#monitoring)

Flume的监控使用的JMX：

> Several Flume components report metrics to the JMX platform MBean server. These metrics can be queried using Jconsole.

如果自定义组件要支持指标上报的话，那么必须继承自：`org.apache.flume.instrumentation.MonitoredCounterGroup`。
